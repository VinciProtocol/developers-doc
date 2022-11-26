# Market parameters

The parameters for each marketplace are set via the configuration files under the markets directory.

## Configurations Files

| Name               | Description                                      |
|--------------------|--------------------------------------------------|
| index.ts           | Basic settings for the market.                   |
| NFTVaultConfigs.ts | Settings for the NFT used in the market.         |
| reservesConfigs.ts | Settings for the ERC20 asset used in the market. |

## Basic settings

| Name           | Type                                           | Description                                     |
|----------------|------------------------------------------------|-------------------------------------------------|
| MarketId       | string                                         | A unique string used to identify the market.    |
| ProviderId     | number                                         | A unique integer used to identify the market.   |
| ReservesConfig | iMultiPoolsAssets<IReserveParams>              | Settings for the ERC20 asset used in the market |
| NFTVaultConfig | iMultiPoolsAssets<INFTParamss>                 | Settings for the NFT asset used in the market   |
| ReserveAssets  | iParamsPerNetwork<SymbolMap<tEthereumAddress>> | Address for the ERC20 asset used in the market  |
| NFTVaultAssets | iParamsPerNetwork<SymbolMap<tEthereumAddress>> | Address for the NFT asset used in the market    |

## ERC20 asset settings

| Name                    | Type                        | Description                                                 |
|-------------------------|-----------------------------|-------------------------------------------------------------|
| name                    | string                      | The name of the ERC20 token.                                |
| symbol                  | string                      | The symbol name of the ERC20 token.                         |
| strategy                | IInterestRateStrategyParams | The settings of the ERC20 asset interest rate.              |
| baseLTVAsCollateral     | string                      | Reserved, should be '0'                                     |
| liquidationThreshold    | string                      | Reserved, should be '0'.                                    |
| liquidationBouns        | string                      | Reserved, should be '0'.                                    |
| borrowingEnabled        | boolean                     | `true` for borrowable asset. `false` for unborrowable.      |
| stableBorrowRateEnabled | boolean                     | Reserved, should be `false`.                                |
| reserveDecimals         | string                      | The decimals of th ERC20 token.                             |
| vTokenImpl              | eContractid                 | Should be `eContractid.VToken`                              |
| reserveFactor           | string                      | How much interest goes into the treasury. `"3000"` for 30%. |

### Interest rate strategy parameters.

| Name                   | Type   | Description                                                                                |
|------------------------|--------|--------------------------------------------------------------------------------------------|
| name                   | string | The name of the strategy.                                                                  |
| optimalUtilizationRate | string | The optimal utilization rate we aim for. One ray is 1.                                     |
| baseVariableBorrowRate | string | The base interest rate. One ray is 1.                                                      |
| variableRateSlope1     | string | The slope of interest rate increase when optimal utilization is not reached. One ray is 1. |
| variableRateSlope2     | string | The slope of interest rate increase when optimal utilization is not reached. ONe ray is 1. |
| stableRateSlope1       | string | Reserved. Should be `"0"`.                                                                 |
| stableRateSlope2       | string | Reserved. Should be `"0"`.                                                                 |


## NFT settings

| Name                 | Type                  | Description                                                                |
|----------------------|-----------------------|----------------------------------------------------------------------------|
| name                 | string                | The name of the NFT.                                                       |
| symbol               | string                | The symbol name of the NFT.                                                |
| baseLTVAsCollateral  | string                | The LTV of the NFT. `"3000"` for 30%.                                      |
| liquidationThreshold | string                | A factor used to calculate if the NFT can be liquidated. `"3000"` for 30%. |
| liquidationBonus     | string                | Discount to liquidators. `"12500"` for 80%, `"13333"` for 75%.             |
| nTokenImpl           | eContractid           | Should be `eContractid.NToken`.                                            |
| lockdropExpiration   | string                | Should be `"0"`.                                                           |
| eligibility          | INFTEligibilityParams | Settings for the NFT eligibility.                                          |

### NFT eligitility settings

| Name | Type   | Description                                                      |
|------|--------|------------------------------------------------------------------|
| name | string | The name of the eligitility. `"AllowAll"` to allow all tokenids. |
| args | any    | The parameters for the eligitility. `null` for `"AllowAll"`.     |

