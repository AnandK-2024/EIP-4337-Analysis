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


