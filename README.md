# jsvutil

A Node.js utility wrapper for the [JSON Schema Validator](https://github.com/garycourt/JSV) (JSV).

## Overview

The JSV implementation is an excellent module by Gary Court. This jsvutil module provides a simple framework around JSV for validating JSON instances and checking JSON schemas.

## Features

This module augments JSV with the following features:

1. A `validate` function that combines JSV report errors into a single `ValidationError` exception class.
2. Applies any default values in the schema to the instance being validated.
3. A `check` function that validates a schema.
4. Supports callback functions as well as standard exceptions. (Useful in a Node.js environment.)

## Installation

    $ npm install jsvutil

## Examples

The following two examples illustrate how a configuration file can be
validated. Note that the `validate` function applies the default values in the
schema so that after validation, the config object returned by the `validate`
function may be different than the one read using the `require` function.

### Using Exceptions

    var jsvutil = require('jsvutil')
    var config = require('./config.json');
    var schema = require('./schema.json');

    try {
        config = jsvutil.validate(config, schema);
        console.log('config file is valid');
    } catch (e) {
        console.error(e.message);
        process.exit(1);
    }

    // Use the config object...

### Using Callbacks

    var jsvutil = require('jsvutil')
    var config = require('./config.json');
    var schema = require('./schema.json');

    jsvutil.validate(config, schema, function (err, config) {
        if (err) {
            console.error(e.message);
            process.exit(1);
        } else {
            console.log('config file is valid');
        }
    });

    // Use the config object...

## API

This section specifies the Application Programming Interface (API) for the
jsvutil package. The API consists of the `validate` and `check` functions.

**Important:** The `instance` and `schema` parameters must valid JavaScript
entities, *not* a JSON string. Either `JSON.parse` or the Node.js `require`
function should already have been called on the instance or schema source.

The optional callback function complies with Node.js conventions. The first
argument is either an error object on failure or null on success. The second
argument is the equivalent return value of the function.

### validate (function)

    validate(instance, schema, [cb])

Validates the specified instance against the specified schema. The return value
(or callback value) is the JSON instance with any schema default values applied
to any unset instance values. The original instance parameter is *not* modified
by this function.

Parameters:

- `instance` - {Any} The JSON instance to validate.
- `schema` - {Object} The JSON schema against which to validate the instance.
- `[cb]` - {Function} The optional cb(err, instance) callback function.

Returns:

- {Object} The validated JSON instance with any schema default values applied to any unset instance values.

Errors:

- Throws an error (or passes it to the callback function) if the specified JSON instance cannot be validated.

### check (function)

    check(schema, [cb])

Checks that the specified schema is valid.

Parameters:

- `schema` - {Object} The schema to check.

Errors:

- Throws an error (or passes it to the callback function) if the specified JSON schema is invalid.

## License (MIT)

Copyright (c) 2012 Frank Hellwig

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to
deal in the Software without restriction, including without limitation the
rights to use, copy, modify, merge, publish, distribute, sublicense, and/or
sell copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in
all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
IN THE SOFTWARE.
