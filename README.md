# Data Source API Analyst Test

This document outlines the homework assignment for the **Data Source API Analyst** role.

---

# Client Requirements

The client outlined the following requirements for the project:

## 1. Search Repositories (public)

**Purpose:**  
Allow users to search for public repositories based on keywords, descriptions, or other metadata.

**Features:**  
- Support for paginated results.
- Ability to sort repositories by relevance, popularity, or recency.

---

## 2. Retrieve Commits

**Purpose:**  
Provide access to the commit history of a specific repository or branch.

**Features:**
- Support for paginated results.
- Return commit messages, authors, and timestamps.  
- Support for filtering commits by date range or author.  
- Include optional details like file changes in the response.  

**Requirements:** 
- The API must handle both full SHA and truncated SHA values seamlessly.

---

## 3. Access Repository Contents

**Purpose:**  
Enable users to retrieve the contents of a repository, including files and directories.

**Features:**  
- Differentiate between files and directories in responses.  
- Support for retrieving content at a specific commit or branch using SHA.  
- Include metadata such as encoding, file size, and content type for files.

---

## 4. Error Handling

**Purpose:**  
Ensure the API delivers clear and actionable error messages.

**Requirements:**  
- Handle **404 errors** gracefully when a path does not exist.  
- Return **400 errors** for malformed requests (e.g., invalid paths).  
- Invalid access token must return a **401 error** with a descriptive message.

---

## Tests for Search Repositories (public)

### 1. Response Status Code
**Purpose**:  
Ensure the server responds with the correct HTTP status code for both valid and invalid requests.

**Why**:  
- **200**: Confirms the request was processed successfully (🧪 _"Request includes only the required query parameter"_ test in Postman).
- **422**: Verifies API's behavior with missing required or invalid value of query parameter (🧪 _"Required parameter not provided"_ test in Postman).

**Results**:
✅ **Passed**
💡 **Note:**
The API ignores invalid values for non-required query parameters (🧪 _"With invalid values of query params"_ test in Postman).

---

### 2. Response Body Structure
**Purpose**:  
Verify the structure of the response matches the provided schema.

**Why**:  
- Ensures the API returns the required fields and adheres to its contract.

**Results**:  
⚠️ **Issue Identified**  
The JSON schema is not entirely correct:  
- The `pushed_at` field can be `null`, not just a string.  
  (Example found during _"Sort by updated in ascending order"_ test in Postman)

---

### 3. Query Parameters

**Purpose:**  
To ensure the query parameters affect the results as expected (`sort`, `order`, `per_page`, `page`).

**Why:**  
- **Sorting:** Ensures the results align with the `sort` and `order` parameters (🧪 _"Sort by..."_ tests in Postman).  
- **Pagination:** Confirms the correct number of results is returned (🧪 _"per_page..."_ tests in Postman).

**Results:**  
⚠️ **Issues identified**:  
1. Sorting in ascending order by `forks` does not differentiate between repositories with a `forks_count` (`forks`) of 0 or 1 (🧪 _"Sort by forks in ascending order"_ test in Postman).  
2. The sorting order by `help-wanted-issues` is not working as expected because it mixes items with and without `help-wanted` in their `topics`. However, all repositories with the `help-wanted` topic are correctly sorted by `open_issues_count` (🧪 _"Sort by help-wanted-issues in default order"_ test in Postman).  
3. The sorting order by `updated` is not working as expected because the order based on the `updated_at` field value can be incorrect (🧪 _"Sort by updated in ascending order"_ test in Postman).

---

### 4. Headers
**Test Purpose:**  
To ensure the correct Content-Type and Link headers and `text_matches` (🧪 _"With text match metadata"_ test in Postman) field are returned.

**Why:**  
Ensures the API communicates its response format correctly.

**Results:**
✅ **Passed**

---

### 5. Performance
**Test Purpose:**  
To measure response time and ensure it meets performance benchmarks..

**Why:**  
Confirms the API performs well under normal conditions.

**Results:**
✅ **Passed**
![Screenshot from 2024-11-28 11-11-21](https://github.com/user-attachments/assets/6e1cbac5-2215-426e-bd4c-6b436d0567ee)

### 6. Restrictions

#### Purpose:
To ensure the API enforces documented restrictions effectively.

#### Why:
Verifies the API behaves as expected when constraints are exceeded, maintaining consistent performance and reliability.

#### Test Cases:

1. **Maximum Return Results**
   - **Description**: Validate that the API does not return more than 1000 results for a single query.
   - **Expected Result**: If the total number of results exceeds 1000, the API should return a status code of 422 and a meaningful error message.
   - **Test Example**: Run a query expected to return more than 1000 results.
   - **Postman Test**: 🧪 _"Search results limit exceeded"_

2. **Query param `q` Length and Operators**
   - **Description**: Validate that the API enforces restrictions on query parameters, including length and the use of logical operators.
   - **Expected Results**:
     - `q` longer than 256 characters (excluding operators or qualifiers) should result in a response with status code of 422 and a meaningful error message.
     - `q` with more than five logical operators (AND, OR, NOT) should result in a response with status code of 422 and a meaningful error message.
   - **Test Example**: Send a query that exceeds the character limit or logical operator count.
   - **Postman Tests**: 🧪 _"Query exceeds character"_ and _"Query operator limit"_

**Results:**
⚠️ Issue identified: `q` with more than five logical operators (AND, OR, NOT) results in deceptive error message: `The search contains only logical operators (AND / OR / NOT) without any search terms.`

---

## Tests for List Commits
### 1. Response Status Code
**Purpose**:  
Ensure the server responds with the correct HTTP status code for both valid and invalid requests.

**Why**:  
- **200**: Confirms the request was processed successfully (🧪 _"Required params are provided"_ test in Postman).
- **404**: Verifies API's behavior with missing or invalid required path parameters (🧪 _"owner not provided"_, _"repo not provided"_, and _"Invalid combination of owner and repo"_ tests in Postman).

**Results**:
✅ **Passed**
💡 **Note:**
The API ignores invalid values for non-required query parameters (🧪 _"With invalid values of query params"_ test in Postman).

---

### 2. Response Body Structure
**Purpose**:  
Verify the structure of the response matches the provided schema.

**Why**:  
- Ensures the API returns the required fields and adheres to its contract.

**Results**:  
⚠️ **Issue identified**: author can be null, not just object (🧪 "_Invalid schema ("author": null)"_ test in Postman).

---

### 3. Query Parameters

**Purpose:**  
To ensure the query parameters affect the results as expected.

**Why:** Ensures the results align with query parameters and confirms the correct number of results is returned.

**Results:**
🧪 _"query param ..."_ and _"per_page..."_ tests in Postman

⚠️ **Issue identified**: if the `sha` query parameter contains a typo and the `author` parameter is populated, the response returns a `500` error.

💡 **Note:** The API supports using a truncated `sha` (🧪 _"query param sha truncated"_ test in Postman)

---

### 4. Headers
**Test Purpose:**  
To ensure the correct Content-Type and Link headers and `text_matches` (🧪 _"With text match metadata"_ test in Postman) field are returned.

**Why:**  
Ensures the API communicates its response format correctly.

**Results:**
✅ **Passed**

---

### 5. Performance
**Test Purpose:**  
To measure response time and ensure it meets performance benchmarks.

**Why:**  
Confirms the API performs well under normal conditions.

**Results:**
✅ **Passed**
![Screenshot from 2024-11-28 14-09-14](https://github.com/user-attachments/assets/f95f1c5d-479c-4d9b-88ec-38158f83fab1)

---

## Tests for Get Repository Content
### 1. Response Status Code
**Purpose**:  
Ensure the server responds with the correct HTTP status code for both valid and invalid requests.

**Why**:  
- **200**: Confirms the request was processed successfully (🧪 _"Root dir"_ test in Postman for an example).
- **404**: Verifies API's behavior with missing or invalid required path parameters (🧪 _"owner not provided"_ and _"invalid ref"_ tests in Postman for an example).

**Results**:
✅ **Passed**

---

### 2. Response Body Structure
**Purpose**:  
Verify the structure of the response matches the provided schema.

**Why**:  
- Ensures the API returns the required fields and adheres to its contract.

**Results**:  
✅ **Passed**
💡 **Note:** Files and directories have slightly different schemas: directories have an array as the root element.

---

### 3. Query Parameters

**Purpose:**  
To ensure the query parameters affect the results as expected.

**Why:** Ensures the results align with query parameter.

**Results:**  
✅ **Passed**
(🧪 _"ref provided"_ and _"invalid ref"_ tests in Postman)

---

### 4. Headers
**Test Purpose:**  
To ensure the correct Content-Type and Link headers and `text_matches` (🧪 _"With text match metadata"_ test in Postman) field are returned.

**Why:**  
Ensures the API communicates its response format correctly.

**Results:**
✅ **Passed**

---

### 5. Performance
**Test Purpose:**  
To measure response time and ensure it meets performance benchmarks.

**Why:**  
Confirms the API performs well under normal conditions.

**Results:**
✅ **Passed**
![Screenshot from 2024-11-28 16-15-14](https://github.com/user-attachments/assets/c8dc4c8c-ca6c-4a9d-8a0b-18ee97abc389)

