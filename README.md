# scout-api-demo

This is a guide that demonstrates how the GraphQL API of Docker Scout(& DHI) can be leveraged for various automation tasks. Note that the API isn't public(no public docs) yet , and therefore has a slight potential to introduce breaking changes. However, we have permission from the product team to share this with customers who are eager to automate certain functionalities via the API. 



## How to Invoke the API

1. Generate a [PAT](https://docs.docker.com/security/for-developers/access-tokens/#create-an-access-token)(Personal Access Token) or [OAT](https://docs.docker.com/security/for-admins/access-tokens/)(Organizational Access Token) 

2. Exchange the PAT or OAT to a JWT Bearer token which provides access to the API
```
curl --location 'https://hub.docker.com/v2/auth/token' \
--header 'Content-Type: text/plain' \
--data '{
"identifier": "<ORG_NAME or DOCKER_ID>",
"secret": "<OAT or PAT>"
}'
```
This should respond with an access token as shown in the following image.

![auth token](/assets/auth_token.png)

3.  We will be using Postman to invoke the API(Since this is a GraphQL API, interacting with it is much easier with an GUI API client such as Postman)

The following API call will, list the node images available in Docker's DHI catalog.

```
URL: https://api.scout.docker.com/v1/graphql

GQL Operation: DhiRepositoryDetails

Authorization: Bearer <token>

```

Query:

```
query DhiRepositoryDetails($org: String!, $repo: String!) {
    dhiRepositoryDetails(
        context: { organization: $org }
        query: { repoName: $repo}
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

Variables:

```
{
    "org": "demonstrationorg",
    "repo": "node"
}
```

Note that the value of repo should be the name of the repo on the DHI catalog. Not the mirrored repository name. 

The Response would be somthing similar to the following

```
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
                },
             ...
    },
    "extensions": {
        "correlation_id": "633df2a2-bc29-473e-8f08-2bf8479d43f4"
    }
}
```

![api call](/assets/api_call.png)
