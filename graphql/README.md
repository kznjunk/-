# GraphQL

## Overview

- [Introduction](#introduction)
- [Links](#links)
- [Installation](#installation)
- [Queries and Mutations](#queries-and-mutations)
  - [Fields](#fields)
  - [Arguments](#arguments)
  - [Aliases](#aliases)
  - [Fragments](#fragments)
  - [Operation Name](#operation-name)
  - [Variables](#variables)
  - [Directives](#directives)
  - [Mutations](#mutations)
  - [Inline Fragments](#inline-fragments)
- [Schemas and Types](#schemas-and-types)
  - [Type System](#type-system)
  - [Object Types and Fields](#object-types-and-fields)
  - [Arguments](#arguments-b)
  - [The Query and Mutation Types](#the-query-and-mutation-types)
  - [FieScalar Typeslds](#scalar-types)
  - [Enumeration Types](#enumeration-types)
  - [Lists and Non-Null](#lists-and-non-null)
  - [Interfaces](#interfaces)
  - [Union Types](#union-types)
  - [Input Types](#input-types)
- [...](#...)

## Links
- https://graphql.org/
- https://github.com/graphql/graphql-js/
- https://www.npmjs.com/package/graphql

## Introduction

TL;DR: A query language for APIs created by Facebook

GraphQL is:
- a query language
- a server-side runtim for executing queries
- not tied to any specific database/storage engine
- backed by the code and data

A GraphQL service is created by:
- defining types and fields on those types
- providing functions for each field on each type

For example:
```gql
type Query {
  me: User
}

type User {
  id: ID
  name: String
}
```

```js
function Query_me(request) {
  return request.auth.user;
}

function User_name(user) {
  return user.getName();
}
```

## Installation

`npm install graphql`


## Queries and Mutations

### Fields

The query has exactly the same shape as the result, because you always get back what you expect and
the server knows exactly what fields the client is asking for.

GraphQL queries can traverse related objects and their fields, letting clients fetch lots of related data in one request.

```gql
{
  hero {
    name
    # Queries can have comments!
    friends {
      name
    }
  }
}
```

### Arguments

When you add the ability to pass arguments to fields, things get much more interesting.
Every field and nested object can get its own set of arguments and you can even pass arguments into
scalar fields, to implement data transformations once on the server (cf. [Schemas and Types](#schemas-and-types) for more).

```gql
{
  human(id: "1000") {
    name
    height(unit: FOOT)
  }
}
```

### Aliases

You can't directly query for the same field with different arguments.
That's why you need aliases - they let you rename the result of a field to anything you want.

```gql
{
  empireHero: hero(episode: EMPIRE) {
    name
  }
  jediHero: hero(episode: JEDI) {
    name
  }
}

### Fragments

GraphQL includes reusable units called fragments.
Fragments let you construct sets of fields, and then include them in queries where you need to.

```gql
query HeroComparison($first: Int = 3) {
  leftComparison: hero(episode: EMPIRE) {
    ...comparisonFields
  }
  rightComparison: hero(episode: JEDI) {
    ...comparisonFields
  }
}

fragment comparisonFields on Character {
  name
  friendsConnection(first: $first) {
    totalCount
    edges {
      node {
        name
      }
    }
  }
}
```

### Operation Name

In production apps it's useful to use the *query* (operation type) and the *query name* (operation name) to make our code less ambiguous.

```gql
query HeroNameAndFriends {
  hero {
    name
    friends {
      name
    }
  }
}
```

The operation type is either:
- query
- mutation
- subscription

The operation name is a meaningful and explicit name for your operation.

### Variables

When we start working with variables, we need to do three things:
1. Replace the static value in the query with `$variableName`
2. Declare `$variableName` as one of the variables accepted by the query
3. Pass `variableName: value` in the separate, transport-specific (usually JSON) variables dictionary

```gql
query HeroNameAndFriends($episode: Episode) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}

#Variables
{
  "episode": "JEDI"
}
```

The variable definitions are the part that looks like `($episode: Episode)` in the query above.
It works just like the argument definitions for a function in a typed language.
It lists all of the variables, prefixed by $, followed by their type, in this case Episode.

All declared variables must be either
- scalars
- enums
- or input object types

Variable definitions can be optional or required.
In the case above, since there isn't an ! next to the Episode type, it's optional.

```gql
query HeroNameAndFriends($episode: Episode = JEDI) {
  hero(episode: $episode) {
    name
    friends {
      name
    }
  }
}
```

When default values are provided for all variables, you can call the query without passing any variables.
If any variables are passed as part of the variables dictionary, they will override the defaults.

### Directives

```gql
query Hero($episode: Episode, $withFriends: Boolean!) {
  hero(episode: $episode) {
    name
    friends @include(if: $withFriends) {
      name
    }
  }
}

#Variables
{
  "episode": "JEDI",
  "withFriends": false
}
```

Directives can be useful to get out of situations where
you otherwise would need to do string manipulation to add and remove fields in your query.
Server implementations may also add experimental features by defining completely new directives.

A directive can be attached to a field or fragment inclusion,
and can affect execution of the query in any way the server desires.
The core GraphQL specification includes exactly two directives,
which must be supported by any spec-compliant GraphQL server implementation:
- `@include(if: Boolean)`: Only include this field in the result if the argument is `true`
- `@skip(if: Boolean)`: Skip this field if the argument is `true`


### Mutations

Any complete data platform needs a way to modify server-side data.
It's useful to establish a convention that any operations that cause writes should be sent explicitly via a mutation.

Just like in queries, if the mutation field returns an object type, you can ask for nested fields.
This can be useful for fetching the new state of an object after an update.

```gql
mutation CreateReviewForEpisode($ep: Episode!, $review: ReviewInput!) {
  createReview(episode: $ep, review: $review) {
    stars
    commentary
  }
}

#Variables
{
  "ep": "JEDI",
  "review": {
    "stars": 5,
    "commentary": "This is a great movie!"
  }
}
```

A mutation can contain multiple fields, just like a query.
While query fields are executed in parallel, mutation fields run in series, one after the other.

### Inline Fragments

```gql
query HeroForEpisode($ep: Episode!) {
  hero(episode: $ep) {
    name
    ... on Droid {
      primaryFunction
    }
    ... on Human {
      height
    }
  }
}

#Variables
{
  "ep": "JEDI"
}
```

```gql
{
  search(text: "an") {
    __typename
    ... on Human {
      name
    }
    ... on Droid {
      name
    }
    ... on Starship {
      name
    }
  }
}
```

## Schemas and Types

### Type System

```gql
{
  hero {
    name
    appearsIn
  }
}
```

Every GraphQL service defines a set of types which completely describe the set of possible data you can query on that service. Then, when queries come in, they are validated and executed against that schema.

### Object Types and Fields

The most basic components of a GraphQL schema are object types, which just represent a kind of object you can fetch from your service, and what fields it has. In the GraphQL schema language, we might represent it like this:

```gql
type Character {
  name: String!
  appearsIn: [Episode!]!
}
```

- **Character** is a GraphQL Object Type, meaning it's a type with some fields. Most of the types in your schema will be object types
- **name** and **appearsIn** are fields on the **Character** type. That means that **name** and **appearsIn** are the only fields that can appear in any part of a GraphQL query that operates on the **Character** type.
- **String** is one of the built-in scalar types - these are types that resolve to a single scalar object, and can't have sub-selections in the query
- **String!** means that the field is non-nullable, meaning that the GraphQL service promises to always give you a value when you query this field
- **[Episode!]!** represents an array of Episode objects. Since it is also non-nullable, you can always expect an array (with zero or more items) when you query the appearsIn field. And since Episode! is also non-nullable, you can always expect every item of the array to be an Episode object

### Arguments B


### The Query and Mutation Types


### Scalar Types


### Enumeration Types


### Lists and Non-Null


### Interfaces


### Union Types


### Input Types

