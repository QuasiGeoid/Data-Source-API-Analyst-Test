# GitHub REST API: List Commits

Allows you to retrieve a list of commits for a repository, with support for filtering by author, branch, or time range.

### Endpoint

GET /repos/{owner}/{repo}/commits

### Headers

| Name     | Type   | Description                                                                                                                             |
|----------|--------|-----------------------------------------------------------------------------------------------------------------------------------------|
| `accept` | string | `application/vnd.github+json` - is recommended.

### Parameters

#### Path Parameters
| Name   | Type   | Description                                                                                                  |
|--------|--------|--------------------------------------------------------------------------------------------------------------|
| `owner` | string | **Required**. The account owner of the repository. The name is not case sensitive.                          |
| `repo`  | string | **Required**. The name of the repository without the `.git` extension. The name is not case sensitive.      |

#### Query Parameters
| Name        | Type     | Description                                                                                                                                                              |
|-------------|----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `sha`       | string   | SHA or branch to start listing commits from. **Default**: the repository’s default branch (usually `main`).                                                               |
| `path`      | string   | Only commits containing this file path will be returned.                                                                                                                  |
| `author`    | string   | GitHub username or email address to use to filter by commit author.                                                                                                       |
| `committer` | string   | GitHub username or email address to use to filter by commit committer.                                                                                                    |
| `since`     | string   | Only show results that were last updated after the given time. This is a timestamp in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format: `YYYY-MM-DDTHH:MM:SSZ`. Due to Git limitations, timestamps must be between `1970-01-01` and `2099-12-31` (inclusive), or unexpected results may occur. |
| `until`     | string   | Only commits before this date will be returned. This is a timestamp in [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601) format: `YYYY-MM-DDTHH:MM:SSZ`. Due to Git limitations, timestamps must be between `1970-01-01` and `2099-12-31` (inclusive), or unexpected results may occur. |
| `per_page`  | integer  | The number of results per page (**max 100**). For more information, see ["Using pagination in the REST API"](https://docs.github.com/en/rest/guides/using-pagination-in-the-rest-api). **Default**: `30`. |
| `page`      | integer  | The page number of the results to fetch. For more information, see ["Using pagination in the REST API"](https://docs.github.com/en/rest/guides/using-pagination-in-the-rest-api). **Default**: `1`. |

### HTTP Response Status Codes

| Status Code | Description          |
|-------------|----------------------|
| `200`       | OK                   |
| `400`       | Bad Request          |
| `404`       | Resource not found   |
| `409`       | Conflict             |
| `500`       | Internal Error       |

### Request Example

```bash
curl -L \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2022-11-28" \
  https://api.github.com/repos/OWNER/REPO/commits
```

### Response

#### Signature Verification Object
The response will include a `verification` object that describes the result of verifying the commit's signature. The following fields are included in the `verification` object:

| Name        | Type    | Description                                                                 |
|-------------|---------|-----------------------------------------------------------------------------|
| `verified`  | boolean | Indicates whether GitHub considers the signature in this commit to be verified. |
| `reason`    | string  | The reason for the `verified` value. Possible values and their meanings are enumerated in the table below. |
| `signature` | string  | The signature that was extracted from the commit.                           |
| `payload`   | string  | The value that was signed.                                                  |

#### Possible Values for `reason` in the Verification Object

| Value                   | Description                                                                                          |
|-------------------------|------------------------------------------------------------------------------------------------------|
| `expired_key`           | The key that made the signature is expired.                                                         |
| `not_signing_key`       | The "signing" flag is not among the usage flags in the GPG key that made the signature.              |
| `gpgverify_error`       | There was an error communicating with the signature verification service.                           |
| `gpgverify_unavailable` | The signature verification service is currently unavailable.                                         |
| `unsigned`              | The object does not include a signature.                                                            |
| `unknown_signature_type`| A non-PGP signature was found in the commit.                                                        |
| `no_user`               | No user was associated with the committer email address in the commit.                              |
| `unverified_email`      | The committer email address in the commit was associated with a user, but the email address is not verified on their account. |
| `bad_email`             | The committer email address in the commit is not included in the identities of the PGP key that made the signature. |
| `unknown_key`           | The key that made the signature has not been registered with any user's account.                    |
| `malformed_signature`   | There was an error parsing the signature.                                                           |
| `invalid`               | The signature could not be cryptographically verified using the key whose key-id was found in the signature. |
| `valid`                 | None of the above errors applied, so the signature is considered to be verified.                    |

#### Response Example
```json
[
  {
    "url": "https://api.github.com/repos/octocat/Hello-World/commits/6dcb09b5b57875f334f61aebed695e2e4193db5e",
    "sha": "6dcb09b5b57875f334f61aebed695e2e4193db5e",
    "node_id": "MDY6Q29tbWl0NmRjYjA5YjViNTc4NzVmMzM0ZjYxYWViZWQ2OTVlMmU0MTkzZGI1ZQ==",
    "html_url": "https://github.com/octocat/Hello-World/commit/6dcb09b5b57875f334f61aebed695e2e4193db5e",
    "comments_url": "https://api.github.com/repos/octocat/Hello-World/commits/6dcb09b5b57875f334f61aebed695e2e4193db5e/comments",
    "commit": {
      "url": "https://api.github.com/repos/octocat/Hello-World/git/commits/6dcb09b5b57875f334f61aebed695e2e4193db5e",
      "author": {
        "name": "Monalisa Octocat",
        "email": "support@github.com",
        "date": "2011-04-14T16:00:49Z"
      },
      "committer": {
        "name": "Monalisa Octocat",
        "email": "support@github.com",
        "date": "2011-04-14T16:00:49Z"
      },
      "message": "Fix all the bugs",
      "tree": {
        "url": "https://api.github.com/repos/octocat/Hello-World/tree/6dcb09b5b57875f334f61aebed695e2e4193db5e",
        "sha": "6dcb09b5b57875f334f61aebed695e2e4193db5e"
      },
      "comment_count": 0,
      "verification": {
        "verified": false,
        "reason": "unsigned",
        "signature": null,
        "payload": null
      }
    },
    "author": {
      "login": "octocat",
      "id": 1,
      "node_id": "MDQ6VXNlcjE=",
      "avatar_url": "https://github.com/images/error/octocat_happy.gif",
      "gravatar_id": "",
      "url": "https://api.github.com/users/octocat",
      "html_url": "https://github.com/octocat",
      "followers_url": "https://api.github.com/users/octocat/followers",
      "following_url": "https://api.github.com/users/octocat/following{/other_user}",
      "gists_url": "https://api.github.com/users/octocat/gists{/gist_id}",
      "starred_url": "https://api.github.com/users/octocat/starred{/owner}{/repo}",
      "subscriptions_url": "https://api.github.com/users/octocat/subscriptions",
      "organizations_url": "https://api.github.com/users/octocat/orgs",
      "repos_url": "https://api.github.com/users/octocat/repos",
      "events_url": "https://api.github.com/users/octocat/events{/privacy}",
      "received_events_url": "https://api.github.com/users/octocat/received_events",
      "type": "User",
      "site_admin": false
    },
    "committer": {
      "login": "octocat",
      "id": 1,
      "node_id": "MDQ6VXNlcjE=",
      "avatar_url": "https://github.com/images/error/octocat_happy.gif",
      "gravatar_id": "",
      "url": "https://api.github.com/users/octocat",
      "html_url": "https://github.com/octocat",
      "followers_url": "https://api.github.com/users/octocat/followers",
      "following_url": "https://api.github.com/users/octocat/following{/other_user}",
      "gists_url": "https://api.github.com/users/octocat/gists{/gist_id}",
      "starred_url": "https://api.github.com/users/octocat/starred{/owner}{/repo}",
      "subscriptions_url": "https://api.github.com/users/octocat/subscriptions",
      "organizations_url": "https://api.github.com/users/octocat/orgs",
      "repos_url": "https://api.github.com/users/octocat/repos",
      "events_url": "https://api.github.com/users/octocat/events{/privacy}",
      "received_events_url": "https://api.github.com/users/octocat/received_events",
      "type": "User",
      "site_admin": false
    },
    "parents": [
      {
        "url": "https://api.github.com/repos/octocat/Hello-World/commits/6dcb09b5b57875f334f61aebed695e2e4193db5e",
        "sha": "6dcb09b5b57875f334f61aebed695e2e4193db5e"
      }
    ]
  }
]
```

#### Response Schema
```json
{
  "type": "array",
  "items": {
    "title": "Commit",
    "description": "Commit",
    "type": "object",
    "properties": {
      "url": {
        "type": "string",
        "format": "uri",
        "examples": [
          "https://api.github.com/repos/octocat/Hello-World/commits/6dcb09b5b57875f334f61aebed695e2e4193db5e"
        ]
      },
      "sha": {
        "type": "string",
        "examples": [
          "6dcb09b5b57875f334f61aebed695e2e4193db5e"
        ]
      },
      "node_id": {
        "type": "string",
        "examples": [
          "MDY6Q29tbWl0NmRjYjA5YjViNTc4NzVmMzM0ZjYxYWViZWQ2OTVlMmU0MTkzZGI1ZQ=="
        ]
      },
      "html_url": {
        "type": "string",
        "format": "uri",
        "examples": [
          "https://github.com/octocat/Hello-World/commit/6dcb09b5b57875f334f61aebed695e2e4193db5e"
        ]
      },
      "comments_url": {
        "type": "string",
        "format": "uri",
        "examples": [
          "https://api.github.com/repos/octocat/Hello-World/commits/6dcb09b5b57875f334f61aebed695e2e4193db5e/comments"
        ]
      },
      "commit": {
        "type": "object",
        "properties": {
          "url": {
            "type": "string",
            "format": "uri",
            "examples": [
              "https://api.github.com/repos/octocat/Hello-World/commits/6dcb09b5b57875f334f61aebed695e2e4193db5e"
            ]
          },
          "author": {
            "anyOf": [
              {
                "type": "null"
              },
              {
                "title": "Git User",
                "description": "Metaproperties for Git author/committer information.",
                "type": "object",
                "properties": {
                  "name": {
                    "type": "string",
                    "examples": [
                      "\"Chris Wanstrath\""
                    ]
                  },
                  "email": {
                    "type": "string",
                    "examples": [
                      "\"chris@ozmm.org\""
                    ]
                  },
                  "date": {
                    "type": "string",
                    "examples": [
                      "\"2007-10-29T02:42:39.000-07:00\""
                    ]
                  }
                }
              }
            ]
          },
          "committer": {
            "anyOf": [
              {
                "type": "null"
              },
              {
                "title": "Git User",
                "description": "Metaproperties for Git author/committer information.",
                "type": "object",
                "properties": {
                  "name": {
                    "type": "string",
                    "examples": [
                      "\"Chris Wanstrath\""
                    ]
                  },
                  "email": {
                    "type": "string",
                    "examples": [
                      "\"chris@ozmm.org\""
                    ]
                  },
                  "date": {
                    "type": "string",
                    "examples": [
                      "\"2007-10-29T02:42:39.000-07:00\""
                    ]
                  }
                }
              }
            ]
          },
          "message": {
            "type": "string",
            "examples": [
              "Fix all the bugs"
            ]
          },
          "comment_count": {
            "type": "integer",
            "examples": [
              0
            ]
          },
          "tree": {
            "type": "object",
            "properties": {
              "sha": {
                "type": "string",
                "examples": [
                  "827efc6d56897b048c772eb4087f854f46256132"
                ]
              },
              "url": {
                "type": "string",
                "format": "uri",
                "examples": [
                  "https://api.github.com/repos/octocat/Hello-World/tree/827efc6d56897b048c772eb4087f854f46256132"
                ]
              }
            },
            "required": [
              "sha",
              "url"
            ]
          },
          "verification": {
            "title": "Verification",
            "type": "object",
            "properties": {
              "verified": {
                "type": "boolean"
              },
              "reason": {
                "type": "string"
              },
              "payload": {
                "type": [
                  "string",
                  "null"
                ]
              },
              "signature": {
                "type": [
                  "string",
                  "null"
                ]
              },
              "verified_at": {
                "type": [
                  "string",
                  "null"
                ]
              }
            },
            "required": [
              "verified",
              "reason",
              "payload",
              "signature"
            ]
          }
        },
        "required": [
          "author",
          "committer",
          "comment_count",
          "message",
          "tree",
          "url"
        ]
      },
      "author": {
        "oneOf": [
          {
            "title": "Simple User",
            "description": "A GitHub user.",
            "type": "object",
            "properties": {
              "name": {
                "type": [
                  "string",
                  "null"
                ]
              },
              "email": {
                "type": [
                  "string",
                  "null"
                ]
              },
              "login": {
                "type": "string",
                "examples": [
                  "octocat"
                ]
              },
              "id": {
                "type": "integer",
                "format": "int64",
                "examples": [
                  1
                ]
              },
              "node_id": {
                "type": "string",
                "examples": [
                  "MDQ6VXNlcjE="
                ]
              },
              "avatar_url": {
                "type": "string",
                "format": "uri",
                "examples": [
                  "https://github.com/images/error/octocat_happy.gif"
                ]
              },
              "gravatar_id": {
                "type": [
                  "string",
                  "null"
                ],
                "examples": [
                  "41d064eb2195891e12d0413f63227ea7"
                ]
              },
              "url": {
                "type": "string",
                "format": "uri",
                "examples": [
                  "https://api.github.com/users/octocat"
                ]
              },
              "html_url": {
                "type": "string",
                "format": "uri",
                "examples": [
                  "https://github.com/octocat"
                ]
              },
              "followers_url": {
                "type": "string",
                "format": "uri",
                "examples": [
                  "https://api.github.com/users/octocat/followers"
                ]
              },
              "following_url": {
                "type": "string",
                "examples": [
                  "https://api.github.com/users/octocat/following{/other_user}"
                ]
              },
              "gists_url": {
                "type": "string",
                "examples": [
                  "https://api.github.com/users/octocat/gists{/gist_id}"
                ]
              },
              "starred_url": {
                "type": "string",
                "examples": [
                  "https://api.github.com/users/octocat/starred{/owner}{/repo}"
                ]
              },
              "subscriptions_url": {
                "type": "string",
                "format": "uri",
                "examples": [
                  "https://api.github.com/users/octocat/subscriptions"
                ]
              },
              "organizations_url": {
                "type": "string",
                "format": "uri",
                "examples": [
                  "https://api.github.com/users/octocat/orgs"
                ]
              },
              "repos_url": {
                "type": "string",
                "format": "uri",
                "examples": [
                  "https://api.github.com/users/octocat/repos"
                ]
              },
              "events_url": {
                "type": "string",
                "examples": [
                  "https://api.github.com/users/octocat/events{/privacy}"
                ]
              },
              "received_events_url": {
                "type": "string",
                "format": "uri",
                "examples": [
                  "https://api.github.com/users/octocat/received_events"
                ]
              },
              "type": {
                "type": "string",
                "examples": [
                  "User"
                ]
              },
              "site_admin": {
                "type": "boolean"
              },
              "starred_at": {
                "type": "string",
                "examples": [
                  "\"2020-07-09T00:17:55Z\""
                ]
              },
              "user_view_type": {
                "type": "string",
                "examples": [
                  "public"
                ]
              }
            },
            "required": [
              "avatar_url",
              "events_url",
              "followers_url",
              "following_url",
              "gists_url",
              "gravatar_id",
              "html_url",
              "id",
              "node_id",
              "login",
              "organizations_url",
              "received_events_url",
              "repos_url",
              "site_admin",
              "starred_url",
              "subscriptions_url",
              "type",
              "url"
            ]
          },
          {
            "title": "Empty Object",
            "description": "An object without any properties.",
            "type": "object",
            "properties": {},
            "additionalProperties": false
          }
        ],
        "type": [
          "null",
          "object"
        ]
      },
      "committer": {
        "oneOf": [
          {
            "title": "Simple User",
            "description": "A GitHub user.",
            "type": "object",
            "properties": {
              "name": {
                "type": [
                  "string",
                  "null"
                ]
              },
              "email": {
                "type": [
                  "string",
                  "null"
                ]
              },
              "login": {
                "type": "string",
                "examples": [
                  "octocat"
                ]
              },
              "id": {
                "type": "integer",
                "format": "int64",
                "examples": [
                  1
                ]
              },
              "node_id": {
                "type": "string",
                "examples": [
                  "MDQ6VXNlcjE="
                ]
              },
              "avatar_url": {
                "type": "string",
                "format": "uri",
                "examples": [
                  "https://github.com/images/error/octocat_happy.gif"
                ]
              },
              "gravatar_id": {
                "type": [
                  "string",
                  "null"
                ],
                "examples": [
                  "41d064eb2195891e12d0413f63227ea7"
                ]
              },
              "url": {
                "type": "string",
                "format": "uri",
                "examples": [
                  "https://api.github.com/users/octocat"
                ]
              },
              "html_url": {
                "type": "string",
                "format": "uri",
                "examples": [
                  "https://github.com/octocat"
                ]
              },
              "followers_url": {
                "type": "string",
                "format": "uri",
                "examples": [
                  "https://api.github.com/users/octocat/followers"
                ]
              },
              "following_url": {
                "type": "string",
                "examples": [
                  "https://api.github.com/users/octocat/following{/other_user}"
                ]
              },
              "gists_url": {
                "type": "string",
                "examples": [
                  "https://api.github.com/users/octocat/gists{/gist_id}"
                ]
              },
              "starred_url": {
                "type": "string",
                "examples": [
                  "https://api.github.com/users/octocat/starred{/owner}{/repo}"
                ]
              },
              "subscriptions_url": {
                "type": "string",
                "format": "uri",
                "examples": [
                  "https://api.github.com/users/octocat/subscriptions"
                ]
              },
              "organizations_url": {
                "type": "string",
                "format": "uri",
                "examples": [
                  "https://api.github.com/users/octocat/orgs"
                ]
              },
              "repos_url": {
                "type": "string",
                "format": "uri",
                "examples": [
                  "https://api.github.com/users/octocat/repos"
                ]
              },
              "events_url": {
                "type": "string",
                "examples": [
                  "https://api.github.com/users/octocat/events{/privacy}"
                ]
              },
              "received_events_url": {
                "type": "string",
                "format": "uri",
                "examples": [
                  "https://api.github.com/users/octocat/received_events"
                ]
              },
              "type": {
                "type": "string",
                "examples": [
                  "User"
                ]
              },
              "site_admin": {
                "type": "boolean"
              },
              "starred_at": {
                "type": "string",
                "examples": [
                  "\"2020-07-09T00:17:55Z\""
                ]
              },
              "user_view_type": {
                "type": "string",
                "examples": [
                  "public"
                ]
              }
            },
            "required": [
              "avatar_url",
              "events_url",
              "followers_url",
              "following_url",
              "gists_url",
              "gravatar_id",
              "html_url",
              "id",
              "node_id",
              "login",
              "organizations_url",
              "received_events_url",
              "repos_url",
              "site_admin",
              "starred_url",
              "subscriptions_url",
              "type",
              "url"
            ]
          },
          {
            "title": "Empty Object",
            "description": "An object without any properties.",
            "type": "object",
            "properties": {},
            "additionalProperties": false
          }
        ],
        "type": [
          "null",
          "object"
        ]
      },
      "parents": {
        "type": "array",
        "items": {
          "type": "object",
          "properties": {
            "sha": {
              "type": "string",
              "examples": [
                "7638417db6d59f3c431d3e1f261cc637155684cd"
              ]
            },
            "url": {
              "type": "string",
              "format": "uri",
              "examples": [
                "https://api.github.com/repos/octocat/Hello-World/commits/7638417db6d59f3c431d3e1f261cc637155684cd"
              ]
            },
            "html_url": {
              "type": "string",
              "format": "uri",
              "examples": [
                "https://github.com/octocat/Hello-World/commit/7638417db6d59f3c431d3e1f261cc637155684cd"
              ]
            }
          },
          "required": [
            "sha",
            "url"
          ]
        }
      },
      "stats": {
        "type": "object",
        "properties": {
          "additions": {
            "type": "integer"
          },
          "deletions": {
            "type": "integer"
          },
          "total": {
            "type": "integer"
          }
        }
      },
      "files": {
        "type": "array",
        "items": {
          "title": "Diff Entry",
          "description": "Diff Entry",
          "type": "object",
          "properties": {
            "sha": {
              "type": "string",
              "examples": [
                "bbcd538c8e72b8c175046e27cc8f907076331401"
              ]
            },
            "filename": {
              "type": "string",
              "examples": [
                "file1.txt"
              ]
            },
            "status": {
              "type": "string",
              "enum": [
                "added",
                "removed",
                "modified",
                "renamed",
                "copied",
                "changed",
                "unchanged"
              ],
              "examples": [
                "added"
              ]
            },
            "additions": {
              "type": "integer",
              "examples": [
                103
              ]
            },
            "deletions": {
              "type": "integer",
              "examples": [
                21
              ]
            },
            "changes": {
              "type": "integer",
              "examples": [
                124
              ]
            },
            "blob_url": {
              "type": "string",
              "format": "uri",
              "examples": [
                "https://github.com/octocat/Hello-World/blob/6dcb09b5b57875f334f61aebed695e2e4193db5e/file1.txt"
              ]
            },
            "raw_url": {
              "type": "string",
              "format": "uri",
              "examples": [
                "https://github.com/octocat/Hello-World/raw/6dcb09b5b57875f334f61aebed695e2e4193db5e/file1.txt"
              ]
            },
            "contents_url": {
              "type": "string",
              "format": "uri",
              "examples": [
                "https://api.github.com/repos/octocat/Hello-World/contents/file1.txt?ref=6dcb09b5b57875f334f61aebed695e2e4193db5e"
              ]
            },
            "patch": {
              "type": "string",
              "examples": [
                "@@ -132,7 +132,7 @@ module Test @@ -1000,7 +1000,7 @@ module Test"
              ]
            },
            "previous_filename": {
              "type": "string",
              "examples": [
                "file.txt"
              ]
            }
          },
          "required": [
            "additions",
            "blob_url",
            "changes",
            "contents_url",
            "deletions",
            "filename",
            "raw_url",
            "sha",
            "status"
          ]
        }
      }
    },
    "required": [
      "url",
      "sha",
      "node_id",
      "html_url",
      "comments_url",
      "commit",
      "author",
      "committer",
      "parents"
    ]
  }
}
```
