## **<span style="color:red">This Readme is under construction</span>**

# Fuge Plus - The Microservice Shell

[![npm version][npm-badge]][npm-url]
[![npm downloads][npm-downloads-badge]][npm-url]
[![Build Status][travis-badge]][travis-url]
[![Win Status][win-badge]][win-url]
[![Gitter][gitter-badge]][gitter-url]

Fuge Plus is a microservice shell and workbench unlinked fork of . It provides an execution environment for developing microservice systems, eliminating shell hell and significantly reducing developer friction.

If you're using this module, and need help, you can:

- Post a [github issue](https://github.com/priyesh2609/fuge-plus/issues),
- Reach out on twitter to @priyesh2609

## Install

Use npm to install globally.

```
npm install -g fuge-plus-plus
```

## A Micoservice shell?

Yup. Fuge Plus is a shell environment that focuses on a specific system configuration, providing an emulation environment in which services can run in development that is close to a modern container production deployment.

## Usage

In order to run your system via fuge-plus, you need to create a YAML configuration file. Fuge Plus also integrates with docker-compose. Full documentation on configuring fuge-plus is [available here](https://github.com/apparatus/fuge-config).

Full example demonstration systems are available here:
[https://github.com/apparatus/fuge-examples](https://github.com/apparatus/fuge-examples).

## Example Configuration

A simple configuration is provided below:

```
fuge_global:
  tail: true
  monitor: true
  monitor_excludes:
    - '**/node_modules/**'
    - '**/.git/**'
    - '*.log'
myservice:
  type: process
  path: ../myservice
  run: 'node index.js'
  ports:
    - myservice=8000
webapp:
  type: process
  path: ../webapp
  run: 'npm start'
  ports:
    - webapp=3000
mongo:
  image: mongo
  type: container
  ports:
    - mongo=27017:27017
```

This configuration defines a front end webapp process, a single microservice and a mongodb container. The system can be started by running:

```sh
$ fuge-plus shell <path to config file>
fuge-plus> start all
```

This will start the mongo container through the local docker API and also the webapp and myservice processes. Fuge Plus will also inject configuration information into these processes as environment variables, tail the logs and watch the file system for changes, restarting the appropriate processes as you write code.

To fully explore the capabilites of Fuge Plus, you are encouraged to run the example systems which can be found here: [https://github.com/apparatus/fuge-examples](https://github.com/apparatus/fuge-examples).

## Docker

Fuge Plus is fully compatible with V1, V2 and V3 docker-compose formats. Fuge Plus services can be specified entirely using docker-compose or entirely in fuge-plus.yml or a mixture of anything in between.

## Command Reference

Fuge Plus can be started by running:

```sh
$ fuge-plus shell <path to fuge-plus config file>
fuge-plus>
```

The fuge-plus command shell supports the following commands

| command                          | action                                                                                                                               |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| `help`                           | display a list of supported commands                                                                                                 |
| `ps`                             | list status of managed processes and containers                                                                                      |
| `info` [process] [full]`         | show information on a specific process. The `full` option will output the entire environment that will be injected into this process |
| `start [process] / all`          | start a process or all processes                                                                                                     |
| `stop [process] / all`           | stop a process and any associated watcher or all processes                                                                           |
| `debug [process]`                | start a process in debug mode (only supported for node.js processes)                                                                 |
| `apply [Shell command]`          | run the specified shell command in the root of each service e.g. `apply npm install`, `apply git status`                             |
| `zone`                           | display dns zone information for fuge-plus internal DNS server - requires the `dns_enabled` setting in fuge-plus configuration file  |
| `watch [process] / all`          | turn on watching for a process or for all processes                                                                                  |
| `unwatch [process] / all`        | turn off watching for a process or for all processes                                                                                 |
| `tail [process] / all`           | tail output for a process or for all processes                                                                                       |
| `untail [process] / all`         | end tail output for a specific processes or for all processes                                                                        |
| `grep 'search string' [process]` | searches a process' log or all processes' logs                                                                                       |
| `exit`                           | terminate all managed processes and exit the shell                                                                                   |

### Shell pass through

In addition to the above commands, Fuge Plus will pass through all other commands to the underlying shell, for example:

```sh
fuge-plus> ps aux | grep -i node
```

Will bypass the fuge-plus ps command and execute the underlying `ps` command.

```sh
fuge-plus> netstat -an | grep -i listen
```

Will be passed through to the underlying shell to display a lists of open listening port and so on.

## Contributing

I encourage open participation. If you feel you can help in any way, be it with
documentation, examples, extra testing, or new features please get in touch.

## License

Copyright (c) 2020 Priyesh Patel, Licensed under [MIT][].

[npm-badge]: https://badge.fury.io/js/fuge-plus.svg
[npm-url]: https://badge.fury.io/js/fuge-plus
[npm-downloads-badge]: https://img.shields.io/npm/dm/fuge-plus.svg?maxAge=2592000
[mit]: ./LICENSE
