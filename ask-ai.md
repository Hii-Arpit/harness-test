---
description: Ask AI in CACM
---


# Ask AI in CACM

## Ask AI overview <a href="#what-is-ask-ai" id="what-is-ask-ai"></a>

<figure><img src="../.gitbook/assets/ask-ai-overview.png" alt="Ask AI in CACM overview"><figcaption><p>Click to view full size image</p></figcaption></figure>

**Ask AI** is Harness AI built directly into Cloud & AI Cost Management (CACM). It lets you have a natural-language conversation with your cost data and configuration - no need to memorize where filters live, build charts by hand, or read through long reports to find an answer.

Think of it as an assistant that already knows your account's cloud spend, commitments, perspectives, cost categories, and governance rules. You ask a question or describe what you want, and it responds with a context-aware answer, an explanation, or by helping you build something.

### Ask AI capabilities

Use Ask AI for the following types of requests:

* **Get answers, fast** - ask about spend, trends, top cost drivers, and anomalies in plain English.
* **Understand the "why"** - have a number or chart explained in context (e.g. why utilisation is low, what caused a spike).
* **Build things conversationally** - create Perspectives and Cost Categories by describing them instead of clicking through builders.
* **Edit and refine** - adjust an existing configuration by telling the AI what to change.
* **Author policies** - generate and validate Asset Governance rules from a plain-language description.
* **Explore and iterate** - treat it as a back-and-forth conversation, drilling down with follow-up questions.

### How it works

Each place Ask AI appears is **context-aware** - it knows which page you are on and pre-loads relevant suggested prompts to get you started. You will recognize it by the **Ask AI** pill, an AI sparkle icon, or an **Edit with AI** button. Click it, pick a suggested prompt or type your own, and continue the conversation.

> **Note:** Ask AI is available only when Harness AI is enabled for your account and you have accepted the AI end-user license agreement (EULA). If you do not see these options, contact your account administrator.

***

## The gap we are bridging <a href="#the-gap-were-bridging" id="the-gap-were-bridging"></a>

CACM already has the data and the tools - but getting from raw cost data to real answers and action has traditionally required effort and expertise. **Ask AI closes the gap between&#x20;**_**having**_**&#x20;cost data and actually&#x20;**_**using**_**&#x20;it.**

* **Knowledge gap (where/how to look)** - You no longer need to know that Perspectives, Cost Categories, filters, group-bys, coverage reports, or governance rules exist and how to configure each one. Just ask in plain language.
* **Analysis gap (data → insight)** - Dashboards show _what_ your costs are, but not _why_ they changed or _what to do next_. Ask AI turns numbers into explanations and recommendations.
* **Authoring gap (intent → configuration)** - Building a Perspective/View, a Cost Category, or a governance rule is manual and error-prone. Ask AI (and AIDA) translate your intent into working configuration, so you describe the outcome instead of assembling it step by step.
* **Expertise gap (specialist skills)** - Governance rules need policy-YAML knowledge; commitment analysis needs FinOps fluency. Ask AI and AIDA let non-experts produce expert-level output.
* **Time-to-value gap (onboarding)** - New users face a steep learning curve. Conversational entry points get them productive immediately, and contextual prompts deliver answers without navigating away.

> **In short:** Ask AI shifts CACM from _"here is your cost data, now figure it out and configure it yourself"_ to _"ask what you want, and get the answer - or the configuration - built for you."_

***

## Ask AI entry points <a href="#where-youll-find-ask-ai" id="where-youll-find-ask-ai"></a>

| Page / Feature                   | Entry point                                          | Best for                                              |
| -------------------------------- | ---------------------------------------------------- | ----------------------------------------------------- |
| **Overview / Cost Insights**     | "Ask AI for Cost Insights" pill + suggested prompts  | Understanding spend trends and where money is going   |
| **Commitment Orchestrator**      | "Ask AI about Commitments" pill + suggested prompts  | Analyzing RI/Savings Plan commitments and coverage    |
| **Commitment Utilisation cards** | "Ask AI" on each utilisation gauge                   | Explaining a specific utilisation number              |
| **Perspective (View) Builder**   | AI assistant opens with "Let us create a Perspective" | Building a cost perspective / View conversationally   |
| **Cost Category Builder**        | AI assistant + "Edit with AI" button                 | Creating and editing cost categories conversationally |

***

## 1. Overview / cost insights <a href="#id-1-overview-cost-insights" id="id-1-overview-cost-insights"></a>

<figure><img src="../.gitbook/assets/ask-ai-overview.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

### Ask AI on the Overview page

On the CACM **Overview** page, Ask AI appears as an **"Ask AI for Cost Insights"** pill alongside a set of suggested prompts. It is your entry point for understanding overall cloud spend - a conversational layer over the same data you see in the summary cards and charts, so you can ask "what is happening with my costs?" without building a single filter.

### What you can do with it

Ask high-level questions about spend across all your cloud providers and clusters, get plain-language explanations of trends and changes, and receive suggestions on where to focus cost-reduction efforts. It is designed for the "first look" moment when you open CACM.

### Use cases

The following scenarios illustrate how Ask AI can help on the Overview page:

* **Spot your biggest cost drivers** - find which providers, services, or accounts dominate spend this month.
* **Track month-over-month changes** - understand whether costs went up or down and by how much.
* **Investigate spikes** - ask what caused a sudden increase in a given period.
* **Find savings opportunities** - get high-level suggestions for reducing spend.
* **Compare providers** - see how AWS, Azure, GCP, and cluster costs stack up against each other.
* **Kick off deeper analysis** - start broad here, then follow up to narrow into a specific team, service, or time range.

### Example questions

The following prompts work well on the Overview page:

* "What are my top spenders this month across cloud providers?"
* "How have my costs changed over the month?"
* "What can I do to bring down spend?"
* "Which service grew the most compared to last month?"
* "Why did my AWS costs increase last week?"

### How to use it

To access Ask AI for cost insights:

1. Open the **Overview** page.
2. Click the **Ask AI for Cost Insights** pill, or pick one of the suggested prompts.
3. The AI assistant opens; type follow-up questions to drill deeper.

***

## 2. Commitment Orchestrator <a href="#id-2-commitment-orchestrator" id="id-2-commitment-orchestrator"></a>

<figure><img src="../.gitbook/assets/ask-ai-commitments.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

### Ask AI in Commitment Orchestrator

In the **Commitment Orchestrator** page header, Ask AI appears as an **"Ask AI about Commitments"** pill with commitment-focused suggested prompts. It helps you make sense of your Reserved Instance (RI) and Savings Plan (SP) landscape - coverage, gaps, and optimization opportunities - without manually cross-referencing multiple coverage and utilisation reports.

Also, on the **Commitment Utilisation** summary, each gauge card (Overall, Savings Plan, Reserved Instances) has its own **Ask AI** action. Unlike the broader Commitment Orchestrator prompt, this is hyper-focused: it pre-fills a question with the exact utilisation percentage you are looking at, so the AI explains _that specific number_ in context.

### What you can do with it

Ask the AI to analyze your commitment portfolio, surface where coverage is strong or weak, and highlight instance types or families that need attention. It turns a complex commitments picture into a conversation.

### Use cases

The following scenarios illustrate how Ask AI can help in Commitment Orchestrator:

* **Analyze your commitment landscape** - get an overview of your AWS EC2/RDS/ElastiCache commitments.
* **Find coverage gaps** - identify instance types or families with the least coverage that may be running on-demand unnecessarily.
* **Spot over-commitment** - find where you have committed more than you are using.
* **Prioritize action** - understand which changes would have the biggest savings impact.
* **Understand RI vs SP mix** - reason about how Reserved Instances and Savings Plans combine in your coverage.
* **Plan purchases** - explore what additional commitments could improve coverage.

### Example questions

The following prompts work well in Commitment Orchestrator:

* "Analyze my AWS commitments."
* "Analyze my AWS EC2 commitment landscape."
* "Which instance types/families have the most and least coverage?"
* "Where am I running on-demand that could be covered by a commitment?"
* "What should I prioritize to improve my coverage?"

### How to use it

To access Ask AI in Commitment Orchestrator:

1. Go to **Commitment Orchestrator**.
2. In the page header, click the **Ask AI about Commitments** pill or a suggested prompt or on any gauge card (Overall, Savings Plan, Reserved Instances), click the **Ask AI** action.
3. Continue the conversation for deeper analysis.

***

## 3. Perspective (view) builder <a href="#id-3-perspective-view-builder" id="id-3-perspective-view-builder"></a>

<figure><img src="../.gitbook/assets/ask-ai-perspective.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

### Ask AI in the Perspective builder

A **Perspective** - also referred to as a **View** - is a custom, saved view of your cloud spend, defined by rules and filters that slice costs the way your business thinks about them (by team, environment, product, etc.). In the **Perspective Builder**, Ask AI opens automatically with the prompt **"Let us create a Perspective"** and lets you build that view by describing it in plain language instead of manually configuring every rule and filter.

> **Note on terminology:** Perspectives are increasingly surfaced as **Views**, and newer Views can be edited in the **Cost Explorer** (which adds capabilities like custom saved time ranges, unit costs, and AI cost tracking that the legacy Perspective builder does not support). You may see both "Perspective" and "View" in the product - they refer to the same concept.

### What you can do with it

Describe the view you want and let the assistant translate your intent into perspective rules - choosing the right filters, group-bys, and conditions for you. You stay in control and can refine as you go.

### Use cases

The following scenarios illustrate how Ask AI can help in the Perspective builder:

* **Build a perspective from a description** - e.g. "group my AWS spend by team and environment."
* **Filter to what matters** - scope a view to specific accounts, services, regions, or tags.
* **Set the grouping** - ask for spend grouped by a dimension without knowing the exact field names.
* **Combine conditions** - express multi-condition rules conversationally (e.g. production workloads for one team).
* **Speed up onboarding** - help new users create their first perspective without learning the full builder.
* **Iterate quickly** - adjust the perspective by asking for changes rather than reworking rules manually.

### Example questions

The following prompts work well in the Perspective builder:

* "Let us create a Perspective for my production AWS spend."
* "Group my costs by team and environment."
* "Show only GCP spend for the data-platform project."
* "Add a filter for the us-east-1 region."

### How to use it

To access Ask AI in the Perspective builder:

1. From the **Perspectives** list, start creating a new Perspective.
2. The AI assistant opens automatically with an initial prompt.
3. Describe the view you want; the assistant helps assemble it, and you can refin

***

## 4. Cost category builder <a href="#id-4-cost-category-builder" id="id-4-cost-category-builder"></a>

<figure><img src="../.gitbook/assets/ask-ai-costcategory.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

### Ask AI in the Cost Category builder

A **Cost Category** groups spend into meaningful business buckets (for example: teams, departments, products, or cost centers), so shared cloud costs can be allocated and reported in terms your organization understands. In the **Cost Category Builder**, Ask AI opens with the prompt **"Let us create a Cost Category"** to help you define those buckets conversationally, and an **Edit with AI** button lets you refine existing buckets later.

### What you can do with it

Create cost buckets by describing your organizational structure, then edit and reorganize them through conversation - no need to hand-configure each bucket's rules one by one.

### Use cases

The following scenarios illustrate how Ask AI can help in the Cost Category builder:

* **Create a cost category from scratch** - describe your teams/departments and let AI define the buckets.
* **Map spend to your org** - allocate shared costs to business units, products, or cost centers.
* **Define bucket rules** - express what belongs in each bucket in plain language.
* **Edit existing buckets** - use **Edit with AI** to rename, merge, split, or adjust buckets after creation.
* **Handle shared/unattributed costs** - ask how to allocate costs that do not map cleanly to one bucket.
* **Standardize reporting** - build categories that make chargeback/showback reports meaningful.

### Example questions

The following prompts work well in the Cost Category builder:

* "Let us create a Cost Category for my engineering teams."
* "Create buckets for Platform, Data, and Frontend teams."
* "Add a bucket for shared infrastructure costs."
* "Edit this category to split the Platform bucket by environment."

### How to use it

To access Ask AI in the Cost Category builder:

1. From the **Cost Categories** list, start creating a new Cost Category.
2. The AI assistant opens with an initial prompt.
3. As you add cost buckets, use **Edit with AI** to adjust them.

***

## Tips for better answers <a href="#tips-for-better-answers" id="tips-for-better-answers"></a>

Apply these practices to get more relevant and actionable responses from Ask AI:

* **Be specific about scope** - mention the cloud provider, time range, team, or service you care about.
* **Start with a suggested prompt** - then refine with follow-up questions.
* **Reference what you see** - on utilisation cards, the prompt already includes the exact figure, so ask "why" and "what next."
* **Iterate** - treat it as a conversation; narrow down step by step.

***

## Frequently asked questions <a href="#frequently-asked-questions" id="frequently-asked-questions"></a>

<details>

<summary>Why do not I see Ask AI anywhere?</summary>

Harness AI must be enabled for your account and the AI EULA accepted. Ask your administrator.

</details>

<details>

<summary>Is Ask AI the same everywhere?</summary>

The underlying assistant is shared across Overview, Commitment Orchestrator, Commitment Utilisation, Perspective Builder, and Cost Category Builder. Governance uses a separate copilot called **AIDA**.

</details>

<details>

<summary>Can Ask AI make changes for me?</summary>

In the Perspective and Cost Category builders it helps you construct and edit configurations. On analysis pages (Overview, Commitments) it answers questions and provides insights.

</details>

<details>

<summary>Does Ask AI use my actual cost data?</summary>

Yes - answers are grounded in your account's cost, commitment, and usage data for context-aware responses.

</details>

<details>

<summary>Are the suggested prompts the only things I can ask?</summary>

No. Suggested prompts are shortcuts. You can type any question and ask follow-ups.

</details>

## Next steps <a href="#next-steps" id="next-steps"></a>

* Go to [Cost Explorer](../cost-reporting/cost-explorer.md) to see Ask AI for spend trends.
* Go to [Commitment Orchestrator](../cost-optimization/commitment-orchestrator/overview.md) to analyze your RI and Savings Plan coverage with Ask AI.
* Go to [Perspectives](../cost-reporting/perspectives/README.md) to build a custom cost view.
* Go to [Cost categories](../cost-reporting/cost-categories/README.md) to group spend by team or department.

{% @harness-feedback/feedback %}
