---
description: Set per-developer spend limits across AI tools, with enforcement, approvals, and self-service increase requests.
---

{% if "HAS_FEATURE_FLAG" === true %}
```
{
    "featureFlags": [{
        "key": "CCM_USER_BUDGETS",
        "name": "AI User Budgets",
        "status": "LIMITED_GA",
        "description": "Enables the User Budgets section under Cost Governance and the AI Budgets view in user profiles. Admins can create per-user AI spending budgets with configurable approval workflows and enforcement actions."
    }]
}
```
{% endif %}

User Budgets let you define a spending cap for each developer on AI tools such as Cursor, Claude Enterprise, and Amazon Bedrock. When a developer approaches or exceeds their limit, Harness notifies them and, optionally, blocks their access. Developers who need more can request a limit increase directly from their profile — no support ticket required.

{% hint style="info" %}
User Budgets are separate from [resource budgets](../budgets/create-a-budget.md), which track cloud infrastructure spend against cost perspectives.
{% endhint %}

## Overview

Select **User Budgets** under **Cost Governance** in the left navigation to see all user budgets configured in your account.

<figure><img src="../../.gitbook/assets/user-budgets-list.png" alt="User Budgets list view"><figcaption><p>User Budgets list view</p></figcaption></figure>

Each row shows:

| Column | Description |
|---|---|
| Name | Budget name, folder, and the number of users in the budget |
| Budget | Per-user spend limit and billing period (for example, $500.00/wk) |
| Rolled up spend | Aggregate AI spend across all users in the budget for the current period, shown as a percentage and progress bar |
| User health | **Good** — all users within limit; **At risk** — one or more users nearing their threshold; **Over** — one or more users have exceeded the limit |
| Notifications and enforcements | Active alert and block rules configured for the budget |
| Upgrade requests | Pending limit-increase requests submitted by users in the budget |

{% hint style="success" %}
### Finding and filtering budgets

Use the **Search budgets** bar to find a budget by name. Use the sort dropdown to order the list:

* **Name (A→Z / Z→A)**
* **Created (newest / oldest)**
* **Updated (newest / oldest)**
{% endhint %}

## Folders

Folders organize user budgets and control access. People with access to a folder can see the budgets inside it — use folders to scope visibility by team or org unit.

Select a folder in the **Folders** panel to filter the list. Select **All budgets** to return to the full list.

To create a folder, click **+** next to the **Folders** heading, enter a name, and click **Save**. To edit or delete a folder, hover over it to reveal the pencil and trash icons.

You can assign a budget to a folder at creation time, or move it to a different folder later.

{% content-ref url="manage-user-budgets.md" %}
[manage-user-budgets.md](manage-user-budgets.md)
{% endcontent-ref %}

{% @harness-feedback/feedback %}
