# MinimalAccount

This contract is a minimal implementation of **Account Abstraction (EIP-4337)** using the `IAccount` interface.  
It represents a simple account where only the **owner** is authorized to sign and execute transactions.

---

## Features
- Restricted access: only the EntryPoint or the owner can call sensitive methods.
- Supports `execute` for arbitrary external calls.
- UserOperation validation using the account owner’s signature.
- Handles prefunding of gas if required.

---

## Contract Overview
- **constructor**: Stores the `EntryPoint` address and sets the contract owner as the initial `msg.sender`.
- **execute**: Can be called by EntryPoint or the owner to forward calls to any destination.
- **validateUserOp**: Called by EntryPoint to validate a UserOperation. Includes:
  - Signature check via `_validateSignature`
  - Prefund management if extra gas is required
- **_validateSignature**: Ensures the signature matches the contract owner.
- **_payPrefund**: Transfers required prefund to EntryPoint if necessary.
- **getEntryPoint**: Returns the stored EntryPoint address.

---

## Errors
- `MinimalAccount__NotFromEntryPoint`: Thrown if caller is not EntryPoint.
- `MinimalAccount__NotFromEntryPointOrOwner`: Thrown if caller is neither EntryPoint nor owner.
- `MinimalAccount__CallFailed`: Thrown if `execute` call fails.

---

## Flow
1. A user creates a **UserOperation** (with destination, transaction data, and signature).
2. The **Bundler** collects and sends UserOperations to the **EntryPoint** via `handleOps`.
3. EntryPoint calls `validateUserOp` on the account contract to validate the operation.
4. The contract checks the signature against its owner.
5. If valid, EntryPoint marks the operation as valid.
6. Finally, EntryPoint calls `execute` on the account contract to perform the actual transaction.

---

## License
This project is licensed under the MIT License.

