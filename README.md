# Data Source API Analyst Test

This document outlines the homework assignment for the **Data Source API Analyst** role.

---

## Client Needs

The task involves working with the **GitHub API** to test the following endpoints:

1. **Search Repositories (public)**
2. **List Commits**
3. **Contents**

---

## Tests for Search Repositories (public)

### 1. Response Status Code
**Purpose**:  
Ensure the server responds with the correct HTTP status code for both valid and invalid requests.

**Why**:  
- **200**: Confirms the request was processed successfully (see 🧪 _"Request includes only the required query parameter"_ test in Postman).
- **422**: Verifies API's behavior with missing required or invalid value of query parameter (see 🧪 _"Required parameter not provided"_ test in Postman).

**Results**:
✅ **Passed**
💡 **Note:**
The API ignores invalid values for non-required query parameters (see 🧪 _"With invalid values of query params"_ test in Postman).

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
  (Example found during _"Sort by updated in ascending order"_ test in Postman.)_

---

### 3. Query Parameters

**Purpose:**  
To ensure the query parameters affect the results as expected (`sort`, `order`, `per_page`, `page`).

**Why:**  
- **Sorting:** Ensures the results align with the `sort` and `order` parameters (see 🧪 _"Sort by..."_ tests in Postman).  
- **Pagination:** Confirms the correct number of results is returned (see 🧪 _"per_page..."_ tests in Postman).

**Results:**  
⚠️ **Issues identified**:  
1. Sorting in ascending order by `forks` does not differentiate between repositories with a `forks_count` (`forks`) of 0 or 1 (see 🧪 _"Sort by forks in ascending order"_ test in Postman).  
2. The sorting order by `help-wanted-issues` is not working as expected because it mixes items with and without `help-wanted` in their `topics`. However, all repositories with the `help-wanted` topic are correctly sorted by `open_issues_count` (see 🧪 _"Sort by help-wanted-issues in default order"_ test in Postman).  
3. The sorting order by `updated` is not working as expected because the order based on the `updated_at` field value can be incorrect (see 🧪 _"Sort by updated in ascending order"_ test in Postman).

---

### 4. Headers
**Test Purpose:**  
To ensure the correct Content-Type and Link headers and `text_matches` (see 🧪 _"With text match metadata"_ test in Postman) field are returned.

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
✅ **Passed**

---

## Tests for List Commits
### 1. Response Status Code
**Purpose**:  
Ensure the server responds with the correct HTTP status code for both valid and invalid requests.

**Why**:  
- **200**: Confirms the request was processed successfully (see 🧪 _"Required params are provided"_ test in Postman).
- **404**: Verifies API's behavior with missing or invalid required path parameters (see 🧪 _"owner not provided"_, _"repo not provided"_, and _"Invalid combination of owner and repo"_ tests in Postman).

**Results**:
✅ **Passed**
💡 **Note:**
The API ignores invalid values for non-required query parameters (see 🧪 _"With invalid values of query params"_ test in Postman).

---

### 2. Response Body Structure
**Purpose**:  
Verify the structure of the response matches the provided schema.

**Why**:  
- Ensures the API returns the required fields and adheres to its contract.

**Results**:  
⚠️ **Issue identified**: author can be null, not just object (see 🧪 "_Invalid schema ("author": null)"_ test in Postman).

---

### 3. Query Parameters

**Purpose:**  
To ensure the query parameters affect the results as expected.

**Why:** Ensures the results align with query parameters and confirms the correct number of results is returned.

**Results:**  
✅ **Passed**
(see 🧪 _"query param ..."_ and _"per_page..."_ tests in Postman)

---

### 4. Headers
**Test Purpose:**  
To ensure the correct Content-Type and Link headers and `text_matches` (see 🧪 _"With text match metadata"_ test in Postman) field are returned.

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
