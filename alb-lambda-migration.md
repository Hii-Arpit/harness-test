---
description: >-
  Upgrade the AutoStopping Lambda function for AWS connector accounts using
  ALB-based traffic rules.
---

# Migrate AutoStopping Lambda to Authenticated Warm-up API

When you set up an ALB-based AutoStopping rule for EC2 or ECS, Harness deploys a Lambda function in your AWS account. The Lambda function handles warm-ups. When a stopped resource receives traffic, it starts the resource before routing the request.

This migration adds an extra security layer to your AutoStopping warm-up flow. The migration invokes a Harness API to update the Lambda function code with authentication. Migrate one load balancer first, verify that warm-up works as expected, and then migrate the remaining load balancers.

Proxy-based rules, Kubernetes rules, and AutoStopping on Azure or GCP are not affected.

## Before you begin

* Access to the AWS account where the AutoStopping Lambda function is deployed
* The [Harness CE IAM role](../../references/best-practices/aws/aws-permissions.md) in that account must have the following Lambda permissions. If the role already includes `lambda:*`, no changes are needed:
  * `lambda:UpdateFunctionCode`
  * `lambda:UpdateFunctionConfiguration`
  * `lambda:GetFunctionConfiguration`
* Access to your Harness account with permission to create [service accounts](../../resources/access-control/ccm-roles-and-permissions.md)

## Step 1 - Identify your ALB access points

1. In Harness, navigate to **CACM** > **Cost Optimization** > **AutoStopping Rules**.
2. Choose **Load Balancers**.
3.  Review the list of access points:

    * **No badge** under the name: the access point routes traffic through an ALB and Lambda. This needs to be migrated.
    * **Public IP** or **External IP** badge: the access point uses a proxy. No action needed.

    ![Load Balancers list showing ALB-based and proxy-based access points](../../.gitbook/assets/as-mig-1.png)
4.  For each ALB access point, copy the **ID** shown under the name. You will pass these IDs to the migration API in the next step.

    ![Access point IDs to copy for migration](../../.gitbook/assets/as-mig-2.png)

## Step 2 - Create a service account token

The migration API requires a Harness service account token. The token is also stored in the Lambda function as the `API_TOKEN` environment variable, so it must not expire.

### Interactive guide

{% @arcade/embed flowId="btLF7Cxc62uSF8Djbomm" url="https://app.arcade.software/share/btLF7Cxc62uSF8Djbomm" %}

### Step by step

1. In Harness, navigate to **Account Settings** > **Access Control** > [**Service Accounts**](../../resources/access-control/ccm-roles-and-permissions.md).
2. Select **+ New Service Account**, give it a name (for example, `autostopping-migration`), and select **Save**.
3. Open the newly created service account and select **Manage Role Bindings**.
4. Select **+ Add**, open the **Role** dropdown, and choose [**CCM Viewer**](../../resources/access-control/ccm-roles-and-permissions.md).
5. Select **Apply Selected**.
6. Set **Resource Group** to **All Resources Including Child Scopes** and select **Save**.
7. On the service account page, select **+ API Key**, give it a name, and select **Save**.
8. Under the API key, select **Token**, give it a name, set **Expiration** to **No Expiration**, and select **Generate Token**.
9. Copy and store the token — you cannot retrieve it again after closing the dialog.

{% hint style="warning" %}
Do not set a short expiry on this token. The Lambda function uses it for every warm-up request after migration. If the token expires, warm-up will stop working for all AutoStopping rules behind the migrated load balancers.
{% endhint %}

## Step 3 - Find your Harness account ID

The migration API requires your Harness account ID.

1. In Harness, navigate to **Account Settings**.
2.  Select **Account Details** under the **General** tab.

    <figure><img src="../../.gitbook/assets/as-mig-3.png" alt=""><figcaption></figcaption></figure>
3.  Copy the **Account Id** value.

    ![Account Id in Account Details](../../.gitbook/assets/as-mig-4.png)

## Step 4 - Migrate your load balancers

Pass the access point IDs you collected in Step 1 to the migration API. Replace `{account_id}` with the account ID from Step 3 and `{service_account_token}` with the token from Step 2.

{% hint style="success" %}
Start with a single load balancer first. Verify it works in Step 5 before migrating the rest.
{% endhint %}

### Migrate a single load balancer

```bash
curl --request POST \
  --url 'https://app.harness.io/gateway/lw/api/accounts/{account_id}/autostopping/loadbalancers/upgrade?routingId={account_id}&accountIdentifier={account_id}' \
  --header 'x-api-key: {service_account_token}' \
  --header 'Content-Type: application/json' \
  --data '{
    "access_point_ids": ["{access_point_id}"],
    "access_token": "{service_account_token}"
  }'
```

### Migrate multiple load balancers in one call

```bash
curl --request POST \
  --url 'https://app.harness.io/gateway/lw/api/accounts/{account_id}/autostopping/loadbalancers/upgrade?routingId={account_id}&accountIdentifier={account_id}' \
  --header 'x-api-key: {service_account_token}' \
  --header 'Content-Type: application/json' \
  --data '{
    "access_point_ids": ["ap-abc123", "ap-def456", "ap-ghi789"],
    "access_token": "{service_account_token}"
  }'
```

A successful response looks like:

```json
{
  "success": true,
  "enqueued_count": 3
}
```

The `enqueued_count` field reflects the number of load balancers queued. The migration runs as a background job — allow up to **6 minutes per load balancer** before checking the result.

## Step 5 - Verify the migration

After the migration job completes:

1. In the AWS console, navigate to **Lambda** > **Functions** and open the function whose name matches your access point ID (for example, `ap-abc123`).
2. Navigate to the **Configuration** tab and select **Environment variables**.
3.  Confirm the `API_TOKEN` variable is present and contains your service account token.

    ![Lambda Environment variables panel showing API\_TOKEN set](../../.gitbook/assets/as-mig-5.png)
4. Trigger a warm-up by accessing a resource that is covered by an AutoStopping rule behind this load balancer.
5. In CloudWatch Logs, open the log group for the Lambda and confirm the warm-up request completed without errors.

## Rollback

If warm-up stops working after migration, you can revert a load balancer to the previous Lambda code:

```bash
curl --request POST \
  --url 'https://app.harness.io/gateway/lw/api/accounts/{account_id}/autostopping/loadbalancers/upgrade?routingId={account_id}&accountIdentifier={account_id}' \
  --header 'x-api-key: {service_account_token}' \
  --header 'Content-Type: application/json' \
  --data '{
    "access_point_ids": ["{access_point_id}"],
    "access_token": "{service_account_token}",
    "revert": true
  }'
```

## Troubleshooting

### Warm-up fails after migration

1. Open the Lambda function in the AWS console.
2. Check **Configuration** > **Environment variables** and confirm `API_TOKEN` is set.
3. Confirm the service account token has not been deleted or rotated. Navigate to **Account Settings** > **Access Control** > **Service Accounts**, open the service account, and verify the token is still active.
4. If the token is valid but warm-up still fails, revert the load balancer using the rollback command above and contact [Harness Support](https://support.harness.io).

### Migration API returns an error

| Error                              | Likely cause                                                          |
| ---------------------------------- | --------------------------------------------------------------------- |
| `Access point IDs cannot be empty` | The `access_point_ids` array in the request body is empty or missing. |
| `401 Unauthorized`                 | The `x-api-key` header is missing or the token is invalid.            |
| `403 Forbidden`                    | The service account does not have the CCM Viewer role.                |

### Lambda is not updating after the API call

The migration runs as a background job in the Harness backend. If the `API_TOKEN` environment variable is not present after 10 minutes:

* Confirm the Harness CE IAM role has `lambda:UpdateFunctionCode`, `lambda:UpdateFunctionConfiguration`, and `lambda:GetFunctionConfiguration`. Navigate to [AWS Permissions](../../references/best-practices/aws/aws-permissions.md) to review the required policy.
* Check AWS CloudTrail for `UpdateFunctionConfiguration` events on the Lambda function. A failed event will show the error reason.
