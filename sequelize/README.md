# Sequelize

## Overview

- [Links](#links)
- [Installation](#installation)
- [Introduction](#introduction)
- [CLI](#cli)
  - [Commands](#commands)


## Links
- https://sequelize.org/
- https://github.com/sequelize/cli
- 

## Installation

`npm install --save sequelize`

## Introduction

Sequelize is a promise-based Node.js ORM for Postgres, MySQL, MariaDB, SQLite and Microsoft SQL Server.
It features solid transaction support, relations, eager and lazy loading, read replication and more.

## CLI

## Config

```
// .sequelizerc

const path = require('path')

module.exports = {
  'config': path.resolve('config', 'database.js'),
  'models-path': path.resolve('db', 'models'),
  'seeders-path': path.resolve('db', 'seeders'),
  'migrations-path': path.resolve('db', 'migrations')
}
```

### Commands

- `sequelize init`
- `sequelize model:create --name user --attributes username:string,email:string,password:string`
- `sequelize migration:create --name add_some_fields`
- `sequelize db:migrate`
- `sequelize seed:generate` and `sequelize db:seed`




