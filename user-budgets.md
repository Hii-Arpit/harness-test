---
description: Set per-developer spend limits across AI tools, with enforcement, approvals, and self-service increase requests.
---

{% if "HAS_FEATURE_FLAG" === true %}
```
{
    "featureFlags": [{
        "key": "CCM_USER_BUDGETS",
        "name": "AI User Budgets",
        "status": "LIMITED_GA",
        "description": "Enables the User Budgets section under Cost Governance and the AI Budgets view in user profiles. Admins can create per-user AI spending budgets with configurable approval workflows and enforcement actions."
    }]
}
```
{% endif %}

# User Budgets

User Budgets let you define a spending cap for each developer on AI tools such as Cursor, Claude Enterprise, and Amazon Bedrock. When a developer approaches or exceeds their limit, Harness notifies them and, optionally, blocks their access. Developers who need more can request a limit increase directly from their profile — no support ticket required.

User Budgets are separate from [resource budgets](../budgets/create-a-budget.md), which track cloud infrastructure spend against cost perspectives. Resource budgets are scoped to a perspective; user budgets are scoped to individual users and AI providers.

## User Budgets list

Select **User Budgets** from the left navigation bar to see all budgets in your account.

| Column | Description |
|---|---|
| Name | Budget name, folder, and the number of users in the budget |
| Budget | The per-user spend limit and billing period (for example, $75.00/wk) |
| Rolled up spend | Aggregate AI spend across all users in the budget for the current period, shown as a percentage and progress bar |
| User health | **Good** — all users within limit; **At risk** — one or more users nearing their threshold; **Over** — one or more users have exceeded the limit |
| Notifications and enforcements | Active alert and block rules configured for the budget |
| Upgrade requests | Pending limit-increase requests submitted by users in the budget |

Use the **Folders** panel on the left to filter budgets by folder. Use the search bar to find a specific budget by name.

{% content-ref url="manage-user-budgets.md" %}
[manage-user-budgets.md](manage-user-budgets.md)
{% endcontent-ref %}

{% @harness-feedback/feedback %}
