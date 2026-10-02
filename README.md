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

{% content-ref url="user-budgets.md" %}
[user-budgets.md](user-budgets.md)
{% endcontent-ref %}

{% content-ref url="manage-user-budgets.md" %}
[manage-user-budgets.md](manage-user-budgets.md)
{% endcontent-ref %}

{% content-ref url="manage-your-ai-budget.md" %}
[manage-your-ai-budget.md](manage-your-ai-budget.md)
{% endcontent-ref %}

{% @harness-feedback/feedback %}
