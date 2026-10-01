---
description: Create per-developer AI spending budgets, configure approval tiers and enforcement rules, monitor usage across your team, and handle limit increase requests.
---

# Create and manage user budgets

## Before you begin

* Harness Cloud & AI Cost Management must be enabled on your account.
* The feature flag `CCM_USER_BUDGETS` must be enabled. Contact [Harness Support](https://support.harness.io) if you do not see User Budgets in the navigation.
* The users you want to budget must belong to a Harness user group.
* At least one AI connector with observed spend must exist for the user group. Harness blocks budget creation for users who have no AI spend data on any connected provider.
* To create or manage budgets, you need the **Cost Governance** permission or the dedicated User Budgets RBAC role (if enabled on your account).

## Create a user budget

Select **User Budgets** from the left navigation bar, then click **+ Create budget**.

### Step 1: Budget setup

Configure who this budget applies to and what it covers.

**Specify users**

Select **Harness user groups** and choose one or more existing user groups from the dropdown. To create a new group immediately, click **+ Create new**.

**Applies to**

Choose which AI providers this budget governs:

| Option | What it covers |
|---|---|
| All AI spend | Every connected AI provider, including providers you add in the future |
| Cursor | Cursor only |
| Claude Enterprise | Claude (Anthropic) only |
| Amazon Bedrock | Amazon Bedrock only |
| GitHub Copilot | GitHub Copilot (enforcement in progress) |

To apply different limits per provider, create a separate budget for each provider.

**Budget name and folder**

Give the budget a descriptive name (for example, `CCM Team - Cursor`). Select a folder to keep budgets organized.

**Budget period and start date**

Choose **Weekly**, **Monthly**, or **Quarterly**. The spend amount resets at the start of each new period; unused allowance does not roll over.

Click **Next** to continue.

### Step 2: Enforcements and notifications

Configure the per-user limit, approval workflow, and what happens when a user reaches their cap.

**Budget per user**

Enter the dollar amount each user in the group is allowed to spend per period. The scope shown in the top-right corner (for example, **All AI spend**) reflects what you selected in Step 1.

**Allow users to request a higher limit**

Toggle this on to let users request an increase from their profile. When enabled, configure one or more approval tiers:

| Field | Description |
|---|---|
| Requests up to | The dollar ceiling for this tier. A request routes to the lowest tier whose ceiling covers the requested amount. |
| Approved by | The person who approves requests at this tier. Select **Auto-approve** to approve requests automatically up to this ceiling without human review. |

Click **+ Add approval tier** to stack multiple tiers. The highest tier's ceiling is the maximum any user can ever request. Approvers can approve at a different amount than requested and add a note explaining the change.

**Advanced settings (optional)**

Expand **Advanced settings** to configure two per-budget guardrails:

* **Users can request an increase once spend reaches**: the percentage of their current budget they must consume before they can request more (default: 80%).
* **Maximum increase per request**: the largest single-request increase allowed (default: $125).

**Notifications and enforcement**

Click **+ Add percentage threshold** to add one or more rules. For each rule:

* Enter the spend percentage that triggers the rule (for example, `80`).
* Choose **Notify** to send an email to the affected user (and any additional recipients you add).
* Choose **Block** to revoke the user's access to the covered AI providers once the threshold is crossed.

You can add multiple rules. For example, notify at 80% and block at 100%.

{% hint style="info" %}
**Connector permission required for enforcement**

Block enforcement requires an AI Governance permission on the relevant connector. Go to the connector settings and enable the AI Governance checkbox to allow User Budgets to enforce against it. Without this permission, notifications still work, but access will not be blocked.
{% endhint %}

Click **Create** to save the budget.

## Monitor a user budget

Select a budget name from the User Budgets list to open its detail page.

### Summary tiles

| Tile | Description |
|---|---|
| Budget per user | The per-user limit and provider scope |
| Total budget | Sum of all users' limits for the current period |
| Week-to-date spend | Aggregate spend so far; shows the percentage of the budget used and which day of the period it is |
| Upgrade requests | Number of outstanding limit-increase requests; click **Review all** to act on them |

The **Active Enforcements** bar shows the notification and block thresholds configured for this budget.

### Per-user table

| Column | Description |
|---|---|
| User | User email |
| Budget | Their current per-period limit, which may exceed the default if a previous increase was approved |
| Status | **Good** (within limit), **At risk** (approaching the threshold), or **Over** (exceeded the limit) |
| Current spend | Dollar amount spent and percentage of their limit, shown as a progress bar |
| Enforcements | **Blocked** if an enforcement has been applied to this user |
| Upgrade requests | A pending request appears inline; click **Review** to approve or reject it directly from the table |

Use the **Enforcements** and **Upgrade requests** filters above the table to narrow the view.

## Approve or reject a limit increase request

When a user submits a request, the designated approver receives a notification and a direct share link they can open to act on the request immediately. Only the approver for the matching tier can approve or reject from the share link; anyone else sees a "not your approval" message.

To review requests:

1. On the budget detail page, click the **Upgrade requests** tile or **Review all**.
2. For each request, you can:
   * **Approve**: accept the request. Optionally enter a different dollar amount to approve (the UI shows the resulting transition, for example $125, requested $250, approved $200) and add a note visible to the user.
   * **Reject**: decline the request.

Pending requests also appear inline in the per-user table, where you can click **Review** to act on them without leaving the budget detail page.

{% hint style="info" %}
Admins who are also listed as an approver for their own budget can approve their own requests.
{% endhint %}

## View your AI budget (end user)

Users can see their AI budget allocation and spending history from their profile.

1. Click your avatar or name in the bottom-left corner.
2. Select **Profile**.
3. Select the **AI Budgets** tab.

The page shows every budget you belong to. For each budget you can see:

* **Budget allocated**: your current per-period limit and provider scope.
* **Spend so far**: how much you have spent in the current period, with a progress bar and the days remaining.
* **Enforcements**: any active notify or block rules on your account.
* **Spend history**: a bar chart of your daily AI spend broken down by model and provider.
* **My requests**: a history of your limit-increase requests with status, approver, and any note the approver added.

### Request a limit increase

The **Request limit increase** button is available once your spend reaches the threshold configured on the budget (default: 80% of your limit).

1. On the **AI Budgets** tab, click **Request limit increase**.
2. In the **Requested new budget** field, enter the total budget amount you need. It must be higher than your current limit, and no higher than the ceiling of the highest approver tier.
3. Enter a **Reason** for the request.
4. Click **Submit request**.

The request routes to the appropriate approver tier based on the amount requested. If the amount falls within an auto-approve tier, the limit updates immediately.

To cancel a pending request, click **Revoke** next to it in the **My requests** list.

## Edit or delete a user budget

On the budget detail page, click **Edit Budget** to reopen the two-step wizard and change any setting. Click the three-dot menu for additional options including **Delete**.

{% hint style="danger" %}
Deleting a user budget removes all enforcement rules immediately. Users who were blocked regain access.
{% endhint %}

{% @harness-feedback/feedback %}
