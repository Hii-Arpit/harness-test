---
description: >-
  Connect your first billing provider, cloud or AI, and see cost flow into Cloud
  & AI Cost Management.
---


# Get Started with CACM

Connect your first cloud or AI provider and see your spend in one place. Add more providers at any point to expand your coverage.

***

### Before you begin <a href="#before-you-begin" id="before-you-begin"></a>

* **Cloud & AI Cost Management:** The Cloud & AI Cost Management module must be enabled on your Harness account, and you must have permission to create connectors. Go to [RBAC in Harness](https://developer.harness.io/harness-ai/use-harness-platform/platform-access-control) to confirm your role, or contact your account administrator.
* **Admin access to one billing provider:** A cloud account (AWS, Azure, or GCP), a Kubernetes cluster, or an AI provider account (OpenAI or Anthropic). Connect one to start.

***

### Step 1: Connect a billing provider <a href="#step-1-connect-a-billing-provider" id="step-1-connect-a-billing-provider"></a>

CACM brings cloud and AI spend into one place. Pick any one provider to connect. Each one sends cost data on its own, so start with any provider that fits your setup.

1. Open **Account Settings** in Cloud & AI Cost Management.
2. Click the **Integration for cloud & AI cost** tile.
3. On the **Cloud and AI Integration** page, select the tab for your provider: **Cloud Accounts**, **Kubernetes Clusters**, **AI Providers**, or **External Cost Data Sources**.
4. Click the **+ New** button on that tab. The button label changes per tab: **+ New Cloud Account**, **+ New Kubernetes Connector**, or **+ AI Provider**.
5. Select your provider from the picker and follow the setup wizard.

Go to [Cloud Providers](../integrations/cloud-providers/) or [AI Providers](../integrations/ai-providers/) to follow the per-provider setup guide.

***

### Step 2: Wait for the first sync <a href="#step-2-wait-for-the-first-sync" id="step-2-wait-for-the-first-sync"></a>

Allow up to 24 hours for data to sync from the provider. AI providers are usually faster, typically 6 to 12 hours.

{% hint style="info" %}
**HISTORICAL DATA AND BACKFILL**

The first sync loads recent cost data, and how far back it reaches depends on the provider. For example, AWS can backfill up to 36 months through AWS Support, while Kubernetes generates the last 30 days of cost from the first events it receives. Go to the provider's setup guide to review its backfill limits.
{% endhint %}

***

### Step 3: View and analyze your costs <a href="#step-3-view-and-analyze-your-costs" id="step-3-view-and-analyze-your-costs"></a>

Open **Cost Explorer**. Your spend now appears in one place, across all providers configured.

<figure><img src="../.gitbook/assets/cacm-overview.png" alt=""><figcaption><p>CACM overview showing spend across connected providers.</p></figcaption></figure>

From here you can:

* **Explore cost:** Break spend down by provider, service, region, account, or token type.
* **Group cost with Views:** Build [Views](../cost-reporting/cost-explorer.md) to slice cost by team, product, or environment.
* **Map cost to your business:** Use [Cost Categories](../cost-reporting/cost-categories/) to align spend with your organizational structure.
* **Set budgets and alerts:** Add [Budgets](../cost-governance/budgets/) and anomaly alerts on any view.

Once cost is flowing and you can see it in Cost Explorer, you are set up. Everything else builds on this.

***

### Next steps <a href="#next-steps" id="next-steps"></a>

* Go to [Navigating CACM](navigating-cacm.md) to see what CACM offers across visibility, governance, and optimization.
* Go to [AI Cost Management](ai-cost-management/ai-cost-management.md) to attribute AI spend to agents, sessions, and requests with trace-level detail.
* Go to [Harness CACM self-paced training](https://university-registration.harness.io/self-paced-training-harness-cloud-cost-management) for an interactive onboarding experience covering Perspectives, Budgets, and AutoStopping.

{% @harness-feedback/feedback %}
