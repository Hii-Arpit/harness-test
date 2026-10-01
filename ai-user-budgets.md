---
description: Set per-user AI spending limits, configure approval workflows, and enforce budget caps across AI providers such as Cursor, Claude, and Amazon Bedrock.
---

{% if "HAS_FEATURE_FLAG" === true %}
```
{
    "featureFlags": [{
        "key": "CCM_USER_BUDGETS",
        "name": "AI User Budgets",
        "status": "LIMITED_GA",
        "description": "Enables the User Budgets tab under Budgets and the AI Budgets view in user profiles. Admins can create per-user AI spending budgets with approval workflows and enforcement actions."
    }]
}
```
{% endif %}

# AI user budgets

AI user budgets let you define how much each member of a user group can spend on AI tools such as Cursor, Claude, and Amazon Bedrock in a given week, month, or quarter. When a user approaches or exceeds their limit, Harness notifies them and, optionally, blocks their access automatically. Users who need more budget can request a limit increase directly from their profile, and the request routes to the approver you designate.

This is separate from [resource budgets](create-a-budget.md), which track cloud infrastructure spend against perspectives.

## Before you begin

* You must have the **Cloud & AI Cost Management** module enabled.
* The feature flag `CCM_USER_BUDGETS` must be enabled on your account.
* The users you want to budget must belong to a Harness user group.
* At least one AI connector with observed spend must exist; Harness blocks budget creation for a user group that has no AI spend data.

## Create a user budget

{% stepper %}
{% step %}
### Open User Budgets

In the left navigation, go to **Cloud & AI Cost Management** > **Budgets**.

Select the **User Budgets for AI Spend** tab, then click **Create a new Budget**.
{% endstep %}

{% step %}
### Step 1: Budget setup

Configure who this budget applies to and what it covers.

**Specify users**

Select **Harness user groups** and choose one or more existing user groups from the dropdown. To create a new group on the spot, click **+ Create new**.

**Applies to**

Choose which AI providers this budget governs:

| Option | What it covers |
|---|---|
| All AI spend | Every connected AI provider, including providers you add in the future |
| Cursor | Cursor only |
| Claude | Claude Enterprise only |
| Amazon Bedrock | Amazon Bedrock only |

To apply different limits per provider, create a separate budget for each provider.

**Budget name and folder**

Give the budget a descriptive name (for example, `CCM Team — Cursor`). Select a folder to keep budgets organized.

**Budget period and start date**

Choose **Weekly**, **Monthly**, or **Quarterly**. The budget amount resets at the start of each new period; unused allowance does not roll over.

Click **Next** to continue.
{% endstep %}

{% step %}
### Step 2: Enforcements and notifications

Configure the per-user limit, approval workflow, and what happens when a user hits their cap.

**Budget per user**

Enter the dollar amount each user in the group is allowed to spend per period. The scope shown in the top-right corner (for example, **All AI spend**) reflects what you selected in Step 1.

**Allow users to request a higher limit**

Toggle this on to let users request an increase from their profile. When enabled, configure the approval tiers:

| Field | Description |
|---|---|
| Requests up to | The dollar ceiling for this approval tier. A request routes to the lowest tier whose ceiling covers it. |
| Approved by | The person who approves requests at this tier. Select **Auto-approve** to approve automatically up to this ceiling without human review. |

Click **+ Add approval tier** to add more tiers. The highest tier's ceiling is the maximum any user can request in total. Approvers can approve at a different amount than requested and add a note.

**Advanced settings (optional)**

Expand **Advanced settings** to configure two per-budget guardrails:

* **Users can request an increase once spend reaches** — the percentage of their current budget they must consume before they can request more (default: 80%).
* **Maximum increase per request** — the largest single-request increase allowed (default: $125).

**Notifications and enforcement**

Click **+ Add percentage threshold** to add one or more rules. For each rule:

1. Enter the spend percentage that triggers the rule (for example, `80`).
2. Choose **Notify** to send an email to the affected user (and any additional recipients you add).
3. Choose **Block** to revoke the user's access to the covered AI providers once the threshold is crossed.

You can add multiple rules — for example, notify at 80% and block at 100%.

Click **Create** to save the budget.
{% endstep %}
{% endstepper %}

## Monitor a user budget

Click a budget name from the **User Budgets for AI Spend** list to open its detail page.

### Summary tiles

| Tile | Description |
|---|---|
| Budget per user | The per-user limit and provider scope |
| Total budget | Sum of all users' limits for the current period |
| Week-to-date spend | Aggregate spend so far; shows percentage of budget used and which day of the period it is |
| Pending requests | Number of outstanding limit-increase requests; click **Review all** to act on them |

The **Active Enforcements** bar shows the notification and block rules configured for this budget.

### Per-user table

The table lists every member of the budget's user group with the following columns:

| Column | Description |
|---|---|
| User | User email |
| Budget | Their current per-period limit (which may be higher than the default if a previous increase was approved) |
| Status | **OK** (within limit), **At risk** (approaching the notification threshold), or **Exceeded** (over the limit) |
| Current spend | Dollar amount spent and percentage of their limit used, shown as a progress bar |
| Enforcements | **Blocked** if an enforcement has been applied to this user |
| Requests | A pending request appears inline; click **Review** to approve or reject it directly from the table |

Use the **Enforcements** and **Requests** dropdown filters above the table to narrow the view.

## View and request a budget increase (user)

Users access their AI budget from their profile.

1. Click your avatar or name in the bottom-left corner.
2. Select **Profile**.
3. Select the **AI Budgets** tab.

The page shows every budget you belong to. For each budget you can see:

* **Budget allocated** — your current per-period limit and provider scope.
* **Spend so far** — how much you have spent in the current period, with a progress bar and the days remaining.
* **Enforcements** — any active notify or block rules on your account.
* **Spend history** — a bar chart of your daily AI spend broken down by model and provider.
* **My requests** — a history of your limit-increase requests with status, approver, and any note the approver added.

### Request a limit increase

1. On the **AI Budgets** tab, click **Request limit increase**.
2. In the **Requested new monthly budget** field, enter the total budget amount you need (it must be higher than your current limit).
3. Enter a **Reason** for the request.
4. Click **Submit request**.

The request routes to the appropriate approver tier based on the amount. If the amount falls within the auto-approve tier, the limit updates immediately.

To cancel a pending request, click **Revoke** next to the request in the **My requests** tab.

## Approve or reject a request (admin)

When a user submits a request, a notification is sent to the designated approver, who can also open the request via a direct share link. Approvers can:

* **Approve** — accept the request. Optionally enter a different dollar amount to approve (the UI shows the resulting transition, for example $125 → requested $250 → approved $200) and add a note visible to the user.
* **Reject** — decline the request.

Pending requests also appear inline in the per-user table on the budget detail page.

{% hint style="info" %}
Admins who are also listed as an approver for their own budget can approve their own requests.
{% endhint %}

## Edit or delete a user budget

On the budget detail page, click **Edit Budget** to reopen the two-step wizard and change any setting. Click the three-dot menu for additional options including **Delete**.

{% hint style="danger" %}
Deleting a user budget removes all enforcement rules immediately. Users who were blocked regain access.
{% endhint %}

{% @harness-feedback/feedback %}
