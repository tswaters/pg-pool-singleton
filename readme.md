# pg-pool singleton

This module wrapps the pg-pool constructor and exports the created object, effectively creating a singleton for you.

It is literally 2 lines of code... 

```js
const {Pool} = require('pg')
module.exports = new Pool()
```

I've spent more time writing this readme than I did the `index.js` file.

## But why ?

You should typically only create one pool for a given process. If you need more than one for some reason, this module is certainly not for you!

Consider the below -- 

```js

// ./lib/db.js

const {Pool} = require('pg')

module.exports = new Pool()

// ./api/some-endpoint.js

const pool = require('../lib/db')

module.exports = async () => {
  await pool.connect()
  // do stuff, release client, etc.
}
```

You need a location to put the instantiated object.  For web servers, you might put is somewhere on the web server object, i.e. in express I'd put it under `app.locals` but this may not make sense without long-lived object like a web server.

Either way you slice it, you need to keep that pool somewhere, in some required file or long-lived object -- and this module takes that burden off your hands:

```js
const pool = require('pg-pool-singleton')
// that is all. do things with pool!
```

## Caveats

This module does not provide any connection object or string into the constructor, so it only works when using the [https://www.postgresql.org/docs/current/static/libpq-envars.html](postgres environment variables)

## License

WTFPL

## Contributions

There is literally no need to ever change this unless pg-pool adds this capability in the future.

I've got a wild-card `*` peerDependency on `pg` so there is a chance this might break with breaking changes in `pg`

It has been tested with version 7.
