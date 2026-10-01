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

## Overview

User Budgets let you define a spending cap for each developer on AI tools such as Cursor, Claude Enterprise, and Amazon Bedrock. When a developer approaches or exceeds their limit, Harness notifies them and, optionally, blocks their access. Developers who need more can request a limit increase directly from their profile without filing a support ticket.

{% hint style="info" %}
User Budgets are separate from [resource budgets](../budgets/create-a-budget.md), which track cloud infrastructure spend against cost perspectives.
{% endhint %}

## What you will learn from this topic

* How the User Budgets list is organized and what each column shows
* How to search, sort, and filter budgets
* How to use folders to organize budgets and control access
* How to create, move, and delete budgets from the list view

## Before you begin

* You need the **Cost Governance** permission or the dedicated User Budgets RBAC role.

Select **User Budgets** under **Cost Governance** in the left navigation to see all user budgets configured in your account.

<figure><img src="../../.gitbook/assets/user-budgets-list.png" alt="User Budgets list view"><figcaption><p>User Budgets list view</p></figcaption></figure>

Each row shows:

| Column | Description |
|---|---|
| Name | Budget name, folder, and the number of users in the budget |
| Budget | Per-user spend limit and billing period (for example, $500.00/wk) |
| Rolled up spend | Aggregate AI spend across all users in the budget for the current period, shown as a percentage and progress bar |
| User health | <ul><li><strong>Good</strong>: all users within limit</li><li><strong>At risk</strong>: one or more users nearing their threshold</li><li><strong>Over</strong>: one or more users have exceeded the limit</li></ul> |
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

Folders organize user budgets and control access. People with access to a folder can see the budgets inside it. Use folders to scope visibility by team or org unit.

Select a folder in the **Folders** panel to filter the list. Select **All budgets** to return to the full list.

To create a folder, click **+** next to the **Folders** heading.

<figure><img src="../../.gitbook/assets/user-budgets-folders-create.png" alt="Click + to create a new folder"><figcaption><p>Click + next to Folders to create a new folder</p></figcaption></figure>

Enter a folder name and click **Save**.

<figure><img src="../../.gitbook/assets/user-budgets-folder-modal.png" alt="Create new folder modal"><figcaption><p>Create new folder dialog</p></figcaption></figure>

To manage an existing folder, hover over it in the panel to reveal three actions:

| Action | Description |
|---|---|
| Edit (pencil) | Rename the folder |
| Delete (trash) | Permanently remove the folder |
| Pin (pin icon) | Pin the folder to the top of the Folders panel for quick access |

You can assign a budget to a folder when you create it. To change the folder for an existing budget, use the three-dot menu on the budget row. Select **Move to folder** to change only the folder, or select **Edit** to update the folder along with other budget settings.

<figure><img src="../../.gitbook/assets/user-budgets-row-menu.png" alt="Budget row three-dot menu"><figcaption><p>Each budget row has a three-dot menu with Edit, Move to folder, and Delete options</p></figcaption></figure>

## Next steps

Go to [Create and manage user budgets](manage-user-budgets.md) to set up a budget, configure enforcement rules and an approval workflow, and manage limit increase requests.

{% @harness-feedback/feedback %}
