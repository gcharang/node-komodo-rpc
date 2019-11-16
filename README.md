# node-komodo-rpc

nodejs json-rpc for Komodo and Smart Chains, with Promises and support for multiple instances

- Supports on-the-fly RPC methods using [Proxies](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Proxy); all the RPC methods supported by a daemon (including the Antara API) are available
- Supports multiple instances (different daemons) in the same application
- Works only in Nodejs; for the browser version, [click here](https://www.npmjs.com/package/komodo-rpc-js)
- Very small codebase
- Uses [axios](https://github.com/axios/axios) behind the scenes

## Instructions

1. `require()` the module; it imports a class
2. launch a `new` instance of the class with a config object as the argument; the config object can contain the credentials directly or one of the following other ways to access them
   - datadir Ex: `{ datadir: "/home/username/.komodo/LABS" }`
   - name Ex: `{ name: "Rick" }`
   - conffile Ex: `{ conffile: "/home/username/.komodo/MORTY/MORTY.conf" }`
3. call `.config` with the new instance object to access its config
4. call `.rpc()` with the new instance object to access the RPC interface

## Usage

Passing the credentials directly

```js
const SmartChain = require("node-komodo-rpc");

const config = {
  rpchost: "localhost",
  rpcport: 7771,
  rpcuser: "user316977",
  rpcpassword: "pass47aac855ee750dab0128962d29e85920cbb8ad730d0e0307"
};

const komodo = new SmartChain({ config });

console.log(komodo.config); // Prints the config being used by the komodo instance

const komodoRPC = komodo.rpc();

komodoRPC
  .getinfo()
  .then(info => {
    console.log(info);
  })
  .catch(error => {
    console.log(error);
    throw new Error(error);
  });

komodoRPC
  .listunspent(6, 9999999, [
    "RPS3xTZCzr6aQfoMw5Bu1rpQBF6iVCWsyu",
    "RBtNBJjWKVKPFG4To5Yce9TWWmc2AenzfZ"
  ])
  .then(outs => {
    console.log(outs);
  })
  .catch(error => console.log(error));
```

Passing the data directory's path

```js
const rick = new SmartChain({
  datadir: "/home/username/.komodo/RICK"
});
```

Passing the Name

```js
const labs = new SmartChain({
  name: "labs"
});
```

Passing the Conf file's path

```js
const morty = new SmartChain({
  conffile: "/home/username/.komodo/MORTY/MORTY.conf"
});
```

When no argument is passed, it is equivalent to passing the argument `{name: "komodo"}`

```js
const komodo = new SmartChain();
```

To print the config being used by the LABS instance

```js
console.log(labs.config);
```

Output

```json
{
  "NAME": "labs",
  "HOSTNAME": "localhost",
  "PORT": "40265",
  "USERNAME": "user45765873",
  "PASSWORD": "passa8eb6941a2e7f4059d9726126d8989428f54748b94cd775",
  "DATADIR": "/home/<username>/.komodo/LABS/",
  "CONFFILE": "/home/<username>/.komodo/LABS/LABS.conf"
}
```

Descriptions of the properties

- `NAME` is the name of the Blockchain; more accurately, it is the name of the conf file
- `HOSTNAME` the address at which the RPC server (blockchain daemon) is listening for connections
- `PORT` is the port at which the RPC server (blockchain daemon) is listening for connections
- `USERNAME` is the username allowed to send RPC requests to the blockchain daemon
- `PASSWORD` is the password to authenticate the RPC requests to the blockchain daemon
- `DATADIR` is the data directory being used by the blockchain daemon; more accurately, this is the directory containing the conf file
- `CONFFILE` is the file containing the configuration settings of the blockchain daemon

## Defaults

If the `config` object is missing the keys: `rpchost` or `rpcport`, the default values used are `localhost` and `7771` respectively. When the `config` object is passed, `NAME`, `DATADIR` and `CONFFILE` are set to be `undefined`
