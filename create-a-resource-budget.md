---
description: "Create one resource budget on a cost view: set the scope, period, amount, and optional alerts."
---

# Create a resource budget

Create a resource budget to track spend for one cost view. Set the scope, the budget period and amount, and the alert thresholds.

Go to [Budgets](../../concepts/budgets.md) to compare a resource budget with a user budget for AI spend.

---

## What you will learn from this topic

- **Scope:** How to associate a budget with a cost view and a folder.
- **Period and amount:** How to set the budget period, start date, amount, and an optional growth rate.
- **Alerts:** How to configure alert thresholds before you save the budget. Go to [Manage resource budgets](manage-resource-budgets.md#change-alerts) to update alerts after the budget is created.

---

## Before you begin

- **A cost view:** The budget tracks cost for one cost view. Go to [Create a cost view](../../cost-visibility/cost-views/create-a-cost-view.md) to create a cost view.
- **Budgets: Edit:** This permission is required to create a budget. An administrator grants the permission through a role. Go to [Budgets](../../concepts/budgets.md#access-and-auditing) to review budget permissions.
- **Folder access:** Access to the destination folder is required. Without that access, you cannot view the budget or its cost view. Go to [Organize budgets in folders](manage-resource-budgets.md#organize-budgets-in-folders) to manage folders.

---

## Create the budget

The **New budget** panel contains three steps: **Define scope**, **Specify budget period and amount**, and **Setup alerts**. Complete the steps in order.

### Define scope

In **Define scope**, select the cost view, enter a name, and select the folder that stores the budget.

![Define scope step of New budget, with Create a single budget selected, the AWS cost view, a budget name, and a folder](../../.gitbook/assets/budgets-new-budget-scope.png)

To define the scope, do the following:

1. In the left navigation, select **Cloud & AI Costs**, and then select **Budgets**.
2. On the **Resource budgets** tab, click **New budget**, and then select **Resource budget**. The **New budget** panel opens.
3. Select **Create a single budget**. A single budget tracks spend for one team, account, or set of services.
4. In **Select a cost view**, select the cost view that the budget tracks. You can create more than one budget for the same cost view.
5. In **Budget name**, enter a name for the budget.
6. In **Save budget to folder**, select the folder for the budget. To create a folder from this panel, click **New folder**. **Budget access** states that access to the selected folder is required to view the budget and its cost view.
7. Click **Next**.

### Specify budget period and amount

In **Specify budget period and amount**, set the period, the start date, and the budget amount.

![Specify budget period and amount, with a monthly period, a start date, a budget amount, and the add growth rate option](../../.gitbook/assets/budgets-new-budget-period.png)

To set the period and amount, do the following:

1. In **Budget period**, select **Daily**, **Weekly**, **Monthly**, **Quarterly**, or **Yearly**. For a yearly budget, select **Break the budget down by month** to set a separate amount for each month. **Copy to all** applies one month's amount to the remaining months. Under **Advanced configuration**, **Override forecasted spend for each month** replaces the forecast for a month with a value that you enter.
2. In **Start date**, select the first day of the budget period.
3. Enter the budget amount. The amount field label matches the selected period. For a monthly budget, the label is **Monthly budget amount ($)**. The amount must be greater than 0. When the amount is not greater than 0, Harness displays "Budget amount must be greater than 0". Below the amount, Harness displays the previous period's spend for the selected cost view.
4. To increase the amount in subsequent periods, select **Add growth rate to budget amount**, and then enter a value in **Growth rate (%)**. The field is required when the checkbox is selected. When the field is empty, Harness displays "Enter a growth rate".
5. Click **Next**.

To return to **Define scope**, click **Back**.

### Setup alerts

In **Setup alerts**, configure the thresholds that send a notification. Alerts are optional. Go to [Change alerts](manage-resource-budgets.md#change-alerts) to add or update alerts after the budget is created.

![Setup alerts step, with one alert for when actual spend exceeds a percentage of the monthly budget, sent by email](../../.gitbook/assets/budgets-new-budget-alerts.png)

To configure alerts, do the following:

1. Review the summary. The summary displays the budget amount and the renewal period.
2. Click **Add alert**.
3. Select **Actual spend** or **Forecasted spend**. An actual spend alert triggers when spend in the current period reaches the percentage. A forecasted spend alert triggers when the forecast for the end of the period reaches the percentage.
4. Enter the percentage of the budget amount. The label includes the selected period. For a monthly budget, the label is "percent of Monthly Budget".
5. Under **Send alerts to**, in **Channel**, select **Email** or **Slack**.
6. In **Recipients**, enter the recipients. When **Channel** is **Email**, enter the email addresses. The field displays **Enter e-mail IDs**.
7. To add another alert, click **Add alert**.
8. To remove an alert, click the remove icon on its row.
9. To include additional text in the notification, click **Add a custom message to the alert (optional)**.
10. Click **Save**.

The budget is listed on the **Resource budgets** tab.

To close the panel without creating the budget, click **Cancel**. To return to **Specify budget period and amount**, click **Back**.

---

## Next steps

- Go to [Manage resource budgets](manage-resource-budgets.md) to find the budget, move it to a folder, update its alerts, and review budget history.

{% @harness-feedback/feedback %}
