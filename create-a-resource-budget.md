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

### Define scope

![Define scope step of New budget, with Create a single budget selected, the AWS cost view, a budget name, and a folder](../../.gitbook/assets/budgets-new-budget-scope.png)

1. In the left navigation, select **Cloud & AI Costs**, and then select **Budgets**.
2. On the **Resource budgets** tab, click **New budget**, and then select **Resource budget**.
3. Select **Create a single budget**.
4. In **Select a cost view**, select the cost view.
5. In **Budget name**, enter a name.
6. In **Save budget to folder**, select a folder, or click **New folder**.
7. Click **Next**.

**Budget access** states that access to the selected folder is required to view the budget and its cost view. You can create more than one budget for the same cost view.

### Specify budget period and amount

![Specify budget period and amount, with a monthly period, a start date, a budget amount, and the add growth rate option](../../.gitbook/assets/budgets-new-budget-period.png)

1. In **Budget period**, select **Daily**, **Weekly**, **Monthly**, **Quarterly**, or **Yearly**.
2. For **Yearly**, set the monthly breakdown.

   | Field | Action |
   | --- | --- |
   | **Break the budget down by month** | Enter an amount for each month. |
   | **Copy to all** | Apply one month's amount to the remaining months. |
   | **Override forecasted spend for each month** | Under **Advanced configuration**, enter a forecast for a month. |

3. In **Start date**, select the first day of the budget period.
4. In **Monthly budget amount ($)**, enter an amount greater than 0. The field name matches **Budget period**.

   | Display | When it appears |
   | --- | --- |
   | **Budget amount must be greater than 0** | The amount is not greater than 0. |
   | **Last period's spend for this cost view** | Under the amount field. |

5. Select **Add growth rate to budget amount**.
   1. In **Growth rate (%)**, enter the percentage. When the field is empty, Harness displays **Enter a growth rate**.
6. Click **Next**.

Click **Back** to return to **Define scope**.

### Setup alerts

![Setup alerts step, with one alert for when actual spend exceeds a percentage of the monthly budget, sent by email](../../.gitbook/assets/budgets-new-budget-alerts.png)

1. Click **Add alert**.
2. Complete the alert row. **percent of Monthly Budget** follows **Budget period**.

   | Field | Action |
   | --- | --- |
   | **When** | Select **Actual spend** or **Forecasted spend**. |
   | **Enter %** | Enter the percentage of the budget amount. |
   | **Channel** | Under **Send alerts to**, select **Email** or **Slack**. |
   | **Recipients** | Enter the recipients. For **Email**, the field displays **Enter e-mail IDs**. |

   | **When** | The alert triggers when |
   | --- | --- |
   | **Actual spend** | Spend in the current period reaches the percentage. |
   | **Forecasted spend** | The forecast for the end of the period reaches the percentage. |

3. Click **Add alert** to add another alert.
4. Click the remove icon on a row to delete that alert.
5. Click **Add a custom message to the alert (optional)** to add text to the notification.
6. Click **Save**.

The summary above the alerts shows the budget amount and the renewal period. The budget is listed on the **Resource budgets** tab.

Click **Cancel** to close the panel without creating the budget. Click **Back** to return to **Specify budget period and amount**. Go to [Change alerts](manage-resource-budgets.md#change-alerts) to add or update alerts after the budget is created.

---

## Next steps

- Go to [Manage resource budgets](manage-resource-budgets.md) to find the budget, move it to a folder, update its alerts, and review budget history.

{% @harness-feedback/feedback %}
