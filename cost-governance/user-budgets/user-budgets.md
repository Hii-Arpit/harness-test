---
description: Set per-user spend limits across AI tools, with enforcement, approvals, and self-service increase requests.
---

# Overview

{% hint style="success" %}
This feature is generally available (GA). Contact your Harness account team to set it up.
{% endhint %}

{% if "HAS_FEATURE_FLAG" === true %}
```
{
    "featureFlags": [{
        "key": "CCM_USER_BUDGETS",
        "name": "AI User Budgets",
        "status": "GA",
        "description": "Enables the User budgets for AI spend tab on Budgets and the AI Budgets view in user profiles. Admins can create per-user AI spending budgets with configurable approval workflows and enforcement actions."
    }]
}
```
{% endif %}

User Budgets let you define a spending cap for each user on AI tools such as Cursor, Claude, GitHub Copilot, and Amazon Bedrock. When a user approaches or exceeds their limit, Harness notifies them and, optionally, blocks their access. Users who need more can request a limit increase directly from their profile without filing a support ticket.

{% hint style="info" %}
User Budgets are separate from [resource budgets](../budgets/create-a-resource-budget.md), which track cloud infrastructure spend against cost views.
{% endhint %}

---

## What you will learn from this topic

* How the User Budgets list is organized and what each column displays
* How to search, sort, and filter budgets
* How to use folders to organize budgets and control access
* How to create, move, and delete budgets from the list view

---

## Before you begin

* Make sure budget enforcement is enabled on the connector. Select **AI Governance** on your AWS connector for Amazon Bedrock, or **Enforce AI user budgets** on the provider's own connector (Cursor, GitHub Copilot, and Claude). Consult your administrator if you do not have access. Go to [Create and manage user budgets](manage-user-budgets.md#enable-ai-governance-on-your-connector) to enable it on a connector.

---

## View user budgets

Go to **Cloud & AI Costs**, select **Budgets**, and then select **User budgets for AI spend** to view all user budgets configured in your account.

![User Budgets list view](../../.gitbook/assets/user-budgets-list.png)

Each row displays:

| Column | Description |
|---|---|
| Name | Budget name, folder, and the number of users in the budget |
| Budget | Per-user spend limit and billing period (for example, $500.00/wk) |
| Rolled up spend | Aggregate AI spend across all users in the budget for the current period, shown as a percentage and progress bar |
| User health | <ul><li><strong>Good</strong>: all users within limit</li><li><strong>At risk</strong>: one or more users nearing their threshold</li><li><strong>Over</strong>: one or more users have exceeded the limit</li></ul> |
| Notifications and enforcements | Active alert and block rules configured for the budget |
| Upgrade requests | Pending limit increase requests submitted by users in the budget |

{% hint style="success" %}
**Find and filter budgets**

Use the **Search budgets** bar to find a budget by name. Use the sort drop-down menu to order the list:

* **Name (A→Z / Z→A)**
* **Created (newest / oldest)**
* **Updated (newest / oldest)**
{% endhint %}

---

## Folders

Folders organize user budgets and control access. Users with access to a folder can view the budgets inside it. Use folders to scope visibility by team or org unit.

Select a folder in the **Folders** panel to filter the list. Select **All budgets** to return to the full list.

To create a folder, click **+** next to the **Folders** heading.

Enter a folder name and click **Save**.

To manage an existing folder, hover over it in the panel to reveal three actions:

| Action | Description |
|---|---|
| **Edit** | Rename the folder |
| **Delete** | Permanently remove the folder |
| **Pin** | Pin the folder to the top of the Folders panel for quick access |

You can assign a budget to a folder when you create it. To change the folder for an existing budget, select **Show more actions** on the budget row. Select **Move to folder** to change only the folder, or select **Edit** to update the folder along with other budget settings.

---

## Next steps

Go to [Create and manage user budgets](manage-user-budgets.md) to set up a budget, configure enforcement rules and an approval workflow, and manage limit increase requests.

{% @harness-feedback/feedback %}
