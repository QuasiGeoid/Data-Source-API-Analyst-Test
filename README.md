# Data Source API Analyst Test

This document outlines the homework assignment for the **Data Source API Analyst** role.

---

## Client Needs

The task involves working with the **GitHub API** to test the following endpoints:

1. **Search Repositories (public)**
2. **Commits**
3. **Contents**

---

## Tests for Search Repositories (public)

### 1. Response Status Code
**Purpose**:  
Ensure the server responds with the correct HTTP status code for both valid and invalid requests.

**Why**:  
- **200**: Confirms the request was processed successfully.  
- **422 or 503**: Verifies these are not returned for valid queries.

**Results**:  
✅ **Passed**

---

### 2. Validate Response Body Structure
**Purpose**:  
Verify the structure of the response matches the provided schema.

**Why**:  
- Ensures the API returns the required fields and adheres to its contract.

**Results**:  
⚠️ **Issue Identified**  
The JSON schema is not entirely correct:  
- The `pushed_at` field can be `null`, not just a string.  
  _(Example found during "Sort by updated in ascending order" test in Postman.)_

---

## Notes
- Each test is designed to evaluate both correctness and reliability of the GitHub API responses.
- Future work could involve validating additional edge cases for fields like `pushed_at`.

