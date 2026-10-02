---
description: View your AI spending budget, track usage, and request a limit increase from your Harness profile.
---

# Manage your AI budget

## Before you begin

* Your Harness account must have the `CCM_USER_BUDGETS` feature flag enabled.
* You must be part of a Harness user group that has an AI budget assigned to it.

## View your AI budget

1. Click your name or avatar in the bottom-left corner of Harness.
2. Select **Profile**.
3. Select the **AI Budgets** tab.



The **My AI budgets** page lists every budget you belong to. Each budget card shows:

| Field | Description |
|---|---|
| Budget allocated | Your current per-period limit and provider scope (for example, **All AI spend**) |
| Resets on | The date the current period ends and your spend counter resets |
| Spend so far | How much you have spent in the current period, the amount remaining, and which day of the period it is |
| Progress bar | Your spend as a percentage of your total limit |
| Enforcements | Active notify and block rules on your account (for example, **Notify at 80%**, **Block all access at 100%**) |

### Spend history

The **Spend history** tab shows a bar chart of your AI spend. Use the filters to change the view:

| Filter | Options |
|---|---|
| Group by | Providers, Models, Accounts, Model family, Token type |
| Provider | Filter by provider: AWS, Azure, Claude Enterprise, Cursor, Devin, GCP |
| Model | Filter by model |
| Time Range | Select the date range |
| Breakdown | Daily breakdown, Weekly breakdown, Monthly breakdown |

### My requests

The **My requests** tab shows a history of your limit-increase requests with status, approver, and any note the approver added.

## Request a limit increase

1. Click **Request limit increase** on the budget card.

    <figure><img src="../../.gitbook/assets/user-budgets-my-ai-budgets.png" alt="AI Budgets tab in your Harness profile"><figcaption><p>AI Budgets tab showing budget allocation, spend, and enforcements</p></figcaption></figure>

   The panel shows your current limit and the approval tiers configured for this budget, so you can see who will approve your request based on the amount you enter.

   <figure><img src="../../.gitbook/assets/user-budgets-request-limit-increase.png" alt="Request to increase AI budget panel"><figcaption><p>Request to increase AI budget panel showing approval tiers and the request form</p></figcaption></figure>

2. In the **Requested new weekly budget** field, enter the total budget amount you need. It must be higher than your current limit and no higher than the ceiling of the highest approver tier.
3. Enter a **Reason** for the request.
4. Click **Submit request**.

The request goes to the designated approver tier based on the amount requested. If the amount falls within an auto-approve tier, the limit updates immediately.

To cancel a pending request, click **Revoke** next to it in the **My requests** tab. Harness confirms with a "Request revoked" notification.

{% @harness-feedback/feedback %}
