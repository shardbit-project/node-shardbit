# node-shardbit
[![npm][npm-image]][npm-url]
[![downloads][downloads-image]][downloads-url]

[npm-image]: https://img.shields.io/npm/v/shardbit.svg?style=flat
[npm-url]: https://npmjs.org/package/shardbit

[downloads-image]: https://img.shields.io/npm/dm/shardbit.svg?style=flat
[downloads-url]: https://npmjs.org/package/shardbit

node-shardbit is a simple wrapper for the ShardBit client's JSON-RPC API.

## Install

`npm install shardbit`

## Examples

### Create client

```js
// all config options are optional
var client = new shardbit.Client({
  host: 'localhost',
  port: 37452,
  user: 'username',
  pass: 'password',
  timeout: 30000
});
```

### Get balance across all accounts with minimum confirmations of 6

```js
client.getBalance('*', 6, function(err, balance, resHeaders) {
  if (err) return console.log(err);
  console.log('Balance:', balance);
});
```

### Getting the balance directly using `cmd`

```js
client.cmd('getbalance', '*', 6, function(err, balance, resHeaders){
  if (err) return console.log(err);
  console.log('Balance:', balance);
});
```

### Batch multiple RPC calls into single HTTP request

```js
var batch = [];
for (var i = 0; i < 10; ++i) {
  batch.push({
    method: 'getnewaddress',
    params: ['myaccount']
  });
}
client.cmd(batch, function(err, address, resHeaders) {
  if (err) return console.log(err);
  console.log('Address:', address);
});
```

## SSL
See [Enabling SSL on original client](https://en.shardbit.it/wiki/Enabling_SSL_on_original_client_daemon).

If you're using this to connect to shardbitd across a network it is highly
recommended to enable `ssl`, otherwise an attacker may intercept your RPC credentials
resulting in theft of your shardbits.

When enabling `ssl` by setting the configuration option to `true`, the `sslStrict`
option (verifies the server certificate) will also be enabled by default. It is
highly recommended to specify the `sslCa` as well, even if your shardbitd has
a certificate signed by an actual CA, to ensure you are connecting
to your own shardbitd.

```js
var client = new shardbit.Client({
  host: 'localhost',
  port: 37452,
  user: 'username',
  pass: 'password',
  ssl: true,
  sslStrict: true,
  sslCa: fs.readFileSync(__dirname + '/myca.cert')
});
```
