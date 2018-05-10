# Functional tests
Tests are conducted on the Kovan test network. The following contracts have been verified on Etherscan.

## PhysicalAddressClaim.sol [`0x597cb26`](https://kovan.etherscan.io/address/0x597cb26b0dadd5361bf4a544eea571405f0f7d82#code)
## EthereumClaimsRegistry.sol [`0x3ea4494`](https://kovan.etherscan.io/address/0x3ea4494de0f892359544ef1322edef389db15bbf#code)
## ProofOfPhysicalAddress.sol [`0x2dacccb`](https://kovan.etherscan.io/address/0x2dacccbea6443f43a2c96ab66d35ac3266f90145#code)

## Accounts

- Owner: [0x00B9aeCde3876eEF0CA818c3a8de55493e881e21](https://kovan.etherscan.io/address/0x00B9aeCde3876eEF0CA818c3a8de55493e881e21)
- Signer: [0x00B9aeCde3876eEF0CA818c3a8de55493e881e21](https://kovan.etherscan.io/address/0x00B9aeCde3876eEF0CA818c3a8de55493e881e21)

# Expected Behaviour tests

### Should succeed

- [x] `setSigner()` sets the signer - [0x3a1016](https://kovan.etherscan.io/tx/0x3a1016d95846b9564716bb7d90ebdc3e46e29beabef385e3531eaf46991f5ed5)
- [x] `registerAddress()` registers an address [address data](1) - [0x47ba0e](https://kovan.etherscan.io/tx/0x47ba0e18f27e06875da6d3fd3b46d20f9091b707616ce2c53ff4b7cb97b9af6e)
- [x] `confirmAddress()` confirms an address and increases total counters - [0x1885e66](https://kovan.etherscan.io/tx/0x1885e665506d0092f041bc07b91bafcb67c690986aed1bfc06769d589716f5d5)
- [x] `withdrawAll` retrieves the balance of the contracts - [0x451a2f](https://kovan.etherscan.io/tx/0x451a2f1dda9be7c9815517b84878dc0080cf2672ee92c01fcef9cdb615488f59)
- [x] `withdrawSome()` with amount set to 0 [0x5369](https://kovan.etherscan.io/tx/0x5369bd68c4cc6bacadfd54822ed74b96bd12fec27ba83af779f5c41933485e7b)
- [x] `unregisterAddress()` can be used to remove your own address [0x83c6bb](https://kovan.etherscan.io/tx/0x83c6bb517cdeb849f334a5c83a1e880c80dace86dde139071588196a4072be0f)


### Should revert

- [x] Should not be able to `setSigner` from a non-owner - [0x4baf4d](https://kovan.etherscan.io/tx/0x4baf4d7003ef696586811482efc2b71bb5319537246f254dacf115bc6d739e8d)
- [x] `registerAddress()` can't be called again with the same address for the same user [0x3bc6](https://kovan.etherscan.io/tx/0x3bc6e6281e17fb757c8c76bb641f998f9a098ff393e3ee568e1402db9e198d76)
- [x] You can not use `unregisterAddress()` if you don't have an address registered. [0xb7fa](https://kovan.etherscan.io/tx/0xb7fa66f760090be041eacd9b7ea3f170fa427e8494069218ad5b25548110d3ab)


__References__

[1]: `name: 'john doe', country: 'us', state: 'ca', city: 'san francisco', address: '185 berry st', zip: '94107'`
