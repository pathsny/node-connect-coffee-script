[![Build Status](https://secure.travis-ci.org/wdavidw/node-connect-coffee-script.png)](http://travis-ci.org/wdavidw/node-connect-coffee-script)

# connect-coffee-script

It provide a simple [Connect] middleware to serve CoffeeScript files. It is modeled after the [Stylus] middleware.

This project was created after the drop of native support for CoffeeScript in latest [Express]. More specifically, [Express] droped the compiler middleware in its versions 2 and 3 (the current versions at the time of this writing).

Read the [annoucement article][annoucement] for more information.

# Installation

Using npm:

```bash
npm install connect-coffee-script
```

Usage
-----

Function returning a [Connect] middleware with the given `options`.

Options:

*   `force`     Always re-compile
*   `src`       Source directory used to find .coffee files
*   `dest`      Destination directory used to output .js files when undefined defaults to `src`
*   `compile`   Custom compile function, accepting the arguments `(str, options)`
*   `bare`      Compile without a top-level function wrapper
*   `baseDir`   Base directory for path resolution
*   `prefix`    Path which should be stripped from `src`

Basic example
-------------

Here we will setup the middleware with only the required `src` option.

```javascript
var coffeescript = require('connect-coffee-script');
var connect = require('connect');

var app = connect();

app.use(coffeescript({
  src: __dirname,
  bare: true
}));

app.use(connect.static(__dirname + '/public'));

app.listen(3000)
```

Advanced example
----------------

Here we set up the custom compile function so that we may
set the `bare` option, or define additional functions.

By default the compile function simply renders the JavaScript.

```javascript
function compile(str, options) {
  options.bare = true;
  return coffeeScript.compile(str, options);
}
```

Pass the middleware to [Connect], grabbing "*.coffee" files from this directory
and saving .js files to _./public_. Also supplying our custom `compile` function.

Following that we have a `static()` layer setup to serve the .js
files generated by CoffeeScript.

```javascript
var coffeeScript = require('coffee-script');
var connectCoffeescript = require('connect-coffee-script');
var connect = require('connect');

var app = connect();

app.use(connectCoffeescript({
  src: __dirname,
  dest: __dirname + '/public',
  compile: compile
}));

app.use(connect.static(__dirname + '/public'));
```

[connect]:      http://senchalabs.github.com/connect/
[stylus]:       http://learnboost.github.com/stylus/
[express]:      http://expressjs.com/
[annoucement]:  http://www.adaltas.com/blog/2012/07/24/coffee-script-connect-middleware/

Contributors
------------

*   David Worms : <https://github.com/wdavidw>
*   Dave Wasmer : <https://github.com/davewasmer>





