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

Select **User Budgets** from the left navigation bar, then click **+ Create budget** to open the **Create a new Budget** wizard.

### Step 1: Budget setup

| Field | Description |
|---|---|
| Specify users to apply budget | Select **Harness user groups** to reuse an existing Harness user group. |
| Select user groups | Choose one or more groups from the **Select user groups** dropdown. To create a new group, click **+ Create new**. To create a new user group, go to [Manage user groups](#references). |
| Applies to | Choose which AI providers this budget governs: <ul><li>**All AI spend**: Every connected AI provider, including new providers you connect in the future</li><li>**Cursor**: Spend from Cursor IDE</li><li>**Claude**: Spend from Claude (Anthropic)</li><li>**Amazon Bedrock**: Spend from Amazon Bedrock</li></ul> |
| Budget name | Descriptive name for the budget (for example, `CCM Team - Cursor`) |
| Folder | Select a folder to organize the budget. The dropdown lists all existing folders with a search bar to filter by name. If no folder is selected, the budget is placed in the **Default** folder. |
| Budget period | Choose the billing cycle: <ul><li>**Weekly**</li><li>**Monthly** (default)</li><li>**Quarterly**</li></ul> Spend resets at the start of each new period; unused allowance does not roll over. |
| Start date | Use the date picker to select the start date for the first budget period. Defaults to today's date. |

{% hint style="info" %}
To apply different limits per provider, create a separate budget for each provider.
{% endhint %}

### Step 2: Enforcements and notifications

Configure the per-user spending limit, approval workflow, and notification rules.

#### Budget per user

Enter the dollar amount each user in the group is allowed to spend per period. The provider scope badge (for example, **All AI spend**) in the top-right reflects what you selected in Step 1.

<figure><img src="../../.gitbook/assets/user-budgets-step2-overview.png" alt="Step 2: Enforcements and notifications"><figcaption><p>Step 2: Enforcements and notifications</p></figcaption></figure>

#### Allow users to request a higher limit

Turn this on to allow users to request a higher spending limit from their profile. You must then set up at least one approval tier to define who approves requests and the maximum amount they can approve.

| Field | Description |
|---|---|
| Requests up to | The maximum amount this approver can approve. Harness sends the request to the approver whose tier ceiling matches or exceeds the requested amount. |
| Approved by | The approver for requests at this tier. |

Click **+ Add approval tier** to add multiple tiers. The highest tier's ceiling is the maximum any user can ever request.

<figure><img src="../../.gitbook/assets/user-budgets-step2-approval-tiers.png" alt="Approval tiers with Requests up to and Approved by fields"><figcaption><p>Configure approval tiers when limit increase requests are enabled</p></figcaption></figure>

{% hint style="success" %}
### Example

A user has a $125/month budget and needs more. Three approval tiers are configured as follows, and Harness sends the request to the designated approver based on the amount requested:

| Tier | Requests up to | Approved by |
|---|---|---|
| 1 | $250 | Team lead |
| 2 | $500 | Engineering manager |
| 3 | $750 | VP of Engineering |

* A request for $200 goes to the team lead (Tier 1).
* A request for $400 goes to the engineering manager (Tier 2).
* A request for $700 goes to the VP of Engineering (Tier 3).
* No user can request more than $750, the ceiling of the highest tier.
{% endhint %}

#### Advanced settings (optional)

Expand **Advanced settings** to set limits on when users can request an increase and how much they can request:

| Field | Description |
|---|---|
| Users can request an increase once spend reaches | The percentage of their current budget a user must consume before they can submit a request. |
| Maximum increase per request | The largest single-request increase allowed. |

<figure><img src="../../.gitbook/assets/user-budgets-step2-advanced-settings.png" alt="Advanced settings expanded"><figcaption><p>Advanced settings: request threshold and maximum increase per request</p></figcaption></figure>

#### Notifications

Click **+ Add percentage threshold** to define when Harness should act. For each threshold:

* Set the spend percentage in the **When spend reaches** field.
* Click **+ Notify** to send an email to the affected user when the threshold is crossed. You can add more recipients.
* Click **+ Block** to revoke the user's access to the covered AI providers when the threshold is crossed.

For each threshold, you can choose to notify the user, block their access, or both. Add multiple thresholds to trigger different actions at different spend levels.

<figure><img src="../../.gitbook/assets/user-budgets-step2-notifications.png" alt="Notification rules configured with Notify and Block thresholds"><figcaption><p>Notify and Block configured on separate thresholds</p></figcaption></figure>

{% hint style="success" %}
### Example

Two thresholds are configured on the same budget:

| When spend reaches | Action |
|---|---|
| 90% | Notify the affected user |
| 100% | Block access for the affected user |

Alternatively, enable both **Notify** and **Block** on the same threshold — for example, notify and block access at 100% in one rule.
{% endhint %}

{% hint style="info" %}
**Connector permission required for block enforcement**

Block enforcement requires the AI Governance permission on the relevant connector. Go to the connector settings and enable the AI Governance checkbox to allow User Budgets to enforce against it. Without this permission, notifications still work, but access will not be blocked.
{% endhint %}

Click **Create** to save the budget.

## Monitor a user budget

Select a budget name from the User Budgets list to open its detail page.

<figure><img src="../../.gitbook/assets/user-budgets-detail-page.png" alt="Budget detail page"><figcaption><p>Budget detail page showing summary tiles, active enforcements, and per-user table</p></figcaption></figure>

### Summary tiles

| Tile | Description |
|---|---|
| Budget per user | The per-user limit, provider scope, and total number of users in the budget |
| Total weekly budget | Sum of all users' limits for the current period, with a breakdown of OK, At risk, and Exceeded users |
| Week-to-date spend | Aggregate spend so far, shown as a percentage of the total budget and which day of the period it is |
| Pending Requests | Number of outstanding limit-increase requests |

The **Active Enforcements** bar lists the notification and block rules currently applied to this budget. Each tag shows the action and the spend threshold that triggers it. For example, **Notify at 80%** sends an email when a user reaches 80% of their limit, and **Block all access at 100%** revokes their access when they hit the cap.

### Per-user table

| Column | Description |
|---|---|
| User | User email |
| Weekly budget | Their current per-period limit, which may exceed the default if a previous increase was approved |
| Status | **OK** (within limit), **At risk** (approaching the threshold), or **Exceeded** (over the limit) |
| Current spend | Dollar amount spent and percentage of their limit, shown as a progress bar |
| Enforcements | Active enforcement applied to this user |
| Request details | Details of any pending limit-increase request for this user |

{% hint style="success" %}
Use the **Enforcements** and **Requests** filters above the table to narrow the view to users with active enforcements or pending requests.
{% endhint %}

## Approve or reject a limit increase request

When a user submits a limit increase request, it goes to the approver assigned to the matching tier. You can act on it in two ways:

* **Open the review panel**: click **Review all** in the **Pending Requests** tile to see all users who have submitted a limit increase request for this budget. Alternatively, click **Review ↗** next to a specific user in the **Request details** column to go directly to their request.
* **Act directly from the table**: use the inline buttons in the **Request details** column — **Review ↗** to open the panel, (✓) to approve, or (✗) to reject without leaving the page.

<figure><img src="../../.gitbook/assets/user-budgets-pending-requests-inline.png" alt="Budget detail page showing Review all and inline Review button"><figcaption><p>Review all in the Pending Requests tile and inline Review in the Request details column</p></figcaption></figure>

The review panel shows the following details for the selected request:

| Field | Description |
|---|---|
| Current budget | The user's current per-period limit |
| WTD spend | How much the user has spent so far this period, and the percentage of their current limit |
| Requested upgrade | The transition from current to requested amount (for example, $125.00 → $130.00) |
| Approve for | The amount to approve. Defaults to the requested amount; you can change it to approve a different amount. |

<figure><img src="../../.gitbook/assets/user-budgets-request-details-history.png" alt="Request details panel with Request history tab"><figcaption><p>Request details panel showing Request history</p></figcaption></figure>

The panel also includes two tabs:

* **Request history**: shows the user's past requests grouped by budget period.
* **Spend breakdown**: shows a bar chart of the user's daily AI spend. Use the **By providers** dropdown (By providers, By models, By accounts, By model family, By token type) and **By days** dropdown (By days, By weeks, By months) to change the view. Click **View on explorer** to open the full Cost Explorer.

<figure><img src="../../.gitbook/assets/user-budgets-request-details-spend.png" alt="Request details panel with Spend breakdown tab"><figcaption><p>Spend breakdown tab showing daily AI spend by provider</p></figcaption></figure>

At the bottom, choose one of:

* **Approve**: confirm the amount in the **Approve for** field and approve the request.
* **Reject**: decline the request.
* **Skip for now**: defer the decision and move to the next request.

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

## References

* [Manage user groups](https://developer.harness.io/docs/platform/use-harness-platform/platform-access-control/add-user-groups) — Create and manage Harness user groups to use with User Budgets.

{% @harness-feedback/feedback %}
