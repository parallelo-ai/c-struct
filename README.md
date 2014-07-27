c-struct [![Build Status](https://travis-ci.org/majimboo/c-struct.svg?branch=master)](https://travis-ci.org/majimboo/c-struct)
========

[![NPM](https://nodei.co/npm/c-struct.png?downloads=true)](https://nodei.co/npm/c-struct/)

A fast binary data packing &amp; unpacking library for node.js designed for multiplayer games.

What can it do?
---------------

* 8, 16, 24, 32, 40 and 48 bit signed and unsigned integers.
* String with length and null-terminated cstrings.
* Boolean, nibble, float and double.
* Big and little endianness.
* Schema based packing and unpacking.
* Unpack from buffer to object.
* Pack from object to buffer.

More
----
* Available via [npm](https://npmjs.org/package/c-struct).
* Zero production dependencies.

Installation
------------

    npm install c-struct --save

> Execute `$ node examples/` to see examples.

Usage
-----

### Unpacking ###

    var _ = require('c-struct');

    var playerSchema = new _.Schema({
      id: _.type.uint16,
      name: _.type.string(16),
      hp: _.type.uint24,
      exp: _.type.uint32,
      status: _.type.uint8,
      motd: _.type.string(), // null-terminated if no length
      motw: _.type.string(), // cstring if no length
      skills: [{
        id: _.type.uint8,
        name: _.type.string(32),
        secret: _.type.uint40
      }],
      position: {
        x: _.type.uint16,
        y: _.type.uint16
      },
      hash: _.type.uint48
    });

    // register to cache
    _.register('Player', playerSchema);

    // object to buffer | this can be on another file
    var buf = _.packSync('Player', {
      id: 1,
      name: 'Foobar',
      hp: 1000,
      exp: 88888888,
      status: 100,
      skills: [{
        id: 1,
        name: 'traps of thunder',
        secret: 5151515151
      }, {
        id: 2,
      }, {
        name: 'fatal blow'
      }, {
        id: 3,
        name: 'galvano strike'
      }],
      position: {
        x: 102,
        y: 351
      },
      motd: 'welcome',
      motw: 'weekly',
      hash: 99999999,
    });

### Packing ###

    var _ = require('c-struct');

    var playerSchema = new _.Schema({
      id: _.type.uint16,
      name: _.type.string(16),
      hp: _.type.uint24,
      exp: _.type.uint32,
      status: _.type.uint8,
      motd: _.type.string(), // null-terminated if no length
      motw: _.type.string(), // cstring if no length
      skills: [{
        id: _.type.uint8,
        name: _.type.string(32),
        secret: _.type.uint40
      }],
      position: {
        x: _.type.uint16,
        y: _.type.uint16
      },
      hash: _.type.uint48
    });

    // register to cache
    _.register('Player', playerSchema);

    // buffer to object | this can be on another file
    var obj = _.unpackSync('Player', BUFFER_HERE);


Configurations
----

> There is currently no configs.

TODO
----

- add configurable endianness
- add async methods
- add signed
- add boolean and nibble
- add float and double
- add benchmark
- documentation
- more testing

