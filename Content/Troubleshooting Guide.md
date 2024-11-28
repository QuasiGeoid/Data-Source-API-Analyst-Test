# Troubleshooting Guide

This guide provides solutions to common issues encountered when using the API endpoints for **Search Repositories (public)**, **List Commits**, and **Get Repository Content**.

---

## General Issues

### Issue: Response is 401 Unauthorized

A **401 Unauthorized** error occurs when the API is unable to authenticate the request. This error typically indicates that the provided access token is invalid, expired, or missing the necessary permissions. Follow the steps below to handle and resolve this issue effectively.

---

**Causes**

1. **Invalid Access Token**  
   - The token provided in the `Authorization` header is incorrect.  

2. **Expired Token**  
   - The access token has passed its expiration date and is no longer valid.  

3. **Insufficient Permissions**  
   - The token does not have the necessary permissions to access the requested resource.  

4. **Missing Authorization Header**  
   - The `Authorization` header is not included in the request or is formatted incorrectly.  

5. **Token Revocation**  
   - The token has been manually revoked or disabled.  

---

**Solutions**

**1. Verify the Token**

- **Check Token Validity:** Ensure the token is copied correctly and has not been modified.  
- **Regenerate the Token:** If the token is invalid, generate a new token. Follow the instructions for [Creating a Fine-Grained Personal Access Token](https://github.com/QuasiGeoid/Data-Source-API-Analyst-Test/blob/main/Content/Creating%20a%20Fine-Grained%20Personal%20Access%20Token.md).  

**2. Include the Authorization Header**

Ensure your API request includes the `Authorization` header in the correct format:  
```http
Authorization: Bearer <your_token>
```

---

### Issue: API throttling or rate-limiting
**Causes**  
Too many requests sent in a short period.  

**Solutions**  
1. Reduce the frequency of API calls.  
2. Implement exponential backoff in your application to retry requests.  
3. Monitor your API usage and optimize requests to avoid rate limits.

---

## Search Repositories (public)

### Issue: Response is 422 with the message: "The search is longer than 256 characters."

**Causes**  
The query parameter `q` exceeds the maximum allowed length of **256 characters**, which violates the API's constraints.

**Solutions**  
1. **Simplify the Query**  
   - Reduce the number of keywords or filters in the query.
   - Focus on the most relevant terms.

2. **Use Proper Encoding**  
   - Ensure special characters are URL-encoded to avoid unnecessary length.  
     Example: Use `%20` for spaces and `%3A` for colons.

3. **Break Down the Query**  
   - If your query is inherently complex, split it into multiple shorter queries with different focus areas.

---

### Issue: Response is 422 with the message: "The search contains only logical operators (AND / OR / NOT) without any search terms."

**Causes**  
The query parameter `q` contains more than **five logical operators (AND, OR, NOT)**, violating the API's constraints.

**Solutions**  
1. **Simplify the Query**  
   - Reduce the number of logical operators in the query.
   - Focus on the most relevant terms.

2. **Break Down the Query**  
   - If your query is inherently complex, split it into multiple shorter queries with different focus areas.
  
---

## List Commits

### Issue: Response is 404
**Causes**  
The `owner` or `repository` path parameters are incorrect.

**Solutions**  
Verify the `owner` and `repository` path parameters.

---

### Issue: Commits are not returned
**Causes**  
The `path`, `since`, `until` or `author` query parameter is incorrect.

**Solutions**  
Verify the `path`, `since`, `until` and `author` query parameter.

### Issue: Response is 500
**Causes**  
The `sha` query parameter is incorrect, even though the `author` is valid.

**Solutions**  
Ensure the `sha` query parameter is accurate and corresponds to a valid commit hash.

---

## Get Repository Content

### Issue: Response is 404 with `"message":"Not Found"`
**Causes**  
The `owner`, `repo` or `path` path parameter is not provided or incorrect.

**Solutions**  
Verify the `owner`, `repo` and `path` path parameters.

---

### Issue: Response is 404 with `"message": "No commit found for the ref {ref value}"`,
**Causes**  
The `ref` query parameter is incorrect.

**Solutions**  
Verify the `ref` query parameter.

---
