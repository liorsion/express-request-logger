# express-request-logger

An easy way to create a key/value pair list to be logged on each request in express.

## Installation

```` bash
$ cd /path/to/your/project
$ npm install express-request-logger
````

## Usage

This package is meant to be used as middleware for a web app using [Express](http://expressjs.com/). It will allow you to create a key/value pair list of data to get logged per request. Using this it can output data to allow you to analyze how your web app is being used.

### Middleware Setup

Since response time is a part of the logging output, express-request-logger should be put as early in the middleware chain as possible.

```` javascript
var winston   = require('winston');
var reqLogger = require('express-request-logger');

var logger = new (winston.Logger)({ transports: [ new (winston.transports.Console)() ] });
app.use(reqLogger.create(logger, options, extraRequestFields));
````

#### Options

Optionally takes an options object.

 - `excludes`: an array of url paths that will be excluded from logging.

#### extraRequestFields

Optionally takes extra request fields to log. This is either a string or array of strings that include request fields that will be extracted from the request and loged.

For example:

app.use(reqLogger.create(logger, options, ['headers']));

will (in addition to default logging) log headers of request, which is useful in development env.

### Middleware Usage

The `req` object will now have a `kvLog` object that can have extra data added to it. At the end of the request it will be logged with all the other data collected.

```` javascript
function (req, res, next) {
  // ...
  req.kvLog.action = 'test';
  // ...
}
````

The key ```message``` is special, it will be removed from the object and be sent to the logger as the true message. It is optional, so if it is not included, the log will just contain an empty message.

## Dependencies

Currently it is recommended to use [winston](https://github.com/flatiron/winston) as the logger object, but any object that can be called like `loggerObj.log(level, message, {key: value})` will work.
