# Addresses Provider Registry

## LendingPoolAddressesProviderRegistry

A register of the active [`LendingPoolAddressesProvider`](../addresses-provider/) contracts, covering all markets. This contract is immutable and the address will never change.

The source code can be found on [Github here](https://github.com/VinciProtocol/vinci-protocol/blob/master/contracts/protocol/configuration/LendingPoolAddressesProviderRegistry.sol).

## View Methods

### getAddressesProvidersLis()

**`function getAddressesProvidersList()`**

Returns the list of active [`LendingPoolAddressesProvider`](../addresses-provider) contracts.

### getAddressesProviderIdByAddres()

**`function getAddressesProviderIdByAddress(address addressesProvider)`**

Returns the ID of an [`LendingPoolAddressesProvider`](../addresses-provider)

| Parameter Name | Type    | Description                       |
| -------------- | ------- | --------------------------------- |
| `provider`     | address | address of the addresses provider |
