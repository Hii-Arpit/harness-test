---
description: Set per-developer AI spend limits across AI providers to control costs and give each developer visibility into their own usage.
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
| Name | Budget name and folder |
| Budget | The per-user spend limit and provider scope |
| Rolled up spend | Aggregate AI spend across all users in the budget for the current period |
| User health | **OK** — all users within limit; **At risk** — one or more users nearing their threshold; **Exceeded** — one or more users over the limit |
| Notifications and enforcement | Active alert and block rules configured for the budget |

Use the **Folder** panel on the left to filter budgets by folder. You can also filter the list by user health and enforcement status using the dropdowns above the table.

{% content-ref url="manage-user-budgets.md" %}
[manage-user-budgets.md](manage-user-budgets.md)
{% endcontent-ref %}

{% @harness-feedback/feedback %}
