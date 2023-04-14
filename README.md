## Stake Manager contract

| Function Name | Function Argument | Visibility | Return Type | One Line Description |
|---------------|-------------------|------------|-------------|----------------------|
| getDepositInfo | address account | external | DepositInfo | Returns the full deposit information of the given account |
| balanceOf | address account | external | uint256 | Returns the deposit (for gas payment) of the account |
| depositTo | address account | external | payable | Adds to the deposit of the given account |
| addStake | uint32 _unstakeDelaySec | external | payable | Adds to the account's stake - amount and delay |
| unlockStake | N/A | external | N/A | Attempts to unlock the stake |
| withdrawStake | address payable withdrawAddress | external | N/A | Withdraws from the (unlocked) stake |
| withdrawTo | address payable withdrawAddress, uint256 withdrawAmount | external | N/A | Withdraws from the deposit |

----------------------------------
## Sender Creator contract

| Function Name | Function Argument | Visibility | Return Type | One-line Description |
|---------------|-------------------|------------|-------------|----------------------|
| createSender  | bytes calldata initCode | external | address | Call the "initCode" factory to create and return the sender account address. |

-------------------

## Nonce Manager contract

Return the next nonce for this sender.

* Within a given key, the nonce values are sequenced (starting with zero, and incremented by one on each userop)
* But UserOp with different keys can come with arbitrary order.

| Function Name         | Function Argument                | Visibility  | Return Type | One-line Working                                                                                                          |
|-----------------------|----------------------------------|-------------|-------------|---------------------------------------------------------------------------------------------------------------------------|
| getNonce              | address sender, uint192 key      | public view | uint256     | Returns the next valid sequence number for a given nonce key by bitwise ORing the value of nonceSequenceNumber[sender][key] with the left-shifted value of key by 64. |
| incrementNonce        | uint192 key                      | public      | N/A         | Allows an account to manually increment its own nonce by incrementing the value of nonceSequenceNumber[msg.sender][key] by 1. |
| _validateAndUpdateNonce | address sender, uint256 nonce   | internal    | bool        | Validates and updates nonce uniqueness for the given account by checking if nonceSequenceNumber[sender][key] incremented by 1 is equal to the value of seq, which is extracted from nonce by bitwise ANDing it with 64 bits and converting it to uint64. Returns true if validation is successful. |


---------------------------
## Base Account contract

| Function Name | Function Argument | Visibility | Return Type | Working |
|---------------|-------------------|------------|-------------|---------|
| nonce()       | None              | public     | uint256     | Returns the account nonce value. |
| entryPoint()  | None              | public     | IEntryPoint | Returns the current entryPoint used by this account. |
| validateUserOp() | UserOperation calldata userOp, bytes32 userOpHash, uint256 missingAccountFunds | external | uint256 | Validates user's signature and nonce, and updates the account state to prevent replay attacks. Also sends missing funds to the entrypoint. |
| _requireFromEntryPoint() | None | internal | None | Ensures that the request comes from the known entrypoint. |
| _validateSignature() | UserOperation calldata userOp, bytes32 userOpHash | internal | uint256 | Validates the signature of the userOp against the provided userOpHash and returns validation data. |
| _validateAndUpdateNonce() | UserOperation calldata userOp | internal | None | Validates that the current nonce matches the UserOperation nonce, and updates the account's state to prevent replay attacks. |
| _payPrefund() | uint256 missingAccountFunds | internal | None | Sends missing funds to the entrypoint (msg.sender) for the current transaction. |


--------------------------------------------------------
## paymaster Contract

| Function Name | Function Argument | Visibility | Return Type | Working |
|---------------|-------------------|------------|-------------|---------|
| validatePaymasterUserOp | UserOperation calldata userOp, bytes32 userOpHash, uint256 maxCost | external | bytes memory | This function is used for payment validation. It takes in user operation details, including the user operation data, its hash, and the maximum cost of the transaction (based on gas and gas price from userOp). It returns a context value to be sent to the postOp function, and a validationData which includes the signature and time-range of the operation. |
| postOp | PostOpMode mode, bytes calldata context, uint256 actualGasCost | external | N/A | This function is the post-operation handler. It takes in the mode which can be opSucceeded, opReverted, or postOpReverted, the context value returned by validatePaymasterUserOp, and the actual gas cost used so far. It handles the post-operation logic based on the mode received. |


