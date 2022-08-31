# LendingPool

The `LendingPool` contract is the main contract of the protocol. It exposes all the user-oriented actions that can be invoked using either Solidity or web3 libraries.

The source code can be found on [Github here](https://github.com/VinciProtocol/vinci-protocol/blob/master/contracts/protocol/lendingpool/LendingPool.sol).

{% hint style="warning" %}
`LendingPool` methods**`deposit, borrow, withdraw and repay`**are only for ERC20, if you want to deposit, borrow, withdraw or repay using native ETH (or native MATIC incase of Polygon), use [`WETHGateway`](../weth-gateway.md) instead.
{% endhint %}

## Methods

### **deposit()**

**`function deposit(address asset, uint256 amount, address onBehalfOf, uint16 referralCode)`**

Deposits a certain `amount` of an `asset` into the protocol, minting the same `amount` of corresponding vTokens, and transferring them to the `onBehalfOf` address.

{% hint style="danger" %}
The referral program is currently inactive and you can pass`0` as the`referralCode.`

In future for referral code to be active again, a governance proposal, with the list of unique referral codes for various integration must be passed via governance.
{% endhint %}

{% hint style="warning" %}
When depositing, the `LendingPool` contract must have**`allowance()`**to spend funds on behalf of**`msg.sender`** for at-least**`amount`** for the **`asset`** being deposited. This can be done via the standard ERC20 `approve()` method.
{% endhint %}

| Parameter Name | Type    | Description                                                                                                                  |
| -------------- | ------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `asset`        | address | address of the underlying asset                      |
| `amount`       | uint256 | amount deposited, expressed in wei units                                                                                     |
| `onBehalfOf`   | address | <p>address whom will receive the vTokens. <br>Use <code>msg.sender</code> when the vTokens should be sent to the caller.</p> |
| `referralCode` | uint16  | referral code for our referral program. Use 0 for no referral.                                           |

### **withdraw()**

**`function withdraw(address asset, uint256 amount, address to)`**

Withdraws `amount` of the underlying `asset`, i.e. redeems the underlying token and burns the vTokens.

{% hint style="warning" %}
When withdrawing `to`another address,**`msg.sender`**should have`vToken`that will be burned by lendingPool .
{% endhint %}

| Parameter Name | Type    | Description                                                                                                          |
| -------------- | ------- | -------------------------------------------------------------------------------------------------------------------- |
| `asset`        | address | address of the underlying asset, not the vToken   |
| `amount`       | uint256 | <p>amount deposited, expressed in wei units. <br>Use <code>type(uint).max</code> to withdraw the entire balance.</p> |
| `to`           | address | address that will receive the `asset`                                                                                |


### **depositNFT()**

**`function depositNFT(address nft, uint256[] tokenIds, uint256[] amounts, address onBehalfOf, uint16 referralCode)`**

Deposits NFTs with given `tokenIds` and `amounts` into the protocol, minting the corresponding nTokens, and transferring them to the `onBehalfOf` address.

{% hint style="danger" %}
The referral program is currently inactive and you can pass`0` as the`referralCode.`

In future for referral code to be active again, a governance proposal, with the list of unique referral codes for various integration must be passed via governance.
{% endhint %}

{% hint style="warning" %}
When depositing NFTs, the `LendingPool` contract must have**`getApproved()`**to transfering NFT on behalf of**`msg.sender`** for each token Id in **`tokenIds`** for the **`nft`** being deposited. This can be done via the standard ERC721 `approve()` method.
{% endhint %}

| Parameter Name | Type      | Description                                                                                                                  |
| -------------- | --------- | ---------------------------------------------------------------------------------------------------------------------------- |
| `asset`        | address   | address of the underlying asset                      |
| `tokenIds`     | uint256[] | The list of token ids to be   deposited.                   |
| `amounts`      | uint256[] | The amounts of tokens to be deposited for each token id. All elements must be 1 for an ERC721 NFT collection.                |
| `onBehalfOf`   | address   | <p>address whom will receive the nTokens. <br>Use <code>msg.sender</code> when the nTokens should be sent to the caller.</p> |
| `referralCode` | uint16    | referral code for our referral program. Use 0 for no referral.                                           |

### **withdrawNFT()**

**`function withdrawNFT(address nft, uint256[] tokenIds, uint256[] amounts, address to)`**

Withdraws the underlying `nft`corresponding to the `tokenids` and `amounts`, i.e. redeems the underlying tokens and burns the nTokens.

{% hint style="warning" %}
When withdrawing `to` another address, **`msg.sender`** should have `nToken` that will be burned by lendingPool .
{% endhint %}

| Parameter Name | Type    | Description                                                                                                          |
| -------------- | ------- | -------------------------------------------------------------------------------------------------------------------- |
| `nft`          | address | address of the underlying NFT, not the nToken   |
| `tokenIds`     | uint256[] | The list of token ids to be   deposited.                   |
| `amounts`      | uint256[] | The amounts of tokens to be deposited for each token id. All elements must be 1 for an ERC721 NFT collection.                |
| `tokenIds`     | uint256[] | The list of token ids to be   deposited.                   |
| `amounts`      | uint256[] | The amounts of tokens to be deposited for each token id. All elements must be 1 for an ERC721 NFT collection.                |
| `to`           | address | address that will receive the `nft`                                                                                |


### borrow**()**

**`function borrow(address asset, uint256 amount, uint256 interestRateMode, uint16 referralCode, address onBehalfOf)`**

Borrows `amount` of `asset` with `interestRateMode`, sending the `amount` to `msg.sender`, with the debt being incurred by `onBehalfOf`.

Note: `onBehalfOf` must have enough collateral via [`depositNFT()`](./#depositNFT) or have delegated credit to `msg.sender` via [`approveDelegation()`](../debt-tokens/#approvedelegation).

| Parameter Name     | Type    | Description                                                                                                                       |
| ------------------ | ------- | --------------------------------------------------------------------------------------------------------------------------------- |
| `asset`            | address | address of the underlying asset                                |
| `amount`           | uint256 | amount to be borrowed, expressed in wei units                                                                                     |
| `interestRateMode` | uint256 | <p>the type of borrow debt.</p><p>Currently unused, must be > 0</p>                                                                      |
| `referralCode`     | uint16  | referral code for our referral program. Use 0 for no referral code.                                           |
| `onBehalfOf`       | address | <p>address of user who will incur the debt.</p><p>Use <code>msg.sender</code> when not calling on behalf of a different user.</p> |

### repay**()**

**`function repay(address asset, uint256 amount, uint256 rateMode, address onBehalfOf)`**

Repays `onBehalfOf`'s debt `amount` of `asset` which has a `rateMode`.

| Parameter Name | Type    | Description                                                                                                                                                                                                                                                                                                                                                    |
| -------------- | ------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `asset`        | address | address of the underlying asset.md#supported-assets)                                                                                                                                                                                                                                                             |
| `amount`       | uint256 | <p>amount to be borrowed, expressed in wei units.</p><p>Use <code>uint(-1)</code> to repay the entire debt,  <strong>ONLY</strong> when the repayment is not executed on behalf of a 3rd party. </p><p>In case of repayments on behalf of another user, it's recommended to send an <code>_amount</code> slightly higher than the current borrowed amount.</p> |
| `rateMode`     | uint256 | <p>the type of borrow debt.</p><p>Currently unused, must be > 0.</p>                                                                                                                                                                                                                                                                                                   |
| `onBehalfOf`   | address | <p>address of user who will incur the debt.</p><p>Use <code>msg.sender</code> when not calling on behalf of a different user.</p>                                                                                                                                                                                                                              |

### nftLiquidationCall**()**

**`function nftLiquidationCall(address collateral, address debt, address user, uint256[] tokenIds, uint256[] amounts, bool receiveNToken)`**

Liquidate positions with a **health factor** below 1.

When the health factor of a position is below 1, liquidators repay part or all of the outstanding borrowed amount on behalf of the borrower, while **receiving a discounted amount of collateral** in return (also known as a liquidation 'bonus"). Liquidators can decide if they want to receive an equivalent amount of collateral nTokens, or the underlying NFT directly. When the liquidation is completed successfully, the health factor of the position is increased, bringing the health factor above 1.

{% hint style="info" %}
Liquidators can only close a certain amount of collateral defined by a close factor. Currently the **close factor is 0.5**. In other words, liquidators can only liquidate collateral that is worth a maximum of 50% of the amount pending to be repaid in a position. The liquidation discount applies to this amount. The liquidator uses `tokenIds` to select the NFTs he/she wishes to obtain. The NFT at the front of the `tokenIds` list will be acquired first when liquidated.
{% endhint %}

{% hint style="warning" %}
Liquidators must `approve()` the `LendingPool` contract to use as much of the underlying ERC20 as the sum of the value of all `tokensIds` used for the liquidation.
{% endhint %}

**NOTES**

* _In most scenarios_, profitable liquidators will choose to liquidate as much as they can (50% of the `user` position).
* To check a user's health factor, use [`getUserAccountData()`](./#getuseracountdata).

| Parameter Name  | Type    | Description                                                                                                                                   |
| --------------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------------- |
| `collateral`    | address | address of the collateral nft                                                                                                             |
| `debt`          | address | address of the underlying borrowed asset to be repaid                                                                                                                   |
| `user`          | address | address of the borrower                                                                                                                       |
| `tokenIds`     | uint256[] | The list of token ids to be obtained after the liquidation                   |
| `amounts`      | uint256[] | The amounts of tokens to be obtained for each token id. All elements must be 1 for an ERC721 NFT collection.                |
| `receiveNToken` | bool    | if `true`, the user receives the nTokens equivalent of the purchased collateral. If `false`, the user receives the underlying NFT directly. |

## View Methods

### getReserveData**()**

**`function getReserveData(address asset)`**

Returns the state and configuration of the reserve

| Parameter Name | Type    | Description            |
| -------------- | ------- | ---------------------- |
| `asset`        | address | address of the reserve |

#### Return values

| Parameter Name                | Type    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ----------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `configuration`               | uint256 | <p>bit 0-47: reserved</p><p>bit 48-55: Decimals</p><p>bit 56: reserve is active</p><p>bit 57: reserve is frozen</p><p>bit 58: borrowing is enabled</p><p>bit 59-63: reserved</p><p>bit 64-79: reserve factor</p><p></p><p>** <em>All % are 1e4 i.e. percentage plus two decimals</em></p><p>** <em>Decimals is 1e2</em> </p> |
| `liquidityIndex`              | uint128 | liquidity index in ray                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
| `variableBorrowIndex`         | uint128 | variable borrow index in ray                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
| `currentLiquidityRate`        | uint128 | current supply / liquidity / deposit rate in ray                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `currentVariableBorrowRate`   | uint128 | current variable borrow rate in ray                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
| `currentStableBorrowRate`     | uint128 | reserved                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      |
| `lastUpdateTimestamp`         | uint40  | timestamp of when reserve data was last updated                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        |
| `vTokenAddress`               | address | address of associated vToken (tokenised deposit)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `stableDebtTokenAddress`      | address | reserved                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `variableDebtTokenAddress`    | address | address of associated variable debt token                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| `interestRateStrategyAddress` | address | <p>address of interest rate strategy.</p>                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `id`                          | uint8   | the position in the list of active reserves                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |

### getNFTVaultData**()**

**`function getNFTVaultData(address asset)`**

Returns the state and configuration of the reserve

| Parameter Name | Type    | Description            |
| -------------- | ------- | ---------------------- |
| `asset`        | address | address of the NFT |

#### Return values

| Parameter Name                | Type    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| ----------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `configuration`               | uint256 | <p>bit 0-15: LTV</p><p>bit 16-31: Liq. threshold</p><p>bit 32-47: Liq. bonus</p><p>bit 48-55: Decimals</p><p>bit 56: vault is active</p><p>bit 57: vault is frozen</p><p>** <em>All % are 1e4 i.e. percentage plus two decimals</em></p><p>** <em>Decimals is 1e2</em> </p><p>** <em>Caveat on Liquidation bonus</em> <br><code>105% Liq Bonus = 100% principal + 5% bonus</code></p> |
| `nTokenAddress`               | address | address of associated nToken (tokenised vault)                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| `nftEligibility`              | address | address of the contract that checks the eligibility of the deposited NFTs                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
| `id`                          | uint8   | the position in the list of active reserves                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| `expiration`                  | uint40  | reserved                      |


### getUserAccountData**()**

**`function getUserAccountData(address user)`**

Returns the user account data across all the reserves

| Parameter Name | Type    | Description         |
| -------------- | ------- | ------------------- |
| `user`         | address | address of the user |

#### Return values

| Parameter Name                | Type    | Description                                                                                                                           |
| ----------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `totalCollateralETH`          | uint256 | total collateral in ETH of the use (wei decimal unit)                                                                                 |
| `totalDebtETH`                | uint256 | total debt in ETH of the user (wei decimal unit)                                                                                      |
| `availableBorrowsETH`         | uint256 | borrowing power left of the user (wei decimal unit)                                                                                   |
| `currentLiquidationThreshold` | uint256 | <p>liquidation threshold of the user<br>(1e4 format => percentage plus two decimals)</p>                                              |
| `ltv`                         | uint256 | <p>Loan To Value of the user<br>(1e4 format => percentage plus two decimals)</p>                                                      |
| `healthFactor`                | uint256 | <p>current health factor of the user.</p><p>Also see <a href="./#liquidationcall"><code>liquidationCall()</code></a><code></code></p> |

### getConfiguration**()**

**`function getConfiguration(address asset)`**

Returns the configuration of the reserve

| Parameter Name | Type    | Description            |
| -------------- | ------- | ---------------------- |
| `asset`        | address | address of the reserve |

#### Return values

| Return Type | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| uint256     | <p>bit 0-47: reserved</p><p>bit 48-55: Decimals</p><p>bit 56: reserve is active</p><p>bit 57: reserve is frozen</p><p>bit 58: borrowing is enabled</p><p>bit 59-63: reserved</p><p>bit 64-79: reserve factor</p><p></p><p>** <em>All % are 1e4 i.e. percentage plus two decimals</em></p><p>** <em>Decimals is 1e2</em> </p> |


### getNFTVaultConfiguration**()**

**`function getNFTVaultConfiguration(address asset)`**

Returns the configuration of the reserve

| Parameter Name | Type    | Description            |
| -------------- | ------- | ---------------------- |
| `asset`        | address | address of the NFT |

#### Return values

| Return Type | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| ----------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| uint256     | <p>bit 0-15: LTV</p><p>bit 16-31: Liq. threshold</p><p>bit 32-47: Liq. bonus</p><p>bit 48-55: Decimals</p><p>bit 56: vault is active</p><p>bit 57: vault is frozen</p><p>** <em>All % are 1e4 i.e. percentage plus two decimals</em></p><p>** <em>Decimals is 1e2</em> </p><p>** <em>Caveat on Liquidation bonus</em> <br><code>105% Liq Bonus = 100% principal + 5% bonus</code></p> |


### getUserConfiguration**()**

**`function getUserConfiguration(address user)`**

Returns the configuration of the user across all the reserves.

| Parameter Name | Type    | Description         |
| -------------- | ------- | ------------------- |
| `user`         | address | address of the user |

#### Return values

| Parameter Name | Return Type | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
| -------------- | ----------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| data | uint256     | <p>For ERC20 asset. The bitmask is divided into pairs of bits, one pair for each asset. </p><p></p><p>The first bit of the pair is reserved, the second bit indicates if it is being borrowed.<br><br>The corresponding assets are in the same position as <a href="./#getreserveslist"><code>getReservesList()</code></a><br><br>For example, if the hex value returned is <code>0x40010</code>,  which represents a decimal value of <code>262160</code>, then in binary it is <code>1000000000000100000</code>. If we format the binary value into pairs, starting from the right, we get <code>1 00 00 00 00 00 00 01 00 00</code>.</p><p>If we start from the right and move left in the above binary pairs, the third pair is <code>01</code>. The third reserve listed in <a href="./#getreserveslist"><code>getReserveList()</code></a> is WETH. Therefore the <code>1</code> indicates that WETH is being borrowed by this user. </p><p>If we continue to go to the end of the binary pairs (furthest left), we have <code>1</code> which can also be represented as <code>01</code>. This is the 10th pair, which in <a href="./#getreserveslist"><code>getReserveList()</code></a> is DAI. Therefore  the <code>1</code> indicates that it is being borrowed by this user.</p> |
| ndata | uint256     | <p>For NFT asset. The bitmask is divided into bits, one bit for each NFT. </p><p></p><p>The bit indicates if it is being used as collateral by the user.<br><br>The corresponding assets are in the same position as <a href="./#getreserveslist"><code>getNFTVaultsList()</code></a><br><br>For example, if the hex value returned is <code>0x40010</code>,  which represents a decimal value of <code>262160</code>, then in binary it is <code>1000000000000100000</code>. If we format the binary value into pairs, starting from the right, we get <code>1 00 00 00 00 00 00 01 00 00</code>.</p><p>If we start from the right and move left in the above binary pairs, the third pair is <code>01</code>. The third reserve listed in <a href="./#getreserveslist"><code>getNFTVaultsList()</code></a> is WPUNKS. Therefore the <code>1</code> indicates that WPUNKS is used as collateral by this user. </p><p>If we continue to go to the end of the binary pairs (furthest left), we have <code>1</code> which can also be represented as <code>01</code>. This is the 10th pair, which in <a href="./#getreserveslist"><code>getNFTVaultsList()</code></a> is BAYC. Therefore  the <code>1</code> indicates that it is being used as collateral.</p> |

### getReserveNormalizedIncome**()**

**`function getReserveNormalizedIncome(address asset)`**

Returns the normalized income per unit of `asset`.&#x20;

A return value of $$1e27$$ indicates no income. As time passes, the income is accrued. A value of $$2 * 1e27$$ indicates that for each unit of asset, two units of income have been accrued.

| Parameter Name | Type    | Description            |
| -------------- | ------- | ---------------------- |
| `asset`        | address | address of the reserve |

### getReserveNormalizedVariableDebt**()**

**`function getReserveNormalizedVariableDebt(address asset)`**

Returns the normalized variable debt per unit of `asset`.

A return value of $$1e27$$ indicates no debt. As time passes, the debt is accrued. A value of $$2 * 1e27$$ indicates that for each unit of asset, two units of debt have been accrued.

| Parameter Name | Type    | Description            |
| -------------- | ------- | ---------------------- |
| `asset`        | address | address of the reserve |

### paused**()**

**`function paused()`**

Returns `true` if the LendingPool is paused.

### getReservesList**()**

**`function getReservesList()`**

Returns the list of initialized reserves.

### getNFTVaultsList**()**

**`function getNFTVaultsList()`**

Returns the list of initialized NFT vaults.

### getAddressesProvider**()**

**function getAddressesProvider()**

Returns the addresses provider.

## Error Codes

In order to reduce gas usage and code size, vinci contracts return numbered errors. If you are making calls to the protocol and receive numbered errors, you can find what the numbers represent by checking the [`Errors.sol`](https://github.com/VinciProtocol/vinci-protocol/blob/master/contracts/protocol/libraries/helpers/Errors.sol)
