# Creating a Fine-Grained Personal Access Token

## Overview

Fine-grained personal access tokens (PATs) provide enhanced security by allowing precise control over permissions and repository access.

---

## Steps to Create a Fine-Grained Personal Access Token

### 1. Verify Your Email Address
Ensure your email address associated with GitHub is verified.

### 2. Navigate to Developer Settings
1. Click your profile photo in the upper-right corner of any GitHub page.
2. Select **Settings** from the dropdown menu.
3. In the left sidebar, click **Developer settings**.

### 3. Access Fine-Grained Tokens
1. Under **Personal access tokens** in the left sidebar, click **Fine-grained tokens**.
2. Click **Generate new token**.

### 4. Configure the Token
- **Token Name**: Enter a descriptive name for the token.
- **Expiration**: Choose an expiration period for the token. Tokens with infinite lifetimes may be restricted by your organization or enterprise's maximum lifetime policy. Learn more in [Enforcing a maximum lifetime policy for PATs](https://docs.github.com).
- **Description (Optional)**: Add a note describing the token's purpose.

### 5. Set the Resource Owner
- Select the **resource owner** (you or an organization you belong to).
- Note: Organizations will only appear if they allow fine-grained PATs. For details, see [Setting a personal access token policy for your organization](https://docs.github.com).
- If the organization requires approval, provide a **justification** for the request in the designated box.

### 6. Define Repository Access
- **Repository Access**: Choose the repositories the token can access. Select the minimal access required:
  - **All repositories** or
  - **Only selected repositories**
- If selecting specific repositories, specify them in the **Selected repositories** dropdown.

### 7. Set Permissions
- Choose the **minimal permissions** required for the token. Depending on the resource owner and repository access, permissions may include:
  - **Repositories**
  - **Organizations**
  - **Accounts**
- Refer to the [REST API reference documentation](https://docs.github.com) for detailed endpoint permissions.

### 8. Generate the Token
Click **Generate token**.

### 9. Approval (If Required)
- If the token's resource owner is an organization requiring approval:
  - The token will be marked as **pending** until reviewed by an administrator.
  - Pending tokens have read-only access to public resources.
  - Organization owners’ requests are automatically approved.
  - For details, see [Reviewing and revoking PATs in your organization](https://docs.github.com).

---

## Notes
- Fine-grained PATs enable secure, granular access to repositories and other resources.
- All fine-grained PATs include read-only access to public repositories.
- For a comprehensive list of permissions and endpoint compatibility, see [Permissions required for fine-grained PATs](https://docs.github.com).
