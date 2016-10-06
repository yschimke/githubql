# Github Query Tools

## GraphQL Quick Start

1. Approve agreement for prerelease https://github.com/prerelease/agreement
2. Generate a token with (repo + user) https://github.com/settings/tokens 
3. Install githubql and authenticate with the token above 

```
$ brew install yschimke/tap/githubql
$ oksocial --authorize github --token 9999999999999999999999999999999999999999
```

Then experiment with queries https://graphql-explorer.githubapp.com/ and then execute from the command line.
First argument is either an inline query or a file e.g. @myquery.graphql.  The second argument is optional and is used as variables.

```
$ githubql '{ viewer { login } }'

{
  "data": {
    "viewer": {
      "login": "yschimke"
    }
  }
}
```

```
$ githubql 'query Search($q: String!) {
  search(query: $q, type: USER, first: 2) {
    edges {
      node {
        ... on User {
          name
        }
      }
    }
  }
}' '{"q": "schimke" }'

{
  "data": {
    "search": {
      "edges": [
        {
          "node": {
            "name": "Yuri Schimke"
          }
        },
        {
          "node": {
            "name": "Sascha Schimke"
          }
        }
      ]
    }
  }
}
```

Post a mutation

```
$ githubql 'mutation ExampleMutation {
  addComment(input: {
    subjectId: "MDU6SXNzdWUxNTczMzg4ODk=" 
    body: "test"
  }) {
    commentEdge {
      node {
        author {
          name
        }
        repository {
          name
        }
      }
    }
  }
}'
```
