# Deploy to the blockchain

Once you have set all the parameters, you are ready to start the deployment.

## Deploy base contracts

```sh
npx hardhat --network kovan full:deploy-address-provider-registry --pool VinciMAYC --verify
npx hardhat --network kovan full:deploy-collateral-manager --pool VinciMAYC --verify
npx hardhat --network kovan full:deploy-reserve-logic --verify
npx hardhat --network kovan full:deploy-nft-vault-logic --verify
npx hardhat --network kovan full:deploy-generic-logic --verify
npx hardhat --network kovan full:deploy-validation-logic --verify
npx hardhat --network kovan full:deploy-lending-pool-impl --pool VinciMAYC --verify
npx hardhat --network kovan full:deploy-lending-pool-conf-impl --pool VinciMAYC --verify
npx hardhat --network kovan full:deploy-ntoken-impl --verify
npx hardhat --network kovan full:deploy-timelock-ntoken-impl --verify
npx hardhat --network kovan full:deploy-vtoken-impl --verify
npx hardhat --network kovan full:deploy-variable-debt-token-impl --pool VinciMAYC --verify
npx hardhat --network kovan full:deploy-nft-eligibity-impl  --verify --name ALLOWALL
```

## Deploy Treasury

```sh
npx hardhat --network kovan full:deploy-treasury-impl  --verify
npx hardhat --network kovan full:deploy-treasury  --verify
npx hardhat --network kovan full:init-treasury  --verify
```

## Deploy misc contracts

```sh
npx hardhat --network kovan full:deploy-wallet-balance-prvider --verify
npx hardhat --network kovan full:deploy-ui-provider --verify
npx hardhat --network kovan full:deploy-fallback-oracle --pool VinciMAYC --verify
npx hardhat --network kovan full:deploy-aave-oracle --pool VinciMAYC --verify
npx hardhat --network kovan full:set-fallback-oracle --pool VinciMAYC --verify
npx hardhat --network kovan  --pool VinciMAYC --verify
npx hardhat --network kovan --pool VinciMAYC --verify
npx hardhat --network kovan  --verify
```

### WETH Gateway

```sh
npx hardhat --network kovan full:deploy-weth-gateway --pool VinciMAYC --verify
```

### WPUNK Gateway

```sh
npx hardhat --network kovan full:deploy-wpunks-gateway --pool VinciMAYC --verify
```

## Setup a market

Set the addresses in `helpers/markets-commons.ts` to the deployed addresses.

Then:
```sh
npx hardhat --network kovan full:deploy-address-provider --pool VinciMAYC --verify
npx hardhat --network kovan full:deploy-lending-pool --pool VinciMAYC --verify
npx hardhat --network kovan full:deploy-lending-pool-config --pool VinciMAYC --verify
npx hardhat --network kovan full:set-oracle --pool VinciMAYC
npx hardhat --network kovan full:set-pool-admin --pool VinciMAYC
npx hardhat --network kovan full:init-nft-vault --pool VinciMAYC
npx hardhat --network kovan full:config-nft-vault --pool VinciMAYC
```


If using WPUNKS as the NFT:
```sh
npx hardhat --network kovan full:authorize-wpunks-gateway --pool VinciMAYC
```

Then:
```
npx hardhat --network kovan full:deploy-rate-strategy --pool VinciMAYC --verify
npx hardhat --network kovan full:init-reserves --pool VinciMAYC
npx hardhat --network kovan full:config-reserves --pool VinciMAYC
```

If using WETH as the ERC20 asset:
```sh
npx hardhat --network kovan full:authorize-weth-gateway --pool VinciMAYC
```

Then:
```
npx hardhat --network kovan full:set-collateral-manager --pool VinciMAYC
npx hardhat --network kovan full:set-pool-admin --pool VinciMAYC
npx hardhat --network kovan full:set-emergency-admin --pool VinciMAYC
npx hardhat --network kovan full:deploy-vtokens-rates-helper --pool VinciMAYC --verify
npx hardhat --network kovan full:set-vtokens-rates-helper-as-pool-admin --pool VinciMAYC
npx hardhat --network kovan full:deploy-stable-variabletokens-helper --pool VinciMAYC --verify
npx hardhat --network kovan full:set-stable-variabletokens-helper-as-owner --pool VinciMAYC
```
