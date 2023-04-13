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
