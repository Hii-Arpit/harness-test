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

<figure><img src="../../.gitbook/assets/user-budgets-step2-approval-tiers.png" alt="Approval tiers with Requests up to and Approved by fields"><figcaption><p>Configure approval tiers when limit increase requests are enabled</p></figcaption></figure>

| Field | Description |
|---|---|
| Requests up to | The maximum amount this approver can approve. Harness sends the request to the approver whose tier ceiling matches or exceeds the requested amount. |
| Approved by | The approver for requests at this tier. |

Click **+ Add approval tier** to add multiple tiers. The highest tier's ceiling is the maximum any user can ever request.

{% hint style="success" %}
### Example

A user has a $125/month budget. Three approval tiers are configured:

| Tier | Requests up to | Approved by |
|---|---|---|
| 1 | $250 | Team lead |
| 2 | $500 | Engineering manager |
| 3 | $750 | VP of Engineering |

If the user requests $200, the team lead approves it. If they request $400, it goes to the engineering manager. No user can request more than $750, the ceiling of the highest tier.
{% endhint %}

#### Advanced settings (optional)

Expand **Advanced settings** to configure two guardrails:

<figure><img src="../../.gitbook/assets/user-budgets-step2-advanced-settings.png" alt="Advanced settings expanded"><figcaption><p>Advanced settings: request threshold and maximum increase per request</p></figcaption></figure>

| Field | Description |
|---|---|
| Users can request an increase once spend reaches | The percentage of their current budget a user must consume before they can submit a request. |
| Maximum increase per request | The largest single-request increase allowed. |

#### Notifications

Click **+ Add percentage threshold** to add a notification rule. For each rule:

* Set the spend percentage that triggers the rule in the **When spend reaches** field (for example, `90`).
* Click **+ Notify** to send an email notification to the affected user. You can add more recipients.
* Click **+ Block** to revoke the user's access to the covered AI providers once the threshold is crossed.

You can add multiple thresholds. For example, notify at 90% and block at 100%.

<figure><img src="../../.gitbook/assets/user-budgets-step2-notifications.png" alt="Notification rules configured with Notify and Block thresholds"><figcaption><p>Example: notify at 90% and block at 100%</p></figcaption></figure>

{% hint style="info" %}
**Connector permission required for block enforcement**

Block enforcement requires the AI Governance permission on the relevant connector. Go to the connector settings and enable the AI Governance checkbox to allow User Budgets to enforce against it. Without this permission, notifications still work, but access will not be blocked.
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

## References

* [Manage user groups](https://developer.harness.io/docs/platform/use-harness-platform/platform-access-control/add-user-groups) — Create and manage Harness user groups to use with User Budgets.

{% @harness-feedback/feedback %}
