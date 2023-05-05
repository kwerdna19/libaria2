# [libaria2.ts](https://www.npmjs.com/package/libaria2)

![npm bundle size](https://img.shields.io/bundlephobia/min/libaria2?label=size&style=for-the-badge)
![GitHub repo file count](https://img.shields.io/github/directory-file-count/yjl9903/libaria2?style=for-the-badge)
![npm](https://img.shields.io/npm/dm/libaria2?style=for-the-badge)
![GitHub Repo stars](https://img.shields.io/github/stars/yjl9903/libaria2?style=for-the-badge)

> This is a fork of [hydrati/libaria2-ts](https://github.com/hydrati/libaria2-ts).

TypeScript (Node.js & Browser) library for [aria2](https://aria2.github.io/).

- [libaria2.ts](#libaria2ts)
  - [Introduction](#introduction)
  - [Features](#features)
  - [Getting Started](#getting-started)
  - [Usage](#usage)
    - [Create client](#create-client)
    - [Methods](#methods)
  - [License](#license)

## Introduction

libaria2.ts uses [Aria2 JSON-RPC Interface](https://aria2.github.io/manual/en/html/aria2c.html#rpc-interface) to control it.

## Features

- Multiple Transports
  - [HTTP](https://aria2.github.io/manual/en/html/aria2c.html#rpc-interface)
  - [WebSocket](https://aria2.github.io/manual/en/html/aria2c.html#json-rpc-over-websocket)
- Promise-based API
- Full-Typing, JSDoc

## Getting Started

Install this package

```bash
npm install libaria2
```

Start aria2 with rpc, example

```bash
aria2c --enable-rpc --rpc-listen-all=true --rpc-allow-origin-all
```

## Usage

### Create client

```ts
import { WebSocket as Aria2WebSocket } from "libaria2-ts";
// or 
//     import { Http as Aria2Http } from "libaria2-ts";

const aria2 = new Aria2WebSocket({...});
// or
//     const aria2 = new Aria2Http({...});

```

Example options

```ts
{
  host: 'localhost',
  port: 6800,
  path: '/jsonrpc',
  auth: {
    secret: 'hello'
  }
}
```

### Methods

```ts
let ver = await aria2.getVersion();
/*
 * Output:
 * { version: '...', enabledFeatues: [...] }
 */

const resl = await aria2.system.multicall(
  { methodName: 'aria2.getVersion', params: [] },
  { methodName: 'aria2.addUri', params: ['http://example.com/qwer.zip'] }
);
/*
 * Output:
 * Array<Promise<...>>
 */

// or:
aria2.on('aria2.onDownloadStart', (event: IAria2NotificationEvent) => {
  console.log(`Download ${event.gid} Started`);
});

// or:
aria2.onceDownloadStart().then((event: IAria2NotificationEvent) => {
  console.log(`Download ${event.gid} Started`);
}));

await aria2.closeConnection();
```

More methods, see [Aria2ClientBaseClient](https://hydrati.github.io/libaria2-ts/classes/adapter.aria2clientbaseclass.html)

## License

MIT License © 2021 [Oxygen](https://github.com/hydrati)

MIT License © 2023 [XLor](https://github.com/yjl9903)
