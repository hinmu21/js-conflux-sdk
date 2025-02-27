# change log

## v2.1.11

* Add `cfx_getCollateralInfo` RPC method which was import from `conflux-rust v2.2.5`
* `SponsorInfo` add two more fields `availableStoragePoint` and `usedStoragePoint`

## v2.1.7

* Optimize the gas and storageCollateral logic of transaction population.

## v2.1.5

* Add support the new InternalContract `ParamsControl`
* Remove `cfx_getLogs` filter's field `offset` and `limit`
* Add support for new RPC `cfx_getParamsFromVote`
* Update typescript datatype definitions

## v2.1.0

* Add more jsdoc comment to generate ts `d.ts` files
* Move `cfx` related formatters (block, transaction, receipt, log and etc) to independent file `./src/rpc/types/formmatter.js`.

## v2.0.7

* Fix logFilter formater bug

## v2.0.6

* Fix contract method batch call bug

## v2.0.3

* Change trace `epochNumber`, `transactionPosition` from BigInt to Int
* Add field `totalEspaceTokens` in `getSupplyInfo`'s response

## v2.0.0

* Add support for PoS RPC methods
* Split RPC methods to it's own namespace, currently include: `cfx`, `pos`, `trace`, `txpool`
* Add support for batch RPC
* Browser export class name change from Conflux to `TreeGraph`
* Add method `getNextUsableNonce` to `conflux.advanced`, which will first try to use `txpool_nextNonce` to get a usable nonce, if failed it will fall back to `cfx_getNextNonce`
* Add three internal contracts `CrossSpaceCall`, `ConfluxContext`, `PoSRegister`
* Add one method `cfxMappedEVMSpaceAddress` to `address` utility.

### trace updates

* Add a new boolean field `valid` to indicate whether this trace has change state
* The `internal_transfer_action` type trace has added four new field `fromPocket`, `toPocket`, `fromSpace`, `toSpace`
* The `call` and `create` type trace has added one new field `space`

For detail explanation of the trace updates check [this doc](https://github.com/Conflux-Chain/CIPs/issues/88).

Check [v2.0 changes](./docs/v2.0_changes.md) for change details

### Provider API

* Add `provider.request` to make provider compliant with [EIP-1193](https://eips.ethereum.org/EIPS/eip-1193)
* Add a new provider `WechatProvider` which can be used in Wechat environment

Here is the complete Conflux-rust v2.0 [RPC change overview](https://developer.confluxnetwork.org/v2-hardfork/hydra_overview#rpc-changes)

## v1.7.2

* Add `balance` key in the result of `estimateGasAndCollateralAdvance` as the balance of `options.from`.

## v1.7.1

* Add method `checkBalanceAgainstTransaction`, and `estimateGasAndCollateralAdvance` to better estimate gas and check balance.

## v1.6.15

* Add `blockNumber` to block related methods `cfx_getBlockByHash`, `cfx_getBlockByEpochNumber`, `cfx_getBlockByHashWithPivotAssumption` which need `Conflux-rust v1.1.5` or above.
* Add one new RPC method `cfx_getBlockByBlockNubmer`

## v1.6.10

* `format.bytes` now only support hex string, will not accept non hex utf-8 string
* Add cli `cfxjs` which can random generate a `0x1 prefix ethereum` address
* `Conflux` add a new method `getEpochReceiptsByPivotBlockHash`

## v1.6.9

* Support `keepAlive` option in `Conflux` initialization.
* Add one util method `tracesInTree` to return a tree structure traces.
* Fix contract method override bug.
* Add a address utility method `shortenCfxAddress`.

## v1.6.3

* Support `retry` option in `Conflux` initialization.
* Add pending transaction status enum `CONST.PENDING_TX_STATUS` currently have two value: `FUTURE_NONCE` `NOT_ENOUGH_CASH`

## v1.6.2

* Optimize the address convert performance.

## v1.6.1

* `Conflux` add method `getAccountPendingTransactions` to get one account's pending transaction.
* Split API documents into several files, which is easy to read.

## v1.6.0

This version is corresponding to conflux-rust v1.1.3, check it's [changelog](https://github.com/Conflux-Chain/conflux-rust/blob/master/changelogs/CHANGELOG-1.1.x.md#113) for detailed info.

* `format.address` will respect `networkId`, `verbose` flag even if the first parameter is an CIP37 address.
* Add support for a standard token contract through `Conflux.CRC20`
* `cfx_getLogs` filter option add one more field `offset`
* Add one RPC method `cfx_getAccountPendingInfo` to get account's transaction pending info
* `epochs` pubsub now accept one parameter `subscription_epoch` the supported values are `latest_mined` \(default\) and `latest_state`
* Include `blockHash`, `epochHash`, `epochNumber`, `transactionHash`, and `transactionPosition` for trace RPCs
* When ABI encoding `bytes-N` type, if the data's length is not enough, will auto pad \(right\) to `N`

## v1.5.13

* `getStatus` method rethurn three new fields `latestState`, `latestConfirmed`, `latestCheckpoint`
* add two `trace` related rpc `traceTransaction`, `traceFilter`
* add one debug rpc `getEpochReceipts`
* add two provider wrapper `wrapEthereum`, `wrapConflux` to work with metamask

Notice: this is an update corresponding `conflux-rust v1.1.2`

## v1.5.10

* `Conflux`'s option can pass `networkId` now, and add a new method `updateNetworkId` to sync networkId from RPC.
* `format.address` will return new CIP37 addresses, if you pass a hex address, `networkId` should also be passed as second parameter
* add new method `format.hexAddress` to format hex address
* Wallet's constructor add a parameter `networkId`
* PrivateKeyAccount `constructor`, `decrypt`, `random` need one more parameter `networkId`
* `Transaction`, `Message` `sign` method need one more parameter `networkId`
* Conflux's get methods will return new address, and same to contract method returned address.
* `getSupplyInfo` response add new field `totalCirculating`
* `getStatus` response add new field `networkId`

## v1.1.7

* Add RPC method `traceBlock`  to `Conflux` which can used to get block's execution trace

## v1.1.6

* export `Contract`

```js
// nodejs
const { Contract } = require('js-conflux-sdk');
```

```js
import { Contract } from 'js-conflux-sdk'
```

## v1.1.5

* add `stateMutability` for method from abi

```js
console.log(contract.symbol.stateMutability) // "view"
console.log(contract.transfer.stateMutability) // "nonpayable"
```

* rename EventLog.params to EventLog.arguments

```js
await conflux.contract.Transfer(null, null, null).getLogs({
  fromEpoch: 2868400,
  toEpoch: 2868500,
});

/* [ 
  { 
    data: '0x0000000000000000000000000000000000000000000000000001184b321b4e44', 
    topics: [ '0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef', '0x0000000000000000000000001dc05200485776b79f195a1e617dbccb6826f1c4', '0x000000000000000000000000882c4ddb1d3210b5ae778360729c04cd3242df70' ], 
    ...,
    arguments: NamedTuple(from,to,value)(3) [ '0x1dc05200485776b79f195a1e617dbccb6826f1c4', '0x882c4ddb1d3210b5ae778360729c04cd3242df70', 308186218974788n ] 
  } 
] */
```

* add `subscribeLogs` for EventLog

```js
subscription = await contract.Transfer(null, null, null).subscribeLogs();
subscription.on('data', data => { console.log(data); });
```

* contract decode constructor data with out bytecode

## v1.1.4

* add `conflux.getSupplyInfo`

## v1.1.3

* WebsocketProvider with Websocket options

```js
new Conflux({ url: 'ws://127.0.0.1', clientConfig: { maxReceivedFrameSize: 10_1000_1000 } }) // 10 MB
```

## v1.1.2

* add `conflux.getVoteList`
* add `conflux.getDepositList`
* update `conflux.getTransactionReceipt`

## v1.1.1

fix: update request id avoid repeat

```js
// old
conflux.provider.requestId(); // "16055917399420726"
// new
conflux.provider.requestId(); // "175d4b91862001f4f81eb443"
```

## v1.1.0

fix: use native websocket for front-end

* use BigInt for nodejs, JSBI for browser

```js

// for nodejs
// old
conflux.getBalance(ADDRESS); // JSBI(1) [ -1153374696, sign: false ]
// new 
conflux.getBalance(ADDRESS); // 3141592600n
```

## v1.0.1

fix: EventCoder, FunctionCoder, valueCoder decode return string but not JSBI

## v1.0.0

* add `defaultGasRatio` and `defaultStorageRatio`

```js
conflux = new Conflux({ defaultGasRatio: 1.1, defaultStorageRatio: 1.1, ... })
```

* add `BaseAccount` and `PrivateKeyAccount`

Account `signTransaction` and `signMessage` to be `async`

* add wallet

wallet use for create and manage `Account` by address

```js
account = conflux.wallet.addRandom(); 
console.log(conflux.wallet.has(account.address)); 
// true
account = conflux.wallet.addPrivateKey(PRIVATE_KEY); 
console.log(conflux.wallet.has(account.address)); 
// true
account = conflux.wallet.addKeystore(keystore, password); 
console.log(conflux.wallet.has(account.address)); 
// true
```

wallet use for sendTransaction

```js

// old 
account = conflux.Account(PRIVATE_KEY);
conflux.sendTransaction({ from: account.address, ...}) // address will call `cfx_sendTranscion`
conflux.sendTransaction({ from: account,  ... }) // must be instance of `Account` to sign by sdk and call `cfx_sendRawTransaction`

// new 
conflux.sendTransaction({ from: address, ... }) // if address not in `conflux.wallet`, call `cfx_sendTranscion`
account = conflux.wallet.addPrivate(PRIVATE_KEY);
conflux.sendTransaction({ from: account.address,  ... }) // if account in `conflux.wallet`, sign by account and call `cfx_sendRawTransaction`
conflux.sendTransaction({ from: account,  ... }) // same as `from: account.address`, but some user think input account instance with privateKey is unsafe
```

* add Subscription

```js
await conflux.subscribe(name, ...args);  // => id 
await conflux.subscribeEpochs(); // => Subscription with event 'data' 
await conflux.subscribeNewHeads(); // => Subscription with event 'data' 
await conflux.subscribeLogs(); // => Subscription with event 'data', 'revert' 
await conflux.unsubscribe(id); // => boolean 
await conflux.unsubscribe(subscription); // => boolean
```

* add internal contract

```js
contract = conflux.InternalContract('AdminControl'); 
console.log(contract);
contract = conflux.InternalContract('SponsorWhitelistControl'); 
console.log(contract);
contract = conflux.InternalContract('Staking'); 
console.log(contract);
```

* add checksum address

```js
// old 
conflux.getBalance('0x1B716c51381e76900EBAA7999A488511A4E1fD0a'); // ok 
conflux.getBalance('0x1B716c51381e76900EBAA7999A488511A4E1fD0A'); // ok

// new 
conflux.getBalance('0x1B716c51381e76900EBAA7999A488511A4E1fD0a'); // ok 
conflux.getBalance('0x1B716c51381e76900EBAA7999A488511A4E1fD0A'); // Error\('address checksum error'\)
```

* `providerFactory` only accept first argument as override options

```js
// old 
provider = providerFactory('http://localhost:12537')
// new 
provider = providerFactory({ url: 'http://localhost:12537' })
```

* add batch request

```js
provider = providerFactory({ url: 'http://localhost:12537' })
array = await provider.batch([ { method: 'cfx_epochNumber' }, { method: 'cfx_getBalance', params: ['0x0123456789012345678901234567890123456789'] }]) 
// [ '0x1381', '0x0']
```

* add WebSocketProvider

```js
provider = providerFactory({ url: 'ws://localhost:12535' })
provider.close(); // user need to close before process ternimal
```

* BaseProvider instanceof EventEmitter

```js
const EventEmitter = require('events');

// old 
console.log(new BaseProvider() instanceof EventEmitter); // false 
console.log(new HttpProvider() instanceof EventEmitter); // false

// new 
console.log(new BaseProvider({}) instanceof EventEmitter); // true 
console.log(new HttpProvider({}) instanceof EventEmitter); // true 
console.log(new WebSocketProvider({}) instanceof EventEmitter); // true
```

* add CONST

```js
const { CONST } = require('js-conflux-sdk');
console.log(CONST.EPOCH_NUMBER)
{ 
  LATEST_MINED: 'latest_mined', 
  LATEST_STATE: 'latest_state', 
  LATEST_CONFIRMED: 'latest_confirmed', 
  LATEST_CHECKPOINT: 'latest_checkpoint', 
  EARLIEST: 'earliest' 
}
```

* export `format` and `sign` without `util`

```js
// old 
const { util: {format, sign} } = require('js-conflux-sdk');
// new 
const { format, sign } = require('js-conflux-sdk');
```

* add `Drip` to replace unit

```js
// old 
const { util } = require('js-conflux-sdk');
const balance = await conflux.getBalance(ADDRESS); 
console.log(util.unit.fromDripToCFX(balance))

// new 
const { Drip } = require('js-conflux-sdk');
const balance = await conflux.getBalance(ADDRESS); 
console.log(Drip(balance).toCFX())
```

for input, use `Drip.fromXXX` to get drip number string

```js

// old 
const { util } = require('js-conflux-sdk');
const tx = { to: ADDRESS, value: util.unit.fromCFXToDrip(0.1), ... }

// new 
const { Drip } = require('js-conflux-sdk');
const tx = { to: ADDRESS, value: Drip.fromCFX(0.1), ... }

```

* include all method from conflux JSON_RPC document

[JSON_RPC](https://developer.confluxnetwork.org/conflux-doc/docs/json_rpc) 

* friendly example code

example will guide user to use SDK step by step

* charming code organization

split abi coder with types

split contract method, event and override

## v0.13.4

* rename `send_transaction` to `cfx_sendTransaction`

## v0.13.3

* Account.encrypt returned address drop '0x' prefix

## v0.13.2

* use scrypt-js

## v0.13.1

* RPC returned all number as hex

* fix `sendTransaction`, `call`, `estimateGasAndCollateral` shallow copy `options`

## v0.13.0

* `Account.decrypt` required keystoreV3 object as input, and put `password` as second parameter

```js

// old 
Account.decrypt('password', {salt:..., iv:..., cipher:..., mac:...})

// new 
Account.decrypt({ 
  version: 3, 
  id: '0bb47ee0-aac3-a006-2717-03877afa15f0', 
  address: '0x1cad0b19bb29d4674531d6f115237e16afce377c', 
  crypto: { 
    ciphertext: 'a8ec41d2440311ce897bacb6f7942f3235113fa17c4ae6732e032336038a8f73', 
    cipherparams: { iv: '85b5e092c1c32129e3d27df8c581514d' }, 
    cipher: 'aes-128-ctr', 
    kdf: 'scrypt', 
    kdfparams: { dklen: 32, salt: 'b662f09bdf6751ac599219732609dceac430bc0629a7906eaa1451555f051ebc', n: 8192, r: 8, p: 1 }, 
    mac: 'cc89df7ef6c27d284526a65cabf8e5042cdf1ec1aa4ee36dcf65b965fa34843d' 
  } 
}, 
'password')
```

* `Account.prototype.encrypt` returned keystoreV3 format object

```js

const account = new Account('0x0123456789abcdef0123456789abcdef0123456789abcdef0123456789abcdef');

// old 
account.encrypt('password') 
/*
{ 
  algorithm: 'aes-128-ctr', 
  N: 8192, 
  r: 8, 
  p: 1, 
  dkLen: 32, 
  salt: '0xb662f09bdf6751ac599219732609dceac430bc0629a7906eaa1451555f051ebc', 
  iv: '0x85b5e092c1c32129e3d27df8c581514d', 
  cipher: '0xa8ec41d2440311ce897bacb6f7942f3235113fa17c4ae6732e032336038a8f73', 
  mac: '0xcc89df7ef6c27d284526a65cabf8e5042cdf1ec1aa4ee36dcf65b965fa34843d' 
}
*/

// new 
account.encrypt('password') 
/* 
{ 
  version: 3, 
  id: '0bb47ee0-aac3-a006-2717-03877afa15f0', 
  address: '0x1cad0b19bb29d4674531d6f115237e16afce377c', 
  crypto: { 
    ciphertext: 'a8ec41d2440311ce897bacb6f7942f3235113fa17c4ae6732e032336038a8f73', 
    cipherparams: { 
      iv: '85b5e092c1c32129e3d27df8c581514d' 
    }, 
    cipher: 'aes-128-ctr', 
    kdf: 'scrypt', 
    kdfparams: { 
      dklen: 32, 
      salt: 'b662f09bdf6751ac599219732609dceac430bc0629a7906eaa1451555f051ebc', 
      n: 8192, 
      r: 8, 
      pw: 1 
    }, 
    mac: 'cc89df7ef6c27d284526a65cabf8e5042cdf1ec1aa4ee36dcf65b965fa34843d' 
  } 
}
*/
```

* epochNumber accept `earliest`, `latest_checkpoint`, `latest_confirmed` label

## v0.12.0

* add `getAdmin`

```js
await cfx.getAdmin('0x89996a8aefb2228593aae723d47f9517eef1341d') // "0x1be45681ac6c53d5a40475f7526bac1fe7590fb8"
```

* sendTransaction accept privateKey as `from`

```js
cfx.sendTransaction({ 
  from: PRIVATE_KEY, // accept Account instance or privateKey 
  to: ADDRESS, // accept Account instance or address 
  ..., 
})
```

* create Account accept address

```js
new Account(PRIVATE_KEY); // {privateKey:'0x...', publicKey: '0x...', address: '0x...'} 
new Account(PUBLIC_KEY); // {publicKey: '0x...', address: '0x...'} 
new Account(ADDRESS); // {address: '0x...'}
```

## v0.11.0

* defaultGasPrice, only use for sendTransaction

```js
cfx = new Conflux({ url: 'https://test.confluxrpc.com', defaultGasPrice: 100, });
// old 
cfx.call({ address: '0x...', data: '0x...', }); // => cfx_call{defaultGasPrice:'0x64',address:'0x...',data:'0x...'}
// new 
cfx.call({ address: '0x...', data: '0x...', }); // => cfx_call{address:'0x...',data:'0x...'}
```

* remove defaultEpoch, defaultChainId, defaultGas, defaultStorageLimit

```js

// old 
cfx = new Conflux({ url: 'https://test.confluxrpc.com', defaultEpoch: 'latest_state', defaultChainId: 1, defaultGasPrice: 100, defaultGas: 10, defaultStorageLimit: 1, })

// new 
cfx = new Conflux({ url: 'https://test.confluxrpc.com', defaultGasPrice: 100, })

// user could `epochNumber` and `chainId` manual on each method.

```

## v0.10.3

* fix broken sourcemap

## v0.10.2

* fix: include crypto into browserify build

```js

// old 
ConfluxJSSDK.util.sign.randomPrivateKey() // TypeError randomBytes is not a function

```

## v0.10.1

* add format.bytes

```js

format.bytes('abcd'); //  format.bytes([0, 1, 2, 3]);

```

* add contract method & event type or signature indexes

```js
// solidity function override(bytes memory str) public function override(string memory str) public
contract.override('str'); // Error: can not determine override
contract['override(string)']('str'); // specify override method by type contract['0x227ffd52']('str'); // specify override method by signature
```

## v0.10.0-alpha

* add `getStatus`

```js
cfx.getStatus()
```

* remove `getRiskCoefficient` and replace with `getConfirmationRiskByHash`

```js
// old 
cfx.getRiskCoefficient(epochNumber)
// new 
cfx.getConfirmationRiskByHash(blockHash)

```

* remove `getAccount` cause it's internal RPC.
* use `require` replace `import` to gen code.

## v0.9.2

* add defaultStorageLimit and defaultChainId for Conflux

```js
// old 
const cfx = new Conflux({ url: 'https://test.confluxrpc.com', defaultGasPrice: 100, defaultGas: 100000, })
// new 
const cfx = new Conflux({ url: 'https://test.confluxrpc.com', defaultGasPrice: 100, defaultGas: 100000, defaultStorageLimit: 4096, defaultChainId: 0, })
```

## v0.9.1

* abi implicitly converting string to number

solidity method: `function add(uint,uint) public returns (uint);`

```js
// old 
await contract.add(1, '2'); // error! can not accept string
// new version 
await contract.add(1, '2'); // good, converting string to number
```

## v0.9.0-beta

* format nonce as JSBI.BigInt

```js
nonce = await cfx.getNextNonce(...)
// old 100000
// new JSBI.BigInt(100000)
```

* format transaction fields

```js
tx = await cfx.getTransactionByHash(txHash) 
// old 
{ storageLimit: "0x64", chainId: "0x0", epochHeight: "0x400", ... }
// new 
{ 
  storageLimit: JSBI.BigInt(100), // JSBI 
  chainId: 0, 
  epochHeight: 1024, ... 
}
```

* unit return string

```js
// old 
unit.fromCFXToGDrip(123) => JSBI.BigInt(123000000000) 
unit.fromCFXToGDrip('0.1234567891') => Error('Cannot convert JSBI.BigInt')
// new 
unit.fromCFXToGDrip(123) => "123000000000" 
unit.fromCFXToGDrip('0.1234567891') => "123456789.1"
```

* contract fields "code" rename to "bytecode"

```js
// old 
cfx.Contract({code, abi, address})
// new 
cfx.Contract({bytecode, abi, address})
```

* abi decodeData and decodeLog return object

```js
result = contract.abi.decodeData('0x....')
// old 
["Tom", JSBI.BigInt(18)]
// new 
{ name: 'func' fullName: 'func(string name, uint age)', type: 'func(string,uint)', signature: '0x812600df', array: ["Tom", JSBI.BigInt(18)], object: { name: "Tom", age: JSBI.BigInt(18), } }
```
