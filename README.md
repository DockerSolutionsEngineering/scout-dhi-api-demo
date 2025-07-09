# Scout & DHI API Demo

This guide demonstrates how to leverage the GraphQL API of Docker Scout (and DHI) for various automation tasks.

> **Note:** The API isn't publicly documented and may introduce breaking changes. However, we have permission from the product team to share this with customers eager to automate certain functionalities via the API.

---

## How to Invoke the API

### 1. Generate an Access Token

Create a Personal Access Token (PAT) or Organizational Access Token (OAT):

- [Create a PAT](https://docs.docker.com/security/for-developers/access-tokens/#create-an-access-token)
- [Create an OAT](https://docs.docker.com/security/for-admins/access-tokens/)

### 2. Exchange the Token for a JWT Bearer Token

Use the following `curl` command to exchange your PAT or OAT for a JWT Bearer token:

```bash
curl --location 'https://hub.docker.com/v2/auth/token' \
  --header 'Content-Type: text/plain' \
  --data '{ "identifier": "", "secret": "" }'
```

This will respond with an access token:

![Auth Token](/assets/auth_token.png)

### 3. Invoke the API Using Postman

Since this is a GraphQL API, interacting with it is more straightforward using a GUI API client like Postman.

#### API Call to List Node Images

- **URL:** `https://api.scout.docker.com/v1/graphql`
- **GraphQL Operation:** `DhiRepositoryDetails`
- **Authorization:** `Bearer <your_access_token>`

**Query:**

```graphql
query DhiRepositoryDetails($org: String!, $repo: String!) {
  dhiRepositoryDetails(
    context: { organization: $org }
    query: { repoName: $repo }
  ) {
    images {
      tags {
        name
        lastUpdated
      }
    }
  }
}
```

**Variables:**

```json
{
  "org": "demonstrationorg",
  "repo": "node"
}
```

> **Note:** The `repo` value should be the name of the repository in the DHI catalog, not the mirrored repository name.

**Sample Response:**

```json
{
  "data": {
    "dhiRepositoryDetails": {
      "images": [
        {
          "tags": [
            {
              "name": "20.19.3-debian12-dev",
              "lastUpdated": "2025-07-04T22:28:35.128706Z"
            },
            {
              "name": "20.19-debian12-dev",
              "lastUpdated": "2025-07-04T22:28:35.683818Z"
            },
            {
              "name": "20-debian12-dev",
              "lastUpdated": "2025-07-04T22:28:36.241077Z"
            },
            {
              "name": "20.19.3-dev",
              "lastUpdated": "2025-07-04T22:28:36.786834Z"
            },
            {
              "name": "20.19-dev",
              "lastUpdated": "2025-07-04T22:28:37.361655Z"
            },
            {
              "name": "20-dev",
              "lastUpdated": "2025-07-04T22:28:37.94814Z"
            }
          ]
        },
        {
          "tags": [
            {
              "name": "20.19.3-debian12",
              "lastUpdated": "2025-07-04T22:33:15.092892Z"
            },
            {
              "name": "20.19-debian12",
              "lastUpdated": "2025-07-04T22:33:17.928816Z"
            },
            {
              "name": "20-debian12",
              "lastUpdated": "2025-07-04T22:33:19.532574Z"
            },
            {
              "name": "20.19.3",
              "lastUpdated": "2025-07-04T22:33:21.107596Z"
            },
            {
              "name": "20.19",
              "lastUpdated": "2025-07-04T22:33:22.711078Z"
            },
            {
              "name": "20",
              "lastUpdated": "2025-07-04T22:33:24.276178Z"
            }
          ]
        }
      ]
    }
  },
  "extensions": {
    "correlation_id": "633df2a2-bc29-473e-8f08-2bf8479d43f4"
  }
}
```
The image below shows how this would look.

![API Call](/assets/api_call.png)

---

