---
description: View your AI spending budget, track usage, and request a limit increase from your Harness profile.
---

# Manage your AI budget

---

## What you will learn from this topic

* How to find your AI budget in your Harness profile
* What each field on your budget card means
* How to read your spend history and past requests
* How to request a limit increase and track its status

---

## Before you begin

* You must be part of a Harness user group that has an AI budget assigned to it.

---

## View your AI budget

1. Click your name or avatar in the bottom-left corner of your Harness account.
2. Select **Profile**.
3. Select the **AI Budgets** tab.

![AI Budgets tab showing budget allocation, spend, and enforcements](../../.gitbook/assets/user-budgets-my-ai-budgets.png)

Your budgets appear under **My AI budgets**. Each budget card lets you view your allocation, current spend, and when the period resets.

| Field | Description |
|---|---|
| Budget allocated | Your spending limit for the period and which AI providers it covers (for example, **All AI spend**) |
| Resets on | When the current period ends and your spend resets to zero |
| Spend so far | How much you have spent, what is left, and which day of the period you are on |
| Progress bar | A visual indicator of your spend against your limit |
| Enforcements | The action Harness takes when you approach or hit your limit (for example, **Notify at 80%**, **Block all access at 100%**) |

### Spend history

The **Spend history** tab displays a bar chart of your daily AI spend. Use the filters to narrow the data:

| Filter | Options |
|---|---|
| Group by | Providers, Models, Accounts, Model family, Token type |
| Provider | AWS, Azure, Claude Enterprise, Cursor, Devin, GCP |
| Model | Filter by specific model |
| Time Range | Select the date range |
| Breakdown | Daily, Weekly, or Monthly |

### My requests

The **My requests** tab displays all your past limit increase requests, including who approved or rejected them and any notes they left.

---

## Request a limit increase

Your administrator has set approval tiers, so Harness automatically routes your request to the approver based on the amount you ask for.

1. Click **Request limit increase** on the budget card.

   ![Request limit increase button on the budget card](../../.gitbook/assets/user-budgets-request-limit-button.png)

   The panel displays your current limit and who approves requests at each amount, so you know what to expect before you submit.

   ![Request panel showing approval tiers and the request form](../../.gitbook/assets/user-budgets-request-limit-increase.png)

2. In the **Requested new budget** field, enter the amount you need. It must be higher than your current limit.
3. Enter a **Reason** for the request, so your approver has context.
4. Click **Submit request**.

Harness sends the request to the right approver based on the amount. Go to [Review limit increase requests](manage-user-budgets.md#review-limit-increase-requests) to view how approvers review and act on requests. Your budget card updates so you can track the request. It displays the amount you asked for, when you submitted it, and who is reviewing it.

![Budget card showing a pending request with the upgrade amount, approver, and actions](../../.gitbook/assets/user-budgets-pending-request-card.png)

You do not need to do anything else. Harness notifies your approver. If you need to follow up or withdraw the request:

* **Share with approver**: Send a direct link to the approver to speed up the review.
* **Revoke my request**: Cancel the request if you no longer need the increase. Harness confirms with a "Request revoked" notification.

---

## Next steps

* Go to [Create and manage user budgets](manage-user-budgets.md) if you are an administrator who needs to set up budgets, configure approval tiers, or review and approve requests from your team.

{% @harness-feedback/feedback %}
