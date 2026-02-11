# Security Advisory: CPI Return Data Spoofing in Anchor's `Return<T>::get()`

## Summary

| Field | Value |
|-------|-------|
| **Severity** | HIGH |
| **Affected Component** | `anchor-lang` codegen — CPI `Return<T>` struct |
| **Affected Files** | `lang/syn/src/codegen/program/cpi.rs`, `lang/attribute/program/src/declare_program/mods/cpi.rs` |
| **Impact** | Any Anchor program using `Return<T>::get()` to read CPI return data trusts unvalidated data |
| **CVSSv3** | 7.5 (High) — AV:N/AC:L/PR:N/UI:N/S:U/C:N/I:H/A:N |

## Description

Anchor's `Return<T>::get()` method calls Solana's `get_return_data()` syscall to retrieve data returned by a CPI callee. The syscall returns a tuple `(Pubkey, Vec<u8>)` where the `Pubkey` identifies which program set the return data. However, Anchor's codegen **discards this program ID** (binding it to `_key`), deserializing the data without verifying its origin.

### Vulnerable Code (two locations)

**`lang/syn/src/codegen/program/cpi.rs:93-96`** (standard codegen):
```rust
impl<T: AnchorDeserialize> Return<T> {
    pub fn get(&self) -> T {
        let (_key, data) = anchor_lang::solana_program::program::get_return_data().unwrap();
        //   ^^^^ program_id DISCARDED — no validation
        T::try_from_slice(&data).unwrap()
    }
}
```

**`lang/attribute/program/src/declare_program/mods/cpi.rs:111-114`** (declare_program codegen):
```rust
impl<T: AnchorDeserialize> Return<T> {
    pub fn get(&self) -> T {
        let (_key, data) = anchor_lang::solana_program::program::get_return_data().unwrap();
        //   ^^^^ same pattern — program_id DISCARDED
        T::try_from_slice(&data).unwrap()
    }
}
```

### Correct Pattern (same repository)

The same Anchor repository implements the correct pattern in `spl/src/token_2022.rs:377-388`:

```rust
anchor_lang::solana_program::program::get_return_data()
    .ok_or(ProgramError::InvalidInstructionData)
    .and_then(|(key, data)| {
        if key != ctx.program_id {
            Err(ProgramError::IncorrectProgramId)  // Validates program_id!
        } else {
            data.try_into().map(u64::from_le_bytes).map_err(|_| {
                ProgramError::InvalidInstructionData
            })
        }
    })
```

This pattern is used in three separate functions in `token_2022.rs` (`get_account_data_size`, `amount_to_ui_amount`, `ui_amount_to_amount`), demonstrating that the team is aware program_id validation is necessary — but it was not applied to the generic `Return<T>` codegen.

## Attack Scenario

### How `Return<T>` Works

When an Anchor program does a CPI to a callee that returns a value:
1. The CPI method calls `invoke_signed()` and returns a `Return<T>` struct
2. The developer calls `result.get()` to read the callee's return value
3. `get()` calls `get_return_data()` which returns the **most recent** `set_return_data()` in the call chain

### The Exploit

`get_return_data()` returns data from whoever **last** called `set_return_data()` — this is global state, not scoped to a specific CPI. If any other CPI call occurs between the initial CPI and the `result.get()` call, the return data can be overwritten.

**Attack flow:**
1. Caller CPIs to Target program → gets `Return<u64>` (Target sets return data = 10)
2. Caller CPIs to another program (or an attacker-controlled program is invoked in the chain)
3. That program calls `set_return_data()` with malicious data (999)
4. Caller calls `result.get()` → reads 999 instead of 10
5. `Return<T>::get()` does NOT validate the program_id, so the spoofed data is accepted

### Real-World Impact

This affects any Anchor program that:
- Uses `Return<T>::get()` to read CPI return data
- Makes multiple CPI calls where return data from an earlier CPI is read after a later CPI
- Reads return data from programs that may internally CPI to other programs

Framework-level vulnerability: Anchor has 4,949 GitHub stars and is the dominant Solana development framework. Every Anchor program using CPI return values is potentially affected.

## Proof of Concept

The PoC consists of three programs:

### 1. Callee (legitimate program)
Returns `u64 = 10` via Anchor's standard return mechanism:
```rust
pub fn return_u64(_ctx: Context<CpiReturn>) -> Result<u64> {
    Ok(10)
}
```

### 2. Malicious program
Manually calls `set_return_data()` with a spoofed value:
```rust
pub fn spoof_return_data(_ctx: Context<SpoofReturn>) -> Result<()> {
    let spoofed_value: u64 = 999;
    anchor_lang::solana_program::program::set_return_data(&spoofed_value.to_le_bytes());
    Ok(())
}
```

### 3. Caller program (victim)
Makes two CPIs, then reads return data:
```rust
pub fn cpi_call_return_u64_spoofed(ctx: Context<SpoofedReturnContext>) -> Result<()> {
    // Step 1: CPI to callee → gets Return<u64> (callee sets return data = 10)
    let result = callee::cpi::return_u64(cpi_ctx)?;

    // Step 2: CPI to malicious → overwrites return data with 999
    malicious::cpi::spoof_return_data(spoof_ctx)?;

    // Step 3: Read "callee's" return data — but it's spoofed!
    let spoofed_value = result.get_unchecked(); // Returns 999, not 10
    Ok(())
}
```

### Test Results
```
VULNERABILITY CONFIRMED (get_unchecked / old behavior):
  Callee returned: 10
  Malicious spoofed: 999
  Caller received: 999 (SPOOFED!)

FIX CONFIRMED: get() rejected spoofed return data
  Error: Transaction simulation failed...
```

## Fix

### Changes

1. **Add `program_id` field to `Return<T>`** — stores the expected program ID from the CPI context
2. **Validate in `get()`** — compare `get_return_data()` key against stored `program_id`
3. **Add `get_unchecked()`** — backward-compatible escape hatch for intentional cross-program reads

### Diff (both codegen paths)

```diff
 pub struct Return<T> {
     phantom: std::marker::PhantomData<T>,
+    program_id: Pubkey,
 }

 impl<T: AnchorDeserialize> Return<T> {
     pub fn get(&self) -> T {
-        let (_key, data) = anchor_lang::solana_program::program::get_return_data().unwrap();
+        let (key, data) = anchor_lang::solana_program::program::get_return_data().unwrap();
+        if key != self.program_id {
+            panic!("CPI return data from unexpected program: expected {}, got {}", self.program_id, key);
+        }
         T::try_from_slice(&data).unwrap()
     }
+
+    pub fn get_unchecked(&self) -> T {
+        let (_key, data) = anchor_lang::solana_program::program::get_return_data().unwrap();
+        T::try_from_slice(&data).unwrap()
+    }
 }

 // Constructor now includes program_id:
-Ok(Return::<T> { phantom: PhantomData })
+Ok(Return::<T> { phantom: PhantomData, program_id: ctx.program_id })
```

## Recommendation

1. Apply the fix to both codegen paths (`cpi.rs` and `declare_program/mods/cpi.rs`)
2. Consider returning `Result<T>` instead of panicking in `get()` for cleaner error handling
3. Audit existing Anchor programs that use `Return<T>::get()` for potential exposure
4. Add this to the Anchor security documentation as a known pattern
