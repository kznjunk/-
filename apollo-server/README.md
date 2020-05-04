# Apollo Server 

## Overview

- [Links](#links)
- [Introduction](#introduction)
- [Installation](#installation)
- [First Steps](#first-steps)

## Links

- https://www.apollographql.com/docs/apollo-server/
- https://www.robinwieruch.de/graphql-apollo-server-tutorial

## Introduction

**Apollo Server** is an open-source, spec-compliant GraphQL server that's compatible with any GraphQL client, 
including Apollo Client.
It's the best way to build a production-ready, self-documenting GraphQL API that can use data from any source.

You can use Apollo Server as:
- a stand-alone GraphQL server, including in a serverless environment
- an add-on to your application's existing Node.js middleware (such as Express or Fastify)
- a gateway for a federated data graph

## Installation

`npm install apollo-server-express`

## First steps

```
import express from 'express';
import { ApolloServer, gql } from 'apollo-server-express';
 
const app = express();
 
const schema = gql`
  type Query {
    me: User
  }
 
  type User {
    username: String!
  }
`;
 
const resolvers = {
  Query: {
    me: () => {
      return {
        username: 'Robin Wieruch',
      };
    },
  },
};
```



