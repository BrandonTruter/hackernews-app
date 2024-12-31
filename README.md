# hackernews-app

## CRUD'ing Links

### Find a specific link

Request

```graphql
query {
  link(id:"link-0") {
    id
    description
  }
}
```

Response 

```json
{
  "data": {
    "link": {
      "id": "link-0",
      "description": "Fullstack tutorial for GraphQL"
    }
  }
}
```

###  Update a specific link

Request

```graphql
mutation {
  updateLink(id: "link-0", url: "www.local.io", description: "Prisma replaces ORMs") {
    id
  }
}
```

Response 

```json
{
  "data": {
    "updateLink": {
      "id": "link-0"
    }
  }
}
```

###  Delete a specific link

Request

```graphql
mutation {
  deleteLink(id: "link-0") {
    id
    url
  }
}
```

Response 

```json
{
  "data": {
    "deleteLink": {
      "id": "link-0",
      "url": "www.local.io"
    }
  }
}
```