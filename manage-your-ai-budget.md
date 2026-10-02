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

<figure><img src="../../.gitbook/assets/user-budgets-my-ai-budgets.png" alt="AI Budgets tab in your Harness profile"><figcaption><p>AI Budgets tab showing budget allocation, spend, and enforcements</p></figcaption></figure>

Your budgets appear under **My AI budgets**. Each budget card shows how much you have been allocated, how much you have spent, and when the period resets.

| Field | Description |
|---|---|
| Budget allocated | Your spending limit for the period and which AI providers it covers (for example, **All AI spend**) |
| Resets on | When the current period ends and your spend resets to zero |
| Spend so far | How much you have spent, what is left, and which day of the period you are on |
| Progress bar | A visual indicator of your spend against your limit |
| Enforcements | What happens when you approach or hit your limit (for example, **Notify at 80%**, **Block all access at 100%**) |

### Spend history

The **Spend history** tab shows a bar chart of your daily AI spend. Use the filters to slice the data:

| Filter | Options |
|---|---|
| Group by | Providers, Models, Accounts, Model family, Token type |
| Provider | AWS, Azure, Claude Enterprise, Cursor, Devin, GCP |
| Model | Filter by specific model |
| Time Range | Select the date range |
| Breakdown | Daily, Weekly, or Monthly |

### My requests

The **My requests** tab shows all your past limit increase requests — including who approved or rejected them and any notes they left.

## Request a limit increase

If you need more budget, you can submit a request directly from your profile. Your admin has set approval tiers, so Harness automatically routes your request to the right person based on the amount you ask for.

1. Click **Request limit increase** on the budget card.

   <figure><img src="../../.gitbook/assets/user-budgets-request-limit-button.png" alt="Request limit increase button on the budget card"><figcaption><p>Request limit increase button on the budget card</p></figcaption></figure>

   The panel shows your current limit and who approves requests at each amount, so you know what to expect before you submit.

   <figure><img src="../../.gitbook/assets/user-budgets-request-limit-increase.png" alt="Request to increase AI budget panel"><figcaption><p>Request panel showing approval tiers and the request form</p></figcaption></figure>

2. In the **Requested new weekly budget** field, enter the amount you need. It must be higher than your current limit.
3. Enter a **Reason** for the request so your approver has context.
4. Click **Submit request**.

Harness sends the request to the right approver based on the amount. Your budget card updates so you can track the request: you can see the amount you asked for, when you submitted it, and who is currently reviewing it.

<figure><img src="../../.gitbook/assets/user-budgets-pending-request-card.png" alt="Budget card showing a pending limit increase request"><figcaption><p>Budget card showing a pending request with the upgrade amount, approver, and actions</p></figcaption></figure>

You do not need to do anything else. Your approver will be notified. If you need to follow up or withdraw the request:

* **Share with approver**: Send a direct link to the approver to speed up the review.
* **Revoke my request**: Cancel the request if you no longer need the increase. Harness confirms with a "Request revoked" notification.

{% @harness-feedback/feedback %}
