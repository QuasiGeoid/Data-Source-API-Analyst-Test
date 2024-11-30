# Get Repository Content

Gets the contents of a file or directory in a repository. Specify the file path or directory with the `path` parameter. If you omit the `path` parameter, you will receive the contents of the repository's root directory.

### Endpoint

GET /repos/{owner}/{repo}/contents/{path}

### Behavior:
- If the content is a directory, the response will be an array of objects, one object for each item in the directory. 
  - When listing the contents of a directory, submodules have their `"type"` specified as `"file"`. Logically, the value should be `"submodule"`. This behavior exists for backwards compatibility purposes. In the next major version of the API, the type will be returned as `"submodule"`.
- If the content is a symlink and the symlink's target is a normal file in the repository, then the API responds with the content of the file.
  - Otherwise, the API responds with an object describing the symlink itself.
- If the content is a submodule, the `submodule_git_url` field identifies the location of the submodule repository, and the `sha` identifies a specific commit within the submodule repository. Git uses the given URL when cloning the submodule repository and checks out the submodule at that specific commit.

### Notes:
- To get a repository's contents recursively, you can recursively get the tree.
- This API has an upper limit of 1,000 files for a directory. If you need to retrieve more files, use the Git Trees API.
- Download URLs expire and are meant to be used just once. To ensure the download URL does not expire, please use the contents API to obtain a fresh download URL for each download.
- **File size limitations**:
  - **1 MB or smaller**: All features of this endpoint are supported.
  - **Between 1-100 MB**: Only the `raw` or `object` custom media types are supported. The content field will be empty in the `object` media type, and the encoding field will be `"none"`. Use the `raw` media type for these larger files.
  - **Greater than 100 MB**: This endpoint is not supported.

### Headers
| Name    | Type   | Description                                                      |
|---------|--------|------------------------------------------------------------------|
| accept  | string | `application/vnd.github+json` - is recommended.<br>`application/vnd.github.raw+json` - returns the raw file contents for files and symlinks.<br>`application/vnd.github.html+json` - returns the file contents in HTML. Markup languages are rendered to HTML using GitHub's open-source Markup library.<br>`application/vnd.github.object+json` - returns the contents in a consistent object format regardless of the content type. For example, instead of an array of objects for a directory, the response will be an object with an `entries` attribute containing the array of objects.|

### Parameters
#### Path
| Name  | Type   | Description                                                            |
|-------|--------|------------------------------------------------------------------------|
| owner | string | Required. The account owner of the repository. The name is not case sensitive. |
| repo  | string | Required. The name of the repository without the `.git` extension. The name is not case sensitive. |
| path  | string | Required. The file path or directory in the repository. |

#### Query
| Name  | Type   | Description                                                      |
|-------|--------|------------------------------------------------------------------|
| ref   | string | The name of the commit/branch/tag. Default: the repository’s default branch. |

### HTTP Response Status Codes

| Status Code | Description             |
|-------------|-------------------------|
| 200         | OK                      |
| 302         | Found                   |
| 304         | Not modified            |
| 403         | Forbidden               |
| 404         | Resource not found      |

### Request Example

```bash
curl -L \
  -H "Accept: application/vnd.github+json" \
  -H "Authorization: Bearer <YOUR-TOKEN>" \
  -H "X-GitHub-Api-Version: 2022-11-28" \
  https://api.github.com/repos/:owner/:repo/contents/:path
```

### Response

#### Example
```json
{
  "type": "file",
  "encoding": "base64",
  "size": 5362,
  "name": "README.md",
  "path": "README.md",
  "content": "IyBZb2dhIEJvmsgaW4gcHJvZ3Jlc3MhIEZlZWwgdAoKOndhcm5pbmc6IFdvc\\nZnJlZSBmUgdG8gY0byBjaGVjayBvdXQgdGhlIGFwcCwgYnV0IGJlIHN1c29t\\nZSBiYWNrIG9uY2UgaXQgaXMgY29tcGxldGUuCgpBIHdlYiBhcHAgdGhhdCBs\\nZWFkcyB5b3UgdGhyb3VnaCBhIHlvZ2Egc2Vzc2lvbi4KCltXb3Jrb3V0IG5v\\ndyFdKGh0dHBzOi8vc2tlZHdhcmRzODguZ2l0aHViLmlvL3lvZ2EvKQoKPGlt\\nZyBzcmM9InNyYy9pbWFnZXMvbWFza2FibGVfaWNvbl81MTIucG5nIiBhbHQ9\\nImJvdCBsaWZ0aW5nIHdlaWdodHMiIHdpZHRoPSIxMDAiLz4KCkRvIHlvdSBo\\nYXZlIGZlZWRiYWNrIG9yIGlkZWFzIGZvciBpbXByb3ZlbWVudD8gW09wZW4g\\nYW4gaXNzdWVdKGh0dHBzOi8vZ2l0aHViLmNvbS9za2Vkd2FyZHM4OC95b2dh\\nL2lzc3Vlcy9uZXcpLgoKV2FudCBtb3JlIGdhbWVzPyBWaXNpdCBbQ25TIEdh\\nbWVzXShodHRwczovL3NrZWR3YXJkczg4LmdpdGh1Yi5pby9wb3J0Zm9saW8v\\nKS4KCiMjIERldmVsb3BtZW50CgpUbyBhZGQgYSBuZXcgcG9zZSwgYWRkIGFu\\nIGVudHJ5IHRvIHRoZSByZWxldmFudCBmaWxlIGluIGBzcmMvYXNhbmFzYC4K\\nClRvIGJ1aWxkLCBydW4gYG5wbSBydW4gYnVpbGRgLgoKVG8gcnVuIGxvY2Fs\\nbHkgd2l0aCBsaXZlIHJlbG9hZGluZyBhbmQgbm8gc2VydmljZSB3b3JrZXIs\\nIHJ1biBgbnBtIHJ1biBkZXZgLiAoSWYgYSBzZXJ2aWNlIHdvcmtlciB3YXMg\\ncHJldmlvdXNseSByZWdpc3RlcmVkLCB5b3UgY2FuIHVucmVnaXN0ZXIgaXQg\\naW4gY2hyb21lIGRldmVsb3BlciB0b29sczogYEFwcGxpY2F0aW9uYCA+IGBT\\nZXJ2aWNlIHdvcmtlcnNgID4gYFVucmVnaXN0ZXJgLikKClRvIHJ1biBsb2Nh\\nbGx5IGFuZCByZWdpc3RlciB0aGUgc2VydmljZSB3b3JrZXIsIHJ1biBgbnBt\\nIHN0YXJ0YC4KClRvIGRlcGxveSwgcHVzaCB0byBgbWFpbmAgb3IgbWFudWFs\\nbHkgdHJpZ2dlciB0aGUgYC5naXRodWIvd29ya2Zsb3dzL2RlcGxveS55bWxg\\nIHdvcmtmbG93Lgo=\\n",
  "sha": "3d21ec53a331a6f037a91c368710b99387d012c1",
  "url": "https://api.github.com/repos/octokit/octokit.rb/contents/README.md",
  "git_url": "https://api.github.com/repos/octokit/octokit.rb/git/blobs/3d21ec53a331a6f037a91c368710b99387d012c1",
  "html_url": "https://github.com/octokit/octokit.rb/blob/master/README.md",
  "download_url": "https://raw.githubusercontent.com/octokit/octokit.rb/master/README.md",
  "_links": {
    "git": "https://api.github.com/repos/octokit/octokit.rb/git/blobs/3d21ec53a331a6f037a91c368710b99387d012c1",
    "self": "https://api.github.com/repos/octokit/octokit.rb/contents/README.md",
    "html": "https://github.com/octokit/octokit.rb/blob/master/README.md"
  }
}
```

#### Schema
```json
{
  "title": "Content Tree",
  "description": "Content Tree",
  "type": "object",
  "properties": {
    "type": {
      "type": "string"
    },
    "size": {
      "type": "integer"
    },
    "name": {
      "type": "string"
    },
    "path": {
      "type": "string"
    },
    "sha": {
      "type": "string"
    },
    "content": {
      "type": "string"
    },
    "url": {
      "type": "string",
      "format": "uri"
    },
    "git_url": {
      "type": [
        "string",
        "null"
      ],
      "format": "uri"
    },
    "html_url": {
      "type": [
        "string",
        "null"
      ],
      "format": "uri"
    },
    "download_url": {
      "type": [
        "string",
        "null"
      ],
      "format": "uri"
    },
    "entries": {
      "type": "array",
      "items": {
        "type": "object",
        "properties": {
          "type": {
            "type": "string"
          },
          "size": {
            "type": "integer"
          },
          "name": {
            "type": "string"
          },
          "path": {
            "type": "string"
          },
          "sha": {
            "type": "string"
          },
          "url": {
            "type": "string",
            "format": "uri"
          },
          "git_url": {
            "type": [
              "string",
              "null"
            ],
            "format": "uri"
          },
          "html_url": {
            "type": [
              "string",
              "null"
            ],
            "format": "uri"
          },
          "download_url": {
            "type": [
              "string",
              "null"
            ],
            "format": "uri"
          },
          "_links": {
            "type": "object",
            "properties": {
              "git": {
                "type": [
                  "string",
                  "null"
                ],
                "format": "uri"
              },
              "html": {
                "type": [
                  "string",
                  "null"
                ],
                "format": "uri"
              },
              "self": {
                "type": "string",
                "format": "uri"
              }
            },
            "required": [
              "git",
              "html",
              "self"
            ]
          }
        },
        "required": [
          "_links",
          "git_url",
          "html_url",
          "download_url",
          "name",
          "path",
          "sha",
          "size",
          "type",
          "url"
        ]
      }
    },
    "_links": {
      "type": "object",
      "properties": {
        "git": {
          "type": [
            "string",
            "null"
          ],
          "format": "uri"
        },
        "html": {
          "type": [
            "string",
            "null"
          ],
          "format": "uri"
        },
        "self": {
          "type": "string",
          "format": "uri"
        }
      },
      "required": [
        "git",
        "html",
        "self"
      ]
    }
  },
  "required": [
    "_links",
    "git_url",
    "html_url",
    "download_url",
    "name",
    "path",
    "sha",
    "size",
    "type",
    "url"
  ]
}
```
