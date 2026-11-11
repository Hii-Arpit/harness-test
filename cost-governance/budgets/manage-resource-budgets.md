---
description: Find, organize, and update resource budgets, including folders, alerts, and budget history
---

# Manage resource budgets

Use the **Resource budgets** tab to find a budget, organize it in a folder, change its alerts, and track spend against the limit.

---

## What you will learn from this topic

- How to search, filter, and select columns on the list
- How to create a folder and move a budget into it
- How to change alerts for one budget or for several budgets
- How to read the budget detail page and budget history

---

## Before you begin

- You need a resource budget. Navigate to [Create a resource budget](create-a-resource-budget.md) to create one.
- You need permission to view budgets to open the list, permission to edit budgets to change a budget, and permission to delete budgets to remove one. Navigate to [Access and auditing](../../concepts/budgets.md#access-and-auditing) to review these permissions.
- You need access to the folder that contains the budget. You can then view the budget and its cost view.

---

## View the Resource budgets list

1. In the left navigation, select **Cloud & AI Costs**, then select **Budgets**.
2. Select the **Resource budgets** tab.

![Budgets page on the Resource budgets tab](../../.gitbook/assets/budgets-list.png)

### Select columns

Click **Columns**. By default, Harness shows seven columns. If you want, you can add more columns.

| Column | Shown by default |
| --- | --- |
| Budget name and period | Yes |
| Status | Yes |
| Current spend vs budget | Yes |
| Pace | Yes |
| Budget | Yes |
| Current spend (MTD) | Yes |
| Actions | Yes |
| End of month forecast | No |
| Alert thresholds | No |
| Created by | No |
| Last modified | No |

The **Budget** column shows the budget amount. **Pace** shows the pace for the period, including when spend is over the budget.

---

## Find a budget

Use any of the following to narrow the list:

* Use the **Search** field to find a budget by name.
* Click **All**, **Over**, **At risk**, or **OK** to filter by status.
* Click **Add filter**, then select a filter. The table below lists each filter.

### Filter the list

| Filter | What it does |
| --- | --- |
| Created by | Filter by the budget creator. |
| Last modified | Select **Last 7 Days**, **Last 30 Days**, **Last 90 Days**, or **Select custom range**. |
| Min budget amount ($) and Max budget amount ($) | Set the lowest and highest budget amounts to include. |
| Period | Select **Daily**, **Weekly**, **Monthly**, **Quarterly**, or **Yearly**. |
| Cost view | Show budgets for one cost view. |
| Scope type | Select **Perspective** or **Cost bucket**. |
| Cost category | Show budgets for a cost category. |

The filter appears as a chip. Click **Reset** to clear the filters. Click **Save** to save the current filters. Select **Saved filters** to apply a saved set.

---

## View a budget's details

### Open the detail page

Click a budget name to open its detail page.

### Detail page fields

The detail page shows these fields:

| Field | What it shows |
| --- | --- |
| Status | Over, At risk, or OK |
| Period | The budget period, such as Monthly |
| View on | The cost view that the budget tracks |
| Budget | The budget amount |
| Current spend | Spend for the period to date, the percentage of the budget used, and the days left in the period |
| Forecast | The forecast for the end of the period |
| Alerts configured | The alerts set on the budget |

### Edit or delete the budget

Click **Edit budget** to change the budget, including its alerts. Click **More**, then select **Delete** to remove it.

### Budget history

The **Budget history** chart shows past spend, period-to-date spend, spend over budget, forecast, and the period budget. Use the legend toggles to show or hide a series.

The history table lists each period with these columns:

| Column | Description |
| --- | --- |
| Actual cost | Spend incurred in the period |
| Budgeted cost | Budget amount for the period |
| Budget variance ($) | Dollar difference between actual and budgeted cost |
| Budget variance (%) | Percentage difference between actual and budgeted cost |

---

## Work with budgets

For one budget, click **Show more actions** on the budget row. For several budgets, select one or more budgets, then click **Bulk actions**.

| Action | What it does | One budget | Several budgets |
| --- | --- | --- | --- |
| Edit | Open the budget and change its settings, including its alerts. | Yes | No |
| Subscribe to alerts | Receive this budget's alerts. | Yes | No |
| Move to folder | Change the folder. Navigate to [Organize budgets in folders](#organize-budgets-in-folders) to use the folder panel. | Yes | No |
| Adjust budget amount | Change the amount of the selected budgets. Navigate to [Adjust the amount on several budgets](#adjust-the-amount-on-several-budgets) to review the options. | No | Yes |
| Manage alerts | Change thresholds or recipients for the selected budgets. Navigate to [Change alerts on several budgets](#change-alerts-on-several-budgets) to review the options. | No | Yes |
| Clone | Duplicate the selected budgets. | No | Yes |
| Delete | Remove the budget. | Yes | Yes |

{% hint style="danger" %}
**Delete is permanent**

You cannot restore a deleted budget.
{% endhint %}

### Adjust the amount on several budgets

1. Select one or more budgets.
2. Click **Bulk actions**, and then select **Adjust budget amount**.
3. Select **Increase by**, **Decrease by**, or **Set to**.
4. Select **$** or **%**.
5. Click **Update budgets** to apply the change, or click **Cancel** to leave the amounts unchanged. You cannot undo this change.

---

## Change alerts

Harness sends a resource budget alert when actual spend or forecasted spend exceeds a percentage of the budget. The notification goes to email or to Slack.

### Change alerts on one budget

1. Click **Show more actions** on the budget row, then select **Edit**.
2. On the detail page, click **Edit budget**.
3. Update the alert rows, then save.

Each row sets **Actual spend** or **Forecasted spend**, a percentage, a **Channel** of **Email** or **Slack**, and **Recipients**. Navigate to [Create a resource budget](create-a-resource-budget.md#setup-alerts) to review the fields on an alert row.

Select **Subscribe to alerts** on the row to receive the alerts configured for that budget.

### Change alerts on several budgets

1. Select one or more budgets.
2. Click **Bulk actions**, and then select **Manage alerts**.
3. Select one of the following:

   * **Delete all alerts**
   * **Add recipients to existing alerts**
   * **Remove recipients from existing alerts**
   * **Add new alerts**

A new alert threshold is a percentage of the amount of each selected budget.

---

## Organize budgets in folders

The **Folders** panel lists **All budgets** and your folders. Select a folder to show the budgets in it. Select **All budgets** to return to the full list.

Use **Search folders** to find a folder by name.

{% hint style="info" %}
**Folder access**

A user needs access to the folder that contains a budget to view the budget and its cost view. This includes the **Default** folder.
{% endhint %}

To create a folder:

1. Click **New folder**, and then enter a name.
2. Save the folder.

To move a budget:

1. Click **Show more actions** on the budget row, then select **Move to folder**.
2. Select the destination folder.

You can also select the folder in **Save budget to folder** when you create the budget. Navigate to [Create a resource budget](create-a-resource-budget.md#define-scope) to set the folder.

---

## Troubleshooting

<details>

<summary>A budget is missing from the list</summary>

Click **Reset** to clear the filters, then select **All budgets**. The budget is also hidden when you do not have access to its folder.

</details>

---

## Next steps

* Navigate to [Create a resource budget](create-a-resource-budget.md) to add another budget.
* Navigate to [User budgets](../user-budgets/README.md) to set a limit on AI spend for each user.
* Navigate to [Budgets](../../concepts/budgets.md) to compare a resource budget with a user budget.

{% @harness-feedback/feedback %}
