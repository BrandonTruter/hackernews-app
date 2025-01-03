# hackernews-app

Builds a GraphQL server from scratch based on Node.js, apollo-server and Prisma for database access.

# Queries

Request

```graphql
query {
  feed {
    count
    links {
      id
      description
      url
    }
  }
}
```

Response

```json
{
  "data": {
    "feed": {
      "count": 5,
      "links": [
        {
          "id": "1",
          "description": "Fullstack tutorial for GraphQL",
          "url": "www.howtographql.com"
        },
        {
          "id": "2",
          "description": "Prisma replaces traditional ORMs",
          "url": "www.local.io"
        },
        {
          "id": "3",
          "description": "An awesome GraphQL conference",
          "url": "www.graphqlconf.org"
        },
        {
          "id": "4",
          "description": "Curated GraphQL content coming to your email inbox every Friday",
          "url": "www.graphqlweekly.com"
        },
        {
          "id": "5",
          "description": "Testing second incoming subscription request",
          "url": "http://localhost:4000"
        }
      ]
    }
  }
}
```

## Filters

Request

```graphql
query {
  feed(filter: "QL") {
    id
    description
    url
    postedBy {
      id
      name
    }
  }
}
```

Response

```json
{
  "data": {
    "feed": [
      {
        "id": "1",
        "description": "Fullstack tutorial for GraphQL",
        "url": "www.howtographql.com",
        "postedBy": null
      },
      {
        "id": "3",
        "description": "An awesome GraphQL conference",
        "url": "www.graphqlconf.org",
        "postedBy": {
          "id": "1",
          "name": "Alice"
        }
      },
      {
        "id": "4",
        "description": "Curated GraphQL content coming to your email inbox every Friday",
        "url": "www.graphqlweekly.com",
        "postedBy": {
          "id": "1",
          "name": "Alice"
        }
      }
    ]
  }
}
```

## Pagination

### Example 1

Request

```graphql
query {
  feed(take: 1, skip: 1) {
    id
    description
    url
  }
}
```

Response

```json
{
  "data": {
    "feed": [
      {
        "id": "2",
        "description": "Prisma replaces traditional ORMs",
        "url": "www.local.io"
      }
    ]
  }
}
```

### Example 2

Request

```graphql
query {
  feed(take: 3) {
    id
    description
    url
  }
}
```

Response

```json
{
  "data": {
    "feed": [
      {
        "id": "1",
        "description": "Fullstack tutorial for GraphQL",
        "url": "www.howtographql.com"
      },
      {
        "id": "2",
        "description": "Prisma replaces traditional ORMs",
        "url": "www.local.io"
      },
      {
        "id": "3",
        "description": "An awesome GraphQL conference",
        "url": "www.graphqlconf.org"
      }
    ]
  }
}
```

## Sorting

Request

```graphql
query {
  feed(orderBy: { createdAt: desc }) {
    id
    description
    url
  }
}
```

Response

```json
{
  "data": {
    "feed": [
      {
        "id": "5",
        "description": "Testing second incoming subscription request",
        "url": "http://localhost:4000"
      },
      {
        "id": "4",
        "description": "Curated GraphQL content coming to your email inbox every Friday",
        "url": "www.graphqlweekly.com"
      },
      {
        "id": "3",
        "description": "An awesome GraphQL conference",
        "url": "www.graphqlconf.org"
      },
      {
        "id": "2",
        "description": "Prisma replaces traditional ORMs",
        "url": "www.local.io"
      },
      {
        "id": "1",
        "description": "Fullstack tutorial for GraphQL",
        "url": "www.howtographql.com"
      }
    ]
  }
}
```

## Mutations

### Signup

Request

```graphql
mutation {
  signup(name: "John", email: "doe@prisma.io", password: "graphql") {
    token
    user {
      id
    }
  }
}
```

Response 

```json
{
  "data": {
    "signup": {
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOjIsImlhdCI6MTczNTg5Njc3NX0.CNUWGHzFBk4b6Yz-rLQXTpPoFcC1p0iibjIiFRDNRt0",
      "user": {
        "id": "2"
      }
    }
  }
}
```

### Login

Request

```graphql
mutation {
  login(email: "doe@prisma.io", password: "graphql") {
    token
    user {
      email
      links {
        id
        url
        description
      }
    }
  }
}
```

Response 

```json
{
  "data": {
    "login": {
      "token": "eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOjIsImlhdCI6MTczNTg5Njg0M30.3_ebWCQc3AaYXgm1n7tv5TpJ93v_G6rAZ9iL1LWSlnw",
      "user": {
        "email": "doe@prisma.io",
        "links": []
      }
    }
  }
}
```

### Post

Auth Headers

```json
{
  "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOjIsImlhdCI6MTczNTg5NjkwNX0.UV0b3c1HRwmqjg0J38J06asGXVyDisPPbIzYXSe3V2k"
}
```

Request

```graphql
mutation {
  post(url: "www.local.prisma.io", description: "Prisma is THE traditional ORM") {
    id
  }
}
```

Response 

```json
{
  "data": {
    "post": {
      "id": "6"
    }
  }
}
```

### Vote

Auth Headers

```json
{
  "Authorization": "Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1c2VySWQiOjIsImlhdCI6MTczNTg5NjkwNX0.UV0b3c1HRwmqjg0J38J06asGXVyDisPPbIzYXSe3V2k"
}
```

Request

```graphql
mutation {
  vote(linkId: "7") {
    link {
      url
      description
    }
    user {
      name
      email
    }
  }
}
```

Response 

```json
{
  "data": {
    "vote": {
      "link": {
        "url": "www.local.prisma.io",
        "description": "Prisma is THE traditional ORM"
      },
      "user": {
        "name": "Alice",
        "email": "alice@prisma.io"
      }
    }
  }
}
```

##  Subscriptions

### Links

#### Request 1

```graphql
subscription {
  newLink {
    id
    url
    description
    postedBy {
      id
      name
      email
    }
  }
}
```

#### Request 2

```graphql
mutation {
  post(url: "www.graphqlweekly.com", description: "Curated GraphQL content coming to your email inbox every Friday") {
    id
  }
}
```

#### Response 

```json
{
  "data": {
    "newLink": {
      "id": "4",
      "url": "www.graphqlweekly.com",
      "description": "Curated GraphQL content coming to your email inbox every Friday",
      "postedBy": {
        "id": "1",
        "name": "Alice",
        "email": "alice@prisma.io"
      }
    }
  }
}
```

### Votes

#### Request 1

```graphql
subscription {
  newVote {
    id
    link {
      url
      description
    }
    user {
      name
      email
    }
  }
}
```

#### Request 2

```graphql
mutation {
  vote(linkId: "3") {
    link {
      url
      description
    }
    user {
      name
      email
    }
  }
}
```

#### Response 

```json
{
  "data": {
    "newVote": {
      "id": "1",
      "link": {
        "url": "www.graphqlconf.org",
        "description": "An awesome GraphQL conference"
      },
      "user": {
        "name": "Alice",
        "email": "alice@prisma.io"
      }
    }
  }
}
```

