---
description: >-
  Measure AI coding agent adoption, output, and cost-effectiveness across your
  engineering organization.
---

# AI Engineering Insights [Beta]

{% hint style="success" %}
This feature is in Limited GA and requires enablement. [View all early access features.](https://app.gitbook.com/s/RdSRdIerXDmqsO6h9KZy/features)

{% @harness-feature-flag-access/request-feature-flag-access module="ai-dlc-insights" pagePath="ai-dlc-insights/use-ai-dlc-insights/view-insights/ai-engineering" featureFlags="%5B%7B%22name%22%3A%22Enable%20Agentic%20AI%20Summaries%22%2C%22key%22%3A%22AGENTIC_AI_SUMMARY%22%2C%22status%22%3A%22LIMITED_GA%22%2C%22description%22%3A%22Enables%20agentic%20AI-generated%20summaries%20that%20go%20beyond%20static%20explanations%20by%20incorporating%20contextual%20reasoning%20and%20dynamic%20interpretation%20across%20multiple%20signals.%22%7D%5D" %}
{% endhint %}

{% if "HAS_FEATURE_FLAG" === true %}
```
{
    "featureFlags": [
        {
            "key": "ENABLE_AI_SUMMARY",
            "name": "Enable AI Summaries",
            "status": "GA",
            "description": "Enables AI-generated summaries across insights in Harness AIDI, providing high-level, contextual explanations of metric trends and patterns."
        },
        {
            "key": "AGENTIC_AI_SUMMARY",
            "name": "Enable Agentic AI Summaries",
            "status": "BETA",
            "description": "Enables agentic AI-generated summaries that go beyond static explanations by incorporating contextual reasoning and dynamic interpretation across multiple signals."
        }
    ]
}
```
{% endif %}

AI Engineering Insights provides real-time visibility into how developers use AI coding tools across your organization. It surfaces agent-level signals alongside engineering outcomes so you can measure adoption, quantify code output, track cost efficiency, and compare AI-assisted development against unassisted work.

{% hint style="info" %}
This dashboard is powered by the [Harness AI DLC Agent](../setup/agent/README.md). The agent must be deployed to developer machines before data appears here.
{% endhint %}

### Before you begin <a href="#before-you-begin" id="before-you-begin"></a>

* **Harness AI DLC Agent:** Install the agent on developer machines to collect AI coding telemetry. Go to [Harness AI DLC Agent](../setup/agent/README.md) to set up the agent.
* **Insight access:** Contact [Harness Support](https://app.gitbook.com/s/EYDRcFDt1JPlcws7L5VL/troubleshooting-and-resources/sei-support) to enable AI Engineering Insights for your organization.
* **Org Tree (optional):** Create an [Org Tree](https://app.gitbook.com/s/EYDRcFDt1JPlcws7L5VL/use-software-engineering-insights/setup-sei/setup-org-tree) to slice metrics by team or manager.

### Video walkthrough <a href="#video-walkthrough" id="video-walkthrough"></a>

The following video provides an overview of the AI Engineering Insights dashboard, including how to interpret key metrics and filter data by team.

{% embed url="https://www.youtube.com/embed/Np3ZNeFwdwY?si=T7s8Tk_rRlhaHF--" %}

### Dashboard overview <a href="#dashboard-overview" id="dashboard-overview"></a>

On first load, you see an organization-level view of all AI activity across all developers in your organization.

![AI Engineering Insights organization view](../../.gitbook/assets/ai-eng-5.png)

_Organization-level view showing AI activity across all developers with the Harness AI DLC Agent installed._

If your account has Org Trees configured, expand the org tree panel on the left of the dashboard. Select a team or manager in the tree to filter all metrics to the developers within it.

![AI Engineering Insights with org tree](../../.gitbook/assets/ai-eng-15.png)

Use the date picker at the top right to adjust the time range (for example, **Last 7 Days**, **Last 12 Weeks**, or a custom range), and the granularity selector next to it to control how data is grouped in charts. The dashboard shows data only within the **Data available from** range displayed above the cards.

![Custom date range selector](../../.gitbook/assets/custom-date-range.png)

### Adoption <a href="#adoption" id="adoption"></a>

Measures how widely AI coding agents are adopted across your organization. The goal of adoption metrics is to help you measure not just access and utilization but actual usage and volume of work done using AI.

![AI Engineering Insights Adoption view](../../.gitbook/assets/ai-eng-6.png)

_Organization-level view showing AI adoption metrics all developers with the Harness AI DLC Agent installed._

| Metric                   | Description                                                                                               |
| ------------------------ | --------------------------------------------------------------------------------------------------------- |
| **Adoption %**           | Percentage of total developers actively using an AI coding agent, shown as a donut chart.                 |
| **Active developers**    | Number of developers with agent activity out of total developers (for example, 40 of 60).                 |
| **Agent installed**      | Number of developers who have the Harness AI DLC Agent installed out of total developers.                 |
| **AI Committed Code %**  | Percentage of committed lines generated by AI coding agents (AI lines committed / total lines committed). |
| **Sessions/Dev/Week**    | Average number of AI coding sessions per developer per period.                                            |

**What to look for:** A low adoption percentage with high session duration among active users suggests a small group is getting value but the tool has not spread. Investigate license distribution and onboarding gaps.

<details>

<summary>Metric calculations</summary>

**Adoption rate:**



Active Users is the count of distinct developers with at least one recorded AI coding agent session in the selected period. Total Developers is all developer records imported into the system who have the AI DLC Agent installed in their workstations.

**AI Committed Code %:**



A line is considered AI-generated if the agent session that produced it is correlated to the commit via session-to-commit attribution.

**Sessions/dev/granularity:**



For example, for a monthly granularity over a 6-month range: Total Sessions / Active Users / 6.

**Avg session duration:**



Session start is captured at the pre-hook and session end at the post-hook.

</details>

### Impact <a href="#impact" id="impact"></a>

Measures the tangible output generated by AI agents, including lines of code written, how much of that code was committed, and how AI-assisted development compares to unassisted development.

| Metric                 | Description                                                                                                                                              |
| ---------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **AI Lines Generated** | Total lines of code produced by AI coding agents during sessions, counted at generation time regardless of whether the developer kept or discarded them. |
| **AI Lines Committed** | Lines of AI-generated code that were actually committed to a repository branch.                                                                          |

The tile shows AI lines committed out of total AI lines generated (for example, 3.31M committed out of 8.55M generated) as a bar chart, with the committed and generated lines listed for each agent.

![AI Engineering Insights Impact view](../../.gitbook/assets/ai-eng-7.png)

_Organization-level view showing AI impact metrics all developers with the Harness AI DLC Agent installed._

**What to look for:** If the ratio of committed lines to generated lines is consistently low across a team or agent, it may indicate that AI output does not match project conventions, or that developers are using AI for exploration rather than production code. Go to [Leaderboard](ai-engineering.md#leaderboard) to review Ship Rate by developer and identify where to invest in better context files (`CLAUDE.md`, `.cursorrules`) for those repositories.

### Correlate AI usage with engineering outcomes <a href="#correlate-ai-usage-with-engineering-outcomes" id="correlate-ai-usage-with-engineering-outcomes"></a>

The following cards show engineering output delivered through each AI coding agent. Each card lists the agents in descending order, with a count and the share of the total. Select **View breakdown** on a card to see the detail behind it.

![PR Merged, PR Cycle Time, and Work Items Delivered cards](../../.gitbook/assets/ai-eng-11.png)

### PR Merged <a href="#pr-merged" id="pr-merged"></a>

Total number of pull requests merged in the selected range and the number of developers who merged them. The list shows the PR count and percentage share for each agent. PRs are included if their merge timestamp falls within the selected range. Requires an SCM connection.

### PR Cycle Time <a href="#pr-cycle-time" id="pr-cycle-time"></a>

Time from PR open to merge, broken down into **Coding**, **Review**, and **Merge** phases for each agent. Each row also shows the PR count and the number of developers behind the value. Use the headline picker to switch between **Mean**, **Median**, **p90**, and **p95**. A second selector filters by work type: **All**, **Features**, **Bugs**, or **Others**.

$$
\text{Cycle Time} = \text{AGGREGATE}(\text{merge\_timestamp} - \text{open\_timestamp})
$$

Only PRs containing at least one AI-attributed commit are attributed to an agent. The aggregation function changes with the headline picker.

### Work Items Delivered <a href="#work-items-delivered" id="work-items-delivered"></a>

Total number of work items delivered in the selected range, with the count and share for each agent. Filter by **All**, **Features**, **Bugs**, or **Others**.

A work item is considered AI-assisted if any PR linked to it contains AI-attributed commits. Requires an issue tracker integration and SCM connection. One issue linked to multiple PRs counts once.

### Efficiency <a href="#efficiency" id="efficiency"></a>

Efficiency is measured through two lenses. **Spend efficiency** answers whether your organization is spending on AI in an optimized way (total spend, spend per developer, spend per commit). **Usage efficiency** answers whether developers are using AI models to their best capabilities (cache utilization, error rates, session duration).

![AI Engineering Insights Efficiency view](../../.gitbook/assets/ai-eng-8.png)

### Spend efficiency <a href="#spend-efficiency" id="spend-efficiency"></a>

| Metric                   | Description                                                                                |
| ------------------------ | ------------------------------------------------------------------------------------------ |
| **Total spend**          | Total AI agent spend for the selected period.                                              |
| **Code committed**       | Portion of spend attributed to sessions that produced committed code, shown on the bar.    |
| **Spend/Dev/Week**       | Average AI agent spend per active developer per week.                                      |
| **Ship Rate**            | Percentage of AI-generated lines that were committed (AI Lines Committed / AI Lines Generated x 100). |
| **Spend/Commit**         | Average AI agent cost per code commit, shown in the row of tiles below the cards.          |

**What to look for:** A large gap between total spend and the code committed portion indicates developers are running sessions that produce no committed code. Compare it against the AI Enabled Repos count: repositories without context files tend to produce lower returns. If Spend/Commit is rising while AI Committed Code % remains flat, investigate whether sessions are becoming longer without proportional output.

### Usage efficiency <a href="#usage-efficiency" id="usage-efficiency"></a>

| Metric               | Description                                                                                                                                                   |
| -------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Cache Hit Rate**   | Percentage of AI requests served from the model prompt cache. A higher rate indicates more efficient token reuse and lower cost.                              |
| **AI Enabled Repos** | Repositories with AI-ready context files (for example, `CLAUDE.md`, `AGENTS.md`, `.cursorrules`) that provide agents with project conventions and guardrails. |
| **Error Rate**       | Percentage of AI agent tool calls that ended in an error or failure.                                                                                          |
| **Avg Session Duration** | Average length of an AI coding session from start to finish, across all agents.                                                                           |

![AI Usage efficiency metrics](../../.gitbook/assets/ai-eng-9.png)

**What to look for:** A low Ship Rate combined with a high Error Rate suggests the agent is failing frequently and producing code that developers discard. Check whether affected repositories have context files and whether the error types point to misconfigured tools or network issues. A high Cache Hit Rate is a positive signal that prompts are well-structured and reusing context efficiently.

<details>

<summary>Metric calculations</summary>

**Total spend:**



Summed across all sessions, developers, agents, and models in the selected period. Token prices are per-model (for example, Claude Sonnet input = $3/MTok, output = $15/MTok). Cached tokens use the cached price tier.

**Spend/Dev/Week:**



Only active users (developers with at least one session) are included, not total org headcount.

**Spend/commit:**



Only commits containing at least one AI-attributed line are counted.

**Cache hit rate:**



Cached Prompt Tokens are tokens served from context cache (not reprocessed). Total Prompt Tokens are all input tokens sent to models.

**Ship Rate:**



**Error Rate:**



Failures include tool calls that returned an error, MCP generation errors, and file/command operation failures. Tracked per session then aggregated.

</details>

### Spend efficiency charts <a href="#spend-efficiency-charts" id="spend-efficiency-charts"></a>

![Spend Trend over Time chart](../../.gitbook/assets/ai-eng-spend-trend.png)

### Spend Trend over Time <a href="#spend-trend-over-time" id="spend-trend-over-time"></a>

Token spend over time as a stacked bar chart. Toggle the chart to view spend by **Agent** (for example, Claude Code) or **Model** (for example, `claude-sonnet-4-5-20250514`). The X-axis shows time intervals and the Y-axis shows spend in dollars.

For each time bucket $T$ and agent $A$:

$$
\text{Spend}(T, A) = \sum \text{token cost for sessions in bucket } T \text{ using agent } A
$$

### Spend Breakdown <a href="#spend-breakdown" id="spend-breakdown"></a>

Proportional breakdown of token spend, displayed as a donut chart next to a table of spend and percentage of total. Toggle between:

* **Agents:** Spend by agent type, showing each agent's dollar amount and percentage of total.
* **Top 10 Repos:** Spend by repository.
* **Models:** Spend by model identifier.

### Adoption charts <a href="#adoption-charts" id="adoption-charts"></a>

![Active Users % by Agent and Active Users % Trend](../../.gitbook/assets/ai-eng-10.png)

### Active Users % by Agent <a href="#active-users-percent-by-agent" id="active-users-percent-by-agent"></a>

Shows how many active developers use more than one coding agent. **Multi-Agent Users** and **Single-Agent Users** tiles show the developer count and the share of total. The table below lists each agent with its number of users and the percentage of developers using it.

For each agent $A$:

$$
\text{Agent Share} = \frac{\text{COUNT(distinct devs using agent } A\text{)}}{\text{Active Users}} \times 100
$$

A developer who uses two agents counts toward both, so the agent percentages can add up to more than 100%.

### Active Users % Trend <a href="#active-users-percent-trend" id="active-users-percent-trend"></a>

Tracks adoption growth over time. The chart plots the percentage of developers with active AI coding sessions across the selected time range and granularity (for example, weekly across the last 12 weeks). Toggle between **All Agents** and **By Agent** to see the trend for each agent separately.

### AI Committed Code Breakdown <a href="#ai-committed-code-breakdown" id="ai-committed-code-breakdown"></a>

Distribution of AI-authored code across categories. Each bar represents one category's share of all AI-written code for the selected period.

![AI Committed Code Breakdown and Spend Breakdown](../../.gitbook/assets/ai-eng-13.png)

For each agent $A$:

$$
\text{Agent Share} = \frac{\text{SUM(committed lines attributed to sessions using agent } A\text{)}}{\text{Total AI Lines Committed}} \times 100
$$

Use the dropdown to switch between:

* **Language:** Breakdown by programming language (for example, Go 35.9%, TypeScript 33.4%).
* **Type:** Breakdown by code type: Code, Documentation, Script, Test, Configuration, Migration, or Other.

### Leaderboard <a href="#leaderboard" id="leaderboard"></a>

The Leaderboard shows how developers, teams, or repositories use AI coding agents. Select the **Developers**, **Teams**, or **Repositories** tab to change the grouping.

Four tiles group developers into segments. Select a tile to list the developers in that segment.

| Segment         | Definition                                                                                  |
| --------------- | ------------------------------------------------------------------------------------------- |
| **Champions**   | Active days of 33 or more, code ship rate above 60%, and cost per KLOC at or below the 75th percentile. |
| **Overspending** | Cost per KLOC committed above the 75th percentile.                                         |
| **Ramping Up**  | Everyone else with more than zero active days.                                              |
| **Inactive**    | Zero active days in the period.                                                             |

![AI Engineering Insights Leaderboard view](../../.gitbook/assets/ai-eng-14.png)

The table below the tiles lists the developers in the selected segment. Select a column header to sort.

| Column                  | Description                                                           |
| ----------------------- | --------------------------------------------------------------------- |
| **Developer**           | Developer name.                                                       |
| **Agent**               | AI coding agents used by the developer.                               |
| **Tokens**              | Total token consumption for the selected period.                      |
| **Spend**               | Total AI agent spend attributed to the developer.                     |
| **AI Assisted PRs %**   | Percentage of the developer's merged PRs that include AI-attributed commits. |
| **AI Committed Code %** | Percentage of the developer's committed lines generated by AI, with the lines of code committed. |
| **Ship Rate**           | Percentage of AI-generated code that was committed by this developer. |
| **Sessions**            | Number of AI coding sessions in the selected period.                  |

{% hint style="info" %}
**USING THE OLDER AI INSIGHTS DASHBOARD?**

AI Engineering Insights replaces the earlier AI Insights experience. The previous version used AI coding assistant APIs only. AI Engineering Insights uses a hybrid approach: a lightweight agent on developer machines combined with API data, enabling richer telemetry and more accurate commit attribution. The AI Insights experience will be deprecated and removed in a future release.
{% endhint %}

{% @harness-feedback/feedback %}
