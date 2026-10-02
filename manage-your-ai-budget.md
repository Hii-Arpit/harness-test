---
description: View your AI spending budget, track usage, and request a limit increase from your Harness profile.
---

# Manage your AI budget

## Before you begin

* Your Harness account must have the `CCM_USER_BUDGETS` feature flag enabled.
* You must be part of a Harness user group that has an AI budget assigned to it.

## View your AI budget

You can see your AI budget allocation and spending history from your profile.

1. Click your avatar or name in the bottom-left corner.
2. Select **Profile**.
3. Select the **AI Budgets** tab.

The page shows every budget you belong to. For each budget you can see:

* **Budget allocated**: your current per-period limit and provider scope.
* **Spend so far**: how much you have spent in the current period, with a progress bar and the days remaining.
* **Enforcements**: any active notify or block rules on your account.
* **Spend history**: a bar chart of your daily AI spend broken down by model and provider.
* **My requests**: a history of your limit-increase requests with status, approver, and any note the approver added.

## Request a limit increase

The **Request limit increase** button is available once your spend reaches the threshold configured on the budget (default: 80% of your limit).

1. On the **AI Budgets** tab, click **Request limit increase**.
2. In the **Requested new budget** field, enter the total budget amount you need. It must be higher than your current limit, and no higher than the ceiling of the highest approver tier.
3. Enter a **Reason** for the request.
4. Click **Submit request**.

The request goes to the designated approver tier based on the amount requested. If the amount falls within an auto-approve tier, the limit updates immediately.

To cancel a pending request, click **Revoke** next to it in the **My requests** list.

{% @harness-feedback/feedback %}
