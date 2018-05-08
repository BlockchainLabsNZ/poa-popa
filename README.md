# POA network - Proof of Physical Address (PoPA)

[![Build Status](https://travis-ci.org/poanetwork/poa-popa.svg?branch=master)](https://travis-ci.org/poanetwork/poa-popa)
[![Coverage Status](https://coveralls.io/repos/github/poanetwork/poa-popa/badge.svg?branch=master)](https://coveralls.io/github/poanetwork/poa-popa?branch=master)
[![dependencies Status](https://david-dm.org/poanetwork/poa-popa/status.svg)](https://david-dm.org/poanetwork/poa-popa)
[![devDependencies Status](https://david-dm.org/poanetwork/poa-popa/dev-status.svg)](https://david-dm.org/poanetwork/poa-popa?type=dev)

## Identity DApps
In POA Network, identity of individual validators plays a major role for selected consensus. We propose additional checks of identity, performed in a decentralized way. Proof of Identity DApps is a series of decentralized applications focused on connecting a user's identity to his/her wallet. Applications can be run on any Ethereum-compatible network.

## Proof of Physical Address (PoPA)
Using Proof of Physical Address, a user can confirm his/her physical address. It can be used to prove connection between residency and a network address (wallet).
User submits a form with his physical address details (name, state, city, etc) on DApp main page. This data is added to the PoPA contract deployed to the network and thus a correspondence between a wallet and physical address is registered in the contract. However, this correspondence is not yet verified.

To verify the address server sends a postcard (via post office) with confirmation code to the registered physical address. Confirmation code is used by the user to call one of contract's methods (via DApp confirmation page) to verify the correspondence between the confirmation code and the wallet used initially to register the physical address.

A more detailed schematic view of the process:
![popa-scheme](https://raw.githubusercontent.com/poanetwork/wiki/master/assets/imgs/poa/papers/whitepaper/proof-of-address.png)

## How to test the current version locally
1. clone this repository
```
git clone https://github.com/poanetwork/poa-popa.git
```

2. make sure you have node.js version >= 6.9.1 installed

3. install Ganache CLI globally
```
npm install -g ganache-cli
```

4. cd to the repo folder and install dependencies.
```
cd poa-popa
npm install
```

4. sensitive data (like lob api key) can be provided by creating `web-dapp/server-config-private.js` file that exports config object like so:
```
'use strict';

module.exports = function (cfgPublic) {
    return {
        lobApiKey: '******************************',
        lobApiVersion: '2017-06-16',
        rpc: '******************************',
        signer: '0x****************************', // with 0x prefix
        signerPrivateKey: '******************************', // without 0x prefix
        confirmationPageUrl: '******************************',
    };
};
```
_Note:_ you can get the `lobApiKey` registering on [Lob](https://lob.com/) and copying your **Test API Key** from **User -> Settings -> API Keys**

If this file is present, its keys will add to/replace keys in `web-dapp/server-config.js`.

5. open new tab in your terminal, and start testrpc with a set of predefined acounts
```
npm run start-testrpc
```
leave this tab opened until your test is complete.

6. in the first tab of your terminal deploy the contract
```
npm run deploy-contract
```
answer `yes` when confirmation appears.

7. then to compile react components and start dapp, run:
```
npm start
```
wait until a build is ready and `Listening on 3000` is printed in terminal

8. open file `scripts/start_rpc.sh` in text editor and import one of the accounts from there to MetaMask using its private key. You can choose any address-private-key pair except `0xdbde11e51b9fcc9c455de9af89729cf37d835156` which is reserved for contract's owner.

9. navigate to http://localhost:3000 in your browser and do tests.

To find out confirmation code, look for a line like
```
[prepareRegTx] confirmation confirmationCodePlain: y8t44s8yrt
```
in server logs

To find response details from Lob, including links to the postcard, look for a line like
```
[notifyRegTx] postcard: {"id":"psc_106fe1363e5b9521", ..., "to": ..., thumbnails": ... }
```
in server logs

_Note:_ in the property `thumbnails` you can found the url of the front and back sides of the postcard with the confirmation code:
```json
"thumbnails": [
    {
      "small": "https://s3.us-west-2.amazonaws.com/assets.lob.com/psc_...",
      "medium": "https://s3.us-west-2.amazonaws.com/assets.lob.com/psc_...",
      "large": "https://s3.us-west-2.amazonaws.com/assets.lob.com/psc_..."
    },
    {
      "small": "https://s3.us-west-2.amazonaws.com/assets.lob.com/psc_..",
      "medium": "https://s3.us-west-2.amazonaws.com/assets.lob.com/psc_..",
      "large": "https://s3.us-west-2.amazonaws.com/assets.lob.com/psc_.."
    }
```
### Running tests on test network:
1. make sure you have truffle installed
```
npm install -g truffle
```
2. switch to `blockchain` folder
3. Install dependencies
```
npm install
```
4. Setup up `.env` file, copy `.env.example` from the root directory into the `blockchain` folder
5. Run tests
```
truffle test
```

### Running javascript tests:
1. run the test script
```
npm run test
```

2. if you want to run linter test,
```
npm run lint
```

Note: Before to run the `npm install` script it will copy a `pre-push` hook to the `.git` folder, so, before to each `git push`, it will run the tests

## How to deploy to a real network
1. download the latest version from master branch
```
git clone https://github.com/poanetwork/poa-popa.git
```
2. install dependencies
```
cd poa-popa
npm install
```
3. deploy the contract, e.g. use Remix and Metamask
4. create file `poa-popa/web-dapp/src/contract-output.json` with the following structure:
```
    {
        "ProofOfPhysicalAddress": {
            "address": "*** CONTRACT ADDRESS, 0x... ***",
            "bytecode": "*** BYTECODE, 60606040... ***",
            "abi": [ *** ABI *** ]
        }
    }
```
5. create file `poa-popa/web-dapp/server-config-private.js` with the following content:
```
'use strict';

module.exports = function (cfgPublic) {
    return {
        lobApiKey: '*** LOB TEST OR PROD API KEY ***',
        lobApiVersion: '*** LOB TEST OR PROD API VERSION ***',
        rpc: '*** PROBABLY INFURA ***',
        signer: '*** SIGNER ADDRESS, 0x... ***', // with 0x prefix
        signerPrivateKey: '*** SIGNER PRIVATE KEY ***', // without 0x prefix
        confirmationPageUrl: '*** URL FOR CONFIRMATION PAGE, e.g. https://yourserver.com/confirm ***', // used only for postcard
        // it is recommended to install and use redis for keeping session keys
        sessionStore: {
            "type": "redis",
            "params": { *** REDIS CONNECTION PARAMETERS *** }
        },
    };
};
```
6. build react components
```
# in poa-popa/web-dapp
npm run build
```
7. start server
```
# in poa-popa/web-dapp
node server
```
or, still better, use pm2
```
# in poa-popa/web-dapp
pm2 start -i 0 server
```

## Description
### contract
Contract source file is `blockchain/contracts/ProofOfPhysicalAddress.sol`.
* main data structures are `User` and `PhysicalAddress`:
```solidity
struct PhysicalAddress {
    string name;

    string country;
    string state;
    string city;
    string location;
    string zip;

    uint256 creationBlock;
    bytes32 keccakIdentifier;
    bytes32 confirmationCodeSha3;
    uint256 confirmationBlock;
}

struct User {
    uint256 creationBlock;
    PhysicalAddress[] physicalAddresses;
}

mapping (address => User) public users;
```

`location` in contract is alias for `address` in dapp.

* there are also three variables for statistics
```solidity
uint64 public totalUsers;
uint64 public totalAddresses;
uint64 public totalConfirmed;
```

* contract has `owner` which is the account that sent the transaction to deploy the contract.

* contract has `signer` which is the account that is used to calculate signatures on server-side and validate parameters from contract-side. By default when contract is created, `signer` is set to `owner`. You can change it later with `setSigner` method.

* main methods are
```solidity
function registerAddress(
    string name,
    string country, string state, string city, string location, string zip,
    uint256 priceWei,
    bytes32 confirmationCodeSha3, uint8 sigV, bytes32 sigR, bytes32 sigS)
public payable
 ```
 used to register a new address, and
 ```solidity
function confirmAddress(string confirmationCodePlain, uint8 sigV, bytes32 sigR bytes32 sigS)
public
```
used to confirm an address.

* `name` may be different for each new address

* `country`, `state`, `city`, `location` and `zip` are `trim()`ed and `toLowerCase()`ed by dapp before passing them to the contract.

* when confirmation code is entered, `userAddressByConfirmationCode` method is called by dapp to search for address with matching confirmation code.

### signing parameters
First, all relevant parameters for `registerAddress` and `confirmAddress` need to be converted from utf8 strings to hex strings and then combined together into a single long hex string and then passed to `sign()` function (defined in `web-dapp/server-lib/sign.js`). This function produces a signature, that is divided into three parameters `v`, `r` and `s` that need to be passed to client and then by the client to contract's method.
Contract uses built-in ethereum function `ecrecover` to verify that signer's address matches contract's `signer`:
```solidity
function signerIsValid(bytes32 data, uint8 v, bytes32 r, bytes32 s)
public constant returns (bool)
{
    bytes memory prefix = "\x19Ethereum Signed Message:\n32";
    bytes32 prefixed = keccak256(prefix, data);
    return (ecrecover(prefixed, v, r, s) == signer);
}
```
Note the use of magical `prefix`.
