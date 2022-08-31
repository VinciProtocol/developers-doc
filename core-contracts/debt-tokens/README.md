# Debt Tokens

Debt tokens are interest-accruing tokens that are minted and burned on [`borrow`](../lendingpool/#borrow) and [`repay`](../lendingpool/#repay), representing the debt owed by the token holder. Currently, there is only one type of debt tokens:

* Variable debt tokens, representing a debt to the protocol with a variable interest rate

The source code can be found [on Github here](https://github.com/VinciProtocol/vinci-protocol/tree/master/contracts/protocol/tokenization).

## EIP20 Methods

Although debt tokens are modelled on the ERC20/EIP20 standard, they are non-transferrable. Therefore they do not implement any of the standard ERC20/EIP20 functions relating to `transfer()` and `allowance()`.

{% hint style="info" %}
`balanceOf()` will always return the most up to date accumulated debt of the user.
{% endhint %}

{% hint style="info" %}
`totalSupply()` will always return the most up to date total debt accrued by all protocol users for _that specific type (variable) of debt token_.
{% endhint %}

## Shared Methods

### UNDERLYING\_ASSET\_ADDRESS()

**`function UNDERLYING_ASSET_ADDRESS()`**

Returns the underlying asset of the debt token.

### POOL()

**`function POOL()`**

Returns the address of the associated [`LendingPool`](../lendingpool/) for the debt token.

### approveDelegation**()**

**`function approveDelegation(address delegatee, uint256 amount)`**

Sets the `amount` of allowance for `delegatee` to borrow of a particular debt token.

| Parameter Name | Type      | Description                           |
| -------------- | --------- | ------------------------------------- |
| `delegatee`    | address   | the user receiving the allowance      |
| `amount`       | `uint256` | the allowance amounts given to `user` |

### borrowAllowance**()**

**`function borrowAllowance(address fromUser, address toUser)`**

Returns the borrow allowance `toUser` has been given by `fromUser`.

| Parameter Name | Type    | Description                      |
| -------------- | ------- | -------------------------------- |
| `fromUser`     | address | the user giving allowance        |
| `toUser`       | address | the user receiving the allowance |

Returns the current allowance of `toUser` for a particular debt token.

## Variable Debt Methods

### scaledBalanceOf**()**

**`function scaledBalanceOf(address user)`**

Returns the principal debt balance of `user`.

### scaledTotalSupply**()**

**`function scaledTotalSupply()`**

Returns the scaled total supply of the variable debt token.&#x20;

This represents $$sum( borrows  /  index )$$ .

### getScaledUserBalanceAndSupply**()**

**`function getScaledUserBalanceAndSupply(address user)`**

Returns the principal balance of the `user` and principal total supply.

#### Return values

| Type    | Description               |
| ------- | ------------------------- |
| uint256 | principal balance of user |
| uint256 | principal total supply    |

