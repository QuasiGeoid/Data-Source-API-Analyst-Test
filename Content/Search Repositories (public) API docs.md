Find repositories via various criteria. This method returns up to 100 results [per page](https://docs.github.com/rest/guides/using-pagination-in-the-rest-api).

### Headers

| Name     | Type   | Description                                                                                                                             |
|----------|--------|-----------------------------------------------------------------------------------------------------------------------------------------|
| `accept` | string | `application/vnd.github+json` - is recommended.<br>`application/vnd.github.text-match+json` - when searching for repositories, you can get text match metadata for the name and description fields when you pass the text-match media type. For more details about how to receive highlighted search results, see [Text match metadata](https://docs.github.com/rest/search/search#text-match-metadata).|

### Query Parameters

| Name       | Type      | Description                                                                                                                                                                                                          |
|------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| `q`        | string    | **Required.** The query contains one or more search keywords and qualifiers. Qualifiers allow you to limit your search to specific areas of GitHub. The REST API supports the same qualifiers as the web interface for GitHub.<br>To learn more about the format of the query, see [Constructing a search query](https://docs.github.com/en/github/searching-for-information-on-github/constructing-a-search-query). See [Searching for repositories](https://docs.github.com/en/search-github/searching-on-github/searching-for-repositories) for a detailed list of qualifiers.                                                                                                                    |
| `sort`     | string    | Sorts the results of your query by number of stars, forks, help-wanted-issues, or how recently the items were updated.<br>**Default:** `best match`<br>**Can be one of:** `stars`, `forks`, `help-wanted-issues`, `updated`                                                                                                                                                                                                                                       |
| `order`    | string    | Determines whether the first search result returned is the highest number of matches (`desc`) or lowest number of matches (`asc`).<br>This parameter is ignored unless you provide `sort`.<br>**Default:** `desc`<br>**Can be one of:** `desc`, `asc`                                                                                                                                                                                                      |
| `per_page` | integer   | The number of results per page (max 100).<br>For more information, see ["Using pagination in the REST API"](https://docs.github.com/rest/guides/using-pagination-in-the-rest-api).<br>**Default:** `30`              |
| `page`     | integer   | The page number of the results to fetch.<br>For more information, see ["Using pagination in the REST API"](https://docs.github.com/rest/guides/using-pagination-in-the-rest-api).<br>**Default:** `1`                |

### HTTP Response Status Codes for "Search Repositories"

| Status Code | Description                                                                                     |
|-------------|-------------------------------------------------------------------------------------------------|
| `200`       | OK                                                                                              |
| `304`       | Not modified                                                                                    |
| `422`       | Validation failed, or the endpoint has been spammed.                                            |
| `503`       | Service unavailable                                                                             |

### Example JSON Response
1. Success (status code 200):

```json
{
  "total_count": 40,
  "incomplete_results": false,
  "items": [
    {
      "id": 3081286,
      "node_id": "MDEwOlJlcG9zaXRvcnkzMDgxMjg2",
      "name": "Tetris",
      "full_name": "dtrupenn/Tetris",
      "owner": {
        "login": "dtrupenn",
        "id": 872147,
        "node_id": "MDQ6VXNlcjg3MjE0Nw==",
        "avatar_url": "https://secure.gravatar.com/avatar/e7956084e75f239de85d3a31bc172ace?d=https://a248.e.akamai.net/assets.github.com%2Fimages%2Fgravatars%2Fgravatar-user-420.png",
        "url": "https://api.github.com/users/dtrupenn",
        "type": "User"
      },
      "private": false,
      "html_url": "https://github.com/dtrupenn/Tetris",
      "description": "A C implementation of Tetris using Pennsim through LC4",
      "fork": false,
      "created_at": "2012-01-01T00:31:50Z",
      "updated_at": "2013-01-05T17:58:47Z",
      "pushed_at": "2012-01-01T00:37:02Z",
      "size": 524,
      "stargazers_count": 1,
      "watchers_count": 1,
      "language": "Assembly",
      "forks_count": 0,
      "open_issues_count": 0,
      "license": {
        "key": "mit",
        "name": "MIT License",
        "url": "https://api.github.com/licenses/mit"
      }
    }
  ]
}
```
2. Fail (status code 422):

```json
{
    "message": "Validation Failed",
    "errors": [
        {
            "resource": "Search",
            "field": "q",
            "code": "missing"
        }
    ],
    "documentation_url": "https://docs.github.com/v3/search",
    "status": "422"
}
```
