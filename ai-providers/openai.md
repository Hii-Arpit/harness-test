# OpenAI

Connecting OpenAI brings your OpenAI spend into Harness Cloud & AI Cost Management alongside your cloud costs. The connector uses an Admin API key with read-only access to pull usage and cost data from your OpenAI account. You can analyze AI spend in Cost Explorer, attribute it with Views and Cost Categories, and govern it with budgets and anomaly detection, the same way you manage cloud costs.

### Before you begin <a href="#before-you-begin" id="before-you-begin"></a>

**OpenAI Admin API key:** An Admin API key with read-only access from your OpenAI account. The key enables Harness to ingest billing and usage data. Go to the [OpenAI Admin API keys reference](https://developers.openai.com/api/reference/resources/admin/subresources/organization/subresources/admin_api_keys/methods/create) to create one, or perform the following steps.

### Create the OpenAI Admin API key <a href="#create-the-openai-admin-api-key" id="create-the-openai-admin-api-key"></a>

1. Sign in to the OpenAI platform at [platform.openai.com](https://platform.openai.com) as an organization **Owner**. Only organization Owners can create Admin API keys.
2. Go to **Settings** → **Organization** → **Admin keys**, or open [platform.openai.com/settings/organization/admin-keys](https://platform.openai.com/settings/organization/admin-keys) directly.
3. Click **Create new Admin key** and give it a descriptive name, for example, `Harness CCM Integration`.
4. Set the permissions to **Restricted**, then grant **Read** access to the **Management API** and **Usage API** scopes.
5. Click **Create Admin key**.
6. Copy the key and store it securely. OpenAI does not display the key again after creation.

### Set up the OpenAI connector <a href="#set-up-the-openai-connector" id="set-up-the-openai-connector"></a>

To connect OpenAI, go to **Cloud & AI Cost Management** > **Account Settings** > **AI Providers**, click **AI Provider**, then select **OpenAI**.

#### Step 1: Name the connector <a href="#step-1-name-the-connector" id="step-1-name-the-connector"></a>

1. On the **Overview** panel, enter a **Name** for the connector.
2. Optionally, add a **Description** and **Tags** to organize and filter connectors.
3. Click **Continue**.

#### Step 2: Add the connector details <a href="#step-2-add-the-connector-details" id="step-2-add-the-connector-details"></a>

On the **Connector Details** panel, confirm the API endpoint and provide the OpenAI Admin API key you created in Create the OpenAI Admin API key as a Harness secret. Do the following:

1. In the **URL** field, do not change the default `https://api.openai.com/v1` unless you have a custom endpoint.
2. In the **API Key** field, click **Create or Select a Secret**.
   * To use an existing secret that contains your OpenAI Admin API key, select it from the list, then click **Apply Selected**.
   *   To add a new secret, click **New Secret Text**, enter a **Secret Name**, and provide the OpenAI Admin API key you created as the **Secret Value**.

       Go to [Add and reference text secrets](https://app.gitbook.com/s/3F2TpHXhur2QtQnORSM9/use-harness-platform/secrets/add-use-text-secrets) to review all secret creation options.
3. Click **Continue**.

#### Step 3: Verify the connection <a href="#step-3-verify-the-connection" id="step-3-verify-the-connection"></a>

On the **Connection Test** panel, Harness validates the API key against your OpenAI account. Once the verification is successful, click **Finish** to create the connector.
