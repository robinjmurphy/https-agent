# https-agent

> HTTPS agent for Node with transparent proxy support

Creates a [HTTPS agent](http://nodejs.org/api/https.html#https_class_https_agent) that automatically handles proxy tunnelling using the `https_proxy` environment variable.

## Installation

```
npm install https-agent
```

## Usage

```js
var httpsAgent = require('https-agent');
var fs = require('fs');

var agent = httpsAgent({
  pfx: fs.readFileSync('/path/to/client.p12'),
  passphrase: 'client'
});
```

All of the standard [TLS options](http://nodejs.org/api/tls.html#tls_tls_connect_options_callback) are supported when creating an agent. Use the `pfx` and `passphase` options for a certificate in PKCS12 format or the `cert` and `key` options for separate certificate and key files.

## Examples

### Usage with `https.globalAgent`

To use an agent as the default agent (rather than setting it on every request) just set it as the [global agent](http://nodejs.org/api/https.html#https_https_globalagent).

```js
var httpsAgent = require('https-agent');
var fs = require('fs');
var https = require('https');

https.globalAgent = httpsAgent({
  pfx: fs.readFileSync('/path/to/client.p12'),
  passphrase: 'client'
});

// make HTTPS requests
```

### Usage with `https.get`

```js
var httpsAgent = require('https-agent');
var fs = require('fs');
var https = require('https');

var agent = httpsAgent({
  pfx: fs.readFileSync('/path/to/client.p12'),
  passphrase: 'client'
});

var options = {
  protocol: 'https:',
  hostname: 'www.example.com',
  port: 443,
  agent: agent
}

https.get(options, function (res) {
  res.on('data', function (data) {
    console.log(data);
  });
});
```

### Usage with [request](https://github.com/mikeal/request)

```js
var httpsAgent = require('https-agent');
var fs = require('fs');
var request = require('request');

var agent = httpsAgent({
  pfx: fs.readFileSync('/path/to/client.p12'),
  passphrase: 'client'
});

request('https://www.example.com', {agent: agent}, function (err, res, body) {
  console.log(body);
});
```

### Usage with [node-rest-client](https://github.com/aacerox/node-rest-client)

```js
var httpsAgent = require('https-agent');
var fs = require('fs');
var Client = require('node-rest-client').Client;

var agent = httpsAgent({
  pfx: fs.readFileSync('/path/to/client.p12'),
  passphrase: 'client'
});

var client = new Client({
  connection: {
    agent: agent
  }
});

client.get('https://www.example.com', function (body, res) {
  console.log(body);
});
```