---
description: This topic describes how to create a new budget group.
---


# Budget Groups

{% hint style="info" %}
**Behind a Feature Flag**

Budget Groups are behind the `CCM_BUDGET_CASCADES` feature flag. Contact [Harness Support](mailto:support@harness.io) to have the flag enabled for your account.
{% endhint %}

Budget Groups aggregate related budgets and budget groups into one consolidated entity, giving you a unified view of spend. **A given Budget Group can contain&#x20;**_**either**_**&#x20;Budgets&#x20;**_**or**_**&#x20;Budget Groups.**

**With Budget Groups, you can:**

* **Set up one-click alerts** to notify stakeholders automatically when any child budget crosses its threshold.
* **Generate simplified roll-up reports** so you don’t have to track multiple individual budgets.
* **Apply cascading controls** to distribute the group’s amount down to child budgets equally or proportionally.
* **Mirror your org hierarchy** by nesting Budget Groups (for example → Company › Business Unit › Project) and rolling up costs at each level.

### Prerequisites <a href="#prerequisites" id="prerequisites"></a>

* [Budgets](create-a-budget.md): A Budget groups is a collection of Budgets or collection of Budget Groups.

### Create a budget group <a href="#create-a-budget-group" id="create-a-budget-group"></a>

{% tabs %}
{% tab title="Step-by-Step Guide" %}
1. Navigate to the **Cloud & AI Cost Management** module and click **Budgets**
2. Click **Create a new Budget Group**.

#### Step 1: Define group <a href="#step-1-define-group" id="step-1-define-group"></a>

<figure><img src="../../.gitbook/assets/step-one-bg.png" alt=""><figcaption><p>Click to view full-size image</p></figcaption></figure>

* Enter a **Budget group name**.
* Select the **Budgets** to create a group OR **Budget groups** to create a group. You **cannot combine** a budget and a budget group to create a Budget Group.
  * When selecting Budgets to group, ensure that they have the same **Start date**, **Budget Type**, and **Period**.
  * If you are creating a **group of Budget Groups**, the **cascading type** must also be similar.
  * After selecting the first budget or budget group, the remaining budgets or budget groups that have a different **Start date**, **Budget Type**, **Period** or **Cascading Type** are disabled. You would be able to form a new budget group of Budget(s) and Budget Group(s) having similar parameters as the first budget/budget group selected.
* Click **Continue**.

***

#### Step 2: Configure group <a href="#step-2-configure-group" id="step-2-configure-group"></a>

<figure><img src="../../.gitbook/assets/step-two-bg.png" alt=""><figcaption><p>Click to view full-size image</p></figcaption></figure>

* **Budget amount for the group**: The sum of the budgets or budget groups selected in the previous step is displayed by default. You can edit this value. If you make any changes to this amount, you will have to specify the **Cascading** strategy to split the balance.
* **Cascading**: If you wish to split the budget group amount across the budgets in the Budget Group, select one of the following cascading options:
  * **Equally**: Selecting this option splits the amount equally between the budgets in the group.
  * **Proportionally**: Specify the percentage split for each budget by selecting this option. For example, if you have a budget group with three budgets - Budget A, Budget B, and Budget C, you must specify the percentage of the total budget amount allotted to each of the three budgets. Ensure that the sum of the three percentages equals 100%.
* Click **Continue**.

***

#### Step 3: (Optional) set alerts <a href="#step-3-optional-set-alerts" id="step-3-optional-set-alerts"></a>

<figure><img src="../../.gitbook/assets/step-three-bg.png" alt=""><figcaption><p>Click to view full-size image</p></figcaption></figure>

* **Set Alerts for your budget group**: You can set alerts for your budget group to receive notifications when the actual or forecasted cost exceeds a specified percentage of the budget.
  * Select **Actual** or **Forecasted** from the dropdown list.
  * Enter the threshold percentage of the budget that will trigger an alert.
  * In **Send Alert To**, select one of the following options to receive budget notifications.
    * **Email**: Enter the email address (you can enter more than one email address or email groups).
    * **Slack Webhook URL**: Enter the webhook URL.
* Click **Save**.

***
{% endtab %}

{% tab title="Interactive Guide" %}
{% embed url="https://app.tango.us/app/embed/87012224-0977-4cee-b095-550c7e507d99?skipCover=false&defaultListView=false&skipBranding=false&makeViewOnly=true&hideAuthorAndDetails=true" %}
Add AWS Cloud Cost Connector in Harness
{% endembed %}
{% endtab %}
{% endtabs %}

### Budget group tracking <a href="#tracking-budget-group" id="tracking-budget-group"></a>

{% tabs %}
{% tab title="Budget Group Insights" %}
When you click on a specific budget group, you will see a detailed view containing:

#### Budget groups overview cards <a href="#budget-groups-overview-cards" id="budget-groups-overview-cards"></a>

<figure><img src="../../.gitbook/assets/bg-one.gif" alt=""><figcaption><p>Click to view full-size image</p></figcaption></figure>

Each budget displays the following key metrics:

* **Budget Period**: The time frame for your budget group (daily, weekly, monthly, quarterly, or yearly)
* **Spend Till Date**: The actual amount spent from the budget group start date to the current date
* **Budget Amount**: The total budget limit you have set for the specified period
* **Forecasted Cost**: Predicted spending based on current usage patterns and historical data
* **Alerts At**: The threshold percentages and notification settings you have configured
* **Budget Group Contents**: Details about the budgets or budget groups that are part of the budget group.

#### Budget history graph and table <a href="#budget-history-graph-and-table" id="budget-history-graph-and-table"></a>

<figure><img src="../../.gitbook/assets/bg-two.gif" alt=""><figcaption><p>Click to view full-size image</p></figcaption></figure>

Historical data for the budget group is displayed through an interactive graph as well as a table.

**Budget History Graph**

An interactive graph displaying:

* **Forecasted Cost Trend**: Projected spending over the budget period
* **Period-to-Date Cost**: Cumulative actual spending from the start of the latest budget period to current date where "Period" refers to the budget period. For example, if you have a monthly budget, the Month-to-Date Cost will show the cumulative cost from the start of the current month to the current date.
* **Actual Cost**: Real-time comparison of spending against your budget limit
* **Budget**: Visual indicators showing your alert thresholds

**Budget History Table**

A detailed breakdown corresponding to the Budget History Graph:

| Metric                  | Description                                               |
| ----------------------- | --------------------------------------------------------- |
| **Budget Period**       | The specific time frame (e.g., January 2024, Q1 2024)     |
| **Actual Cost**         | Real spending incurred during the period                  |
| **Budgeted Cost**       | The allocated budget amount for the period                |
| **Budget Variance ($)** | Dollar difference between actual and budgeted costs       |
| **Budget Variance (%)** | Percentage variance showing over/under budget performance |
{% endtab %}

{% tab title="Interactive Guide" %}
Follow this interactive guide to understand how to navigate and use the Budget Dashboard effectively:

{% embed url="https://app.tango.us/app/embed/fb211aa1-47d1-4b43-a62c-813129a4ab64?skipCover=false&defaultListView=false&skipBranding=false&makeViewOnly=true&hideAuthorAndDetails=true" %}
Budget Dashboard Navigation Guide
{% endembed %}
{% endtab %}
{% endtabs %}

#### Edit/Delete budget groups <a href="#editdelete-budget-groups" id="editdelete-budget-groups"></a>

**Edit a Budget Group**

{% hint style="info" %}
You cannot modify the budget amount and the **Cascading** settings if you have selected the option while creating the budget group, but the proportions can be modified if **Proportional Cascading** is selected.
{% endhint %}

To update a budget group:

1. In the Budgets homepage, select the budget group that you want to edit.
2. Click **Edit** from the vertical ellipsis (⋮) icon.
3. You can update the name of the budget group.
4. You can exclude existing budgets or budget groups and include new ones with the same parameters.

**Cascading Budget Recalculation**

Whenever there is a change in the budget values within a nested budget group, the adjustment will cascade upwards, causing a readjustment of the budget ratios all the way to the top. Consequently, the budget amounts in that pathway will increase by a consistent delta amount.

**Example of cascading monthly budget recalculation:**

Original monthly budgets:

* Jan, Feb, Mar - $80
* May - $90
* June, July - $100
* Aug, Sep, Oct, Nov - $120
* Dec - $110

The overall budget is USD1200. When this amount is increased to USD1800, the difference in the amount (USD600) is redistributed across months in the same ratio as their initial splits. For example, the budget amount for January will increase by USD40 (80/1200 \* 600 = 40).

**Delete a Budget Group**

{% hint style="danger" %}
**DELETING PERSPECTIVE**

When a perspective is deleted, all associated entities, such as reports, alerts, budgets, and budget groups are also deleted. If all children of a budget group are deleted as part of the perspective deletion, then the budget group itself will also be deleted.
{% endhint %}

To delete a budget group, in **All Budgets**, select the budget group that you want to delete. Click **Delete** from the vertical ellipsis (⋮) icon.

* The budget group is deleted from your account.
* The budgets and budget groups within the deleted budget group are moved to the common pool where they retain their autonomy and function independently.
* If the budget group was part of a cascade, all the upward budget group ratios will be readjusted and computed accordingly.

<figure><img src="../../.gitbook/assets/bg-delete.png" alt=""><figcaption><p>Click to view full-size image</p></figcaption></figure>

{% @harness-feedback/feedback %}
