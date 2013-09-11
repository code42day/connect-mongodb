# connect-mongodb

connect-mongodb is a mongoDB session store backed by [node-mongodb-native](http://github.com/christkv/node-mongodb-native).

Originally written by [dvv](http://github.com/dvv)
With changes by [Nathan Bowser](https://github.com/nathanbowser)

## Version 2.*

Removed dependency on specific connect version (easier updates).

## Installation

via npm:

    $ npm install connect-mongodb

## Options

To start `connect-mongodb`, you have to pass instances of select node-mongodb-native classes, thus permitting the usage of existing connections
or server configurations.

Using an existing connection:

  * `db` Existing connection/database reference (instance of mongodb.Db)

Or with a server configuration:

  * `server_config` Existing server configuration
                   (may be an instance of either mongodb.Server, mongodb.ServerPair, mongodb.ServerCluster, mongodb.ReplSetServers)
                   - review node-mongodb-native docs.

Other options:

  * `collection` MongoDB collection to host sessions. _'sessions' by default_
  * `reapInterval` ms to check expired sessions to remove on db
  * `username` To authenticate your db connection
  * `password` To authenticate your db connection

## Example

You have a complete example on `example/index.js`.

    var connect = require('connect')
      , Db = require('mongodb').Db
      , Server = require('mongodb').Server
      , server_config = new Server('localhost', 27017, {auto_reconnect: true, native_parser: true})
      , db = new Db('test', server_config, {})
      , mongoStore = require('connect-mongodb')(connect);

    connect.createServer(
      connect.bodyParser(),
      connect.cookieParser(),
      connect.session({
        cookie: {maxAge: 60000 * 20} // 20 minutes
      , secret: 'foo'
      , store: new mongoStore({db: db})
      })
    );

## Tests

To run the tests:

``` bash
make
```
