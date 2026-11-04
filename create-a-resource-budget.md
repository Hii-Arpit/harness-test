---
description: "Create one resource budget on a cost view: set the scope, period, amount, and optional alerts."
---

# Create a resource budget

Create one resource budget to track cloud spend for a cost view. You choose the view, set a period and amount, and add alerts for actual or forecasted spend.

Go to [Budgets](../../concepts/budgets.md) to compare a resource budget with a user budget for AI spend.

---

## What you will learn from this topic

- **Scope:** How to attach a budget to one cost view and a folder.
- **Period and amount:** How to set the budget period, start date, amount, and an optional growth rate.
- **Alerts:** How to add the first alert before you save. Later changes are on [Manage resource budgets](manage-resource-budgets.md#change-alerts).

---

## Before you begin

- **A cost view:** The budget tracks cost in one cost view. Go to [Create a cost view](../../cost-visibility/cost-views/create-a-cost-view.md) to create one.
- **Budgets: Edit:** You need this permission to create a budget. An administrator assigns it with a role. Go to [Budgets](../../concepts/budgets.md#access-and-auditing) to review budget permissions.
- **Folder access:** You need access to the folder you save the budget in. People who cannot open that folder cannot view the budget or its cost view. Go to [Organize budgets in folders](manage-resource-budgets.md#organize-budgets-in-folders) to manage folders.

---

## Create the budget

In the left navigation, select **Cloud & AI Costs**, then select **Budgets**.

On the **Resource budgets** tab, click **New budget**, then select **Resource budget**. The **New budget** panel opens.

{% stepper %}
{% step %}
### Define scope

Select **Create a single budget**. This page covers that option. The card describes a budget that tracks spend for one team, account, or set of services.

![Define scope step of New budget, with Create a single budget selected, the AWS cost view, a budget name, and a folder](../../.gitbook/assets/budgets-new-budget-scope.png)

Set the target for this budget:

1. In **Select a cost view**, select the cost view to track. You can create more than one budget on the same cost view.
2. In **Budget name**, enter a name that identifies the budget in the list.
3. In **Save budget to folder**, select a folder. Check the folder before you continue. Click **New folder** to create a folder without leaving the panel.

**Budget access** names the folder you selected and reminds you that users need access to that folder to view the budget and its cost view.

Click **Next**.
{% endstep %}

{% step %}
### Specify budget period and amount

Set how long the budget lasts and the amount for that period.

![Specify budget period and amount, with a monthly period, a start date, a budget amount, and the add growth rate option](../../.gitbook/assets/budgets-new-budget-period.png)

1. In **Budget period**, select **Daily**, **Weekly**, **Monthly**, **Quarterly**, or **Yearly**.
2. In **Start date**, select the first day of the budget.
3. Enter the budget amount. The field label matches the period. For a monthly budget it is **Monthly budget amount ($)**. The amount must be greater than 0. Harness shows the message "Budget amount must be greater than 0" when it is not.

Under the amount, Harness shows last period's spend for this cost view. Use that figure as a reference, and enter the amount in the field above it.

**Add growth rate to budget amount** raises the amount by a percentage in later periods. Select the checkbox, then enter **Growth rate (%)**. The field is required when the checkbox is selected. Harness shows "Enter a growth rate" when it is empty.

**Yearly budgets.** Select **Yearly**, then select **Break the budget down by month** to set an amount for each month instead of one amount for the year. **Copy to all** copies one month's amount to the other months. Under **Advanced configuration**, **Override forecasted spend for each month** replaces the forecast for a month with a value you enter.

Click **Next**. Click **Back** to return to **Define scope**.
{% endstep %}

{% step %}
### Setup alerts

Add the alerts to send when you save. You can also save with no alerts and add them later. Go to [Change alerts](manage-resource-budgets.md#change-alerts) to update alerts on a budget that already exists.

![Setup alerts step, with one alert for when actual spend exceeds a percentage of the monthly budget, sent by email](../../.gitbook/assets/budgets-new-budget-alerts.png)

The step summary shows the budget amount and when the budget renews.

Click **Add alert**. Each alert is one row:

1. Select **Actual spend** or **Forecasted spend**. Actual spend alerts fire when spend so far crosses the percentage. Forecasted spend alerts fire when the forecast for the end of the period crosses the percentage.
2. Enter the percentage of the budget. The row names the period. For a monthly budget it reads "percent of Monthly Budget".
3. Under **Send alerts to**, select a **Channel** of **Email** or **Slack**.
4. In **Recipients**, enter who receives the alert. For **Email**, the field placeholder is **Enter e-mail IDs**.

Click **Add alert** again to add another threshold. Click the remove icon on a row to delete that alert.

Click **Add a custom message to the alert (optional)** to add text to the notification.

Click **Save** to create the budget. Click **Cancel** to close the panel without creating it. Click **Back** to return to the period and amount.
{% endstep %}
{% endstepper %}

---

## Next steps

The budget appears on the **Resource budgets** tab.

- Go to [Manage resource budgets](manage-resource-budgets.md) to find the budget, move it to a folder, change its alerts, and open budget history.

{% @harness-feedback/feedback %}
