---
description: Find, filter, edit, and track resource budgets on the Budgets list and detail page
---

# Manage resource budgets

Use the **Resource budgets** tab to find a budget, change its settings, and track spend against the limit.

---

## What you will learn from this topic

* How to search, filter, and choose columns on the resource budget list
* How to edit, move, clone, or delete a budget
* How to read the budget detail page and budget history

---

## Before you begin

* At least one resource budget. Go to [Create a resource budget](create-a-resource-budget.md) to create one.
* Access to the folder that contains the budget. Go to [Folders](folders.md) to organize budgets.

---

## Open the Resource budgets tab

In the left navigation, select **Cloud & AI Costs**, then select **Budgets**.

Select the **Resource budgets** tab.

![Budgets page on the Resource budgets tab](../../.gitbook/assets/budgets-list.png)

---

## Find a budget

Use the **Search** field to find a budget by name.

### Filter the list

Click **All**, **Over**, **At risk**, or **OK** to filter by status.

Click **Add filter**, then select a filter:

* **Created by:** Filter by the budget creator.
* **Last modified:** Select **Last 7 Days**, **Last 30 Days**, **Last 90 Days**, or **Select custom range**.
* **Min budget amount ($)** and **Max budget amount ($):** Set the lowest and highest budget amounts to include.
* **Period:** Select **Daily**, **Weekly**, **Monthly**, **Quarterly**, or **Yearly**.
* **Cost view:** Show budgets for one cost view.
* **Scope type:** Select **Perspective** or **Cost bucket**.
* **Cost category:** Show budgets for a cost category.

The filter appears as a chip. Click **Reset** to clear the filters. Click **Save** to save the current filters. Select **Saved filters** to apply a saved set.

### Choose columns

Click **Columns**. The control shows **7/11** when seven of the eleven columns are visible.

These columns are visible by default:

* **Budget name and period**
* **Status**
* **Current spend vs budget**
* **Pace**
* **Budget**
* **Current spend (MTD)**
* **Actions**

Select any of these to add them:

* **End of month forecast**
* **Alert thresholds**
* **Created by**
* **Last modified**

The **Budget** column shows the budget amount. **Specified amount** or **Previous period spend** can appear under the amount. **Pace** shows the pace for the period, including when spend is over the budget.

---

## Work with one budget

Click **Show more actions** on the budget row, then select one of the following:

* **Edit:** Open the budget for changes.
* **Subscribe to alerts:** Get notified for this budget. Go to [Alerts and notifications](alerts-and-notifications.md) to configure alert rules.
* **Move to folder:** Change the folder. Go to [Folders](folders.md) to manage folders.
* **Delete:** Remove the budget.

{% hint style="danger" %}
**Delete is permanent**

Once a budget is deleted, it cannot be restored.
{% endhint %}

---

## Work with several budgets

Select one or more budgets, then click **Bulk actions**.

* **Adjust budget amount:** Select **Increase by**, **Decrease by**, or **Set to**. Select **$** or **%**. The dialog says "Budget amounts will be updated. This can't be undone." Click **Update budgets** to apply the change, or click **Cancel** to leave the amounts unchanged.
* **Manage alerts:** Change alert thresholds or recipients for the selected budgets. Go to [Alerts and notifications](alerts-and-notifications.md) for alert options.
* **Clone:** Duplicate the selected budgets.
* **Delete:** Remove the selected budgets.

---

## Open a budget detail page

Click a budget name to open its detail page.

The detail page shows:

* **Status**
* **Period**
* **View on**
* **Budget**
* **Current spend** for the period to date, with the percentage of the budget used and the days left in the period
* **Forecast** for the end of the period
* **Alerts configured**

Click **Edit budget** to change the budget. Click **More**, then select **Delete** to remove it.

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

## Next steps

* Go to [Create a resource budget](create-a-resource-budget.md) to add another budget.
* Go to [Folders](folders.md) to organize budgets.
* Go to [Alerts and notifications](alerts-and-notifications.md) to change alert rules.

{% @harness-feedback/feedback %}
