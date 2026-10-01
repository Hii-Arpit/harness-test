---
description: How Harness Autostopping calculates savings, what to expect, and how to troubleshoot common issues.
---


# Autostopping Savings Calculation Guide

Harness Autostopping automatically shuts down idle cloud resources and brings them back up on-demand, significantly reducing your cloud costs. This guide explains how savings are calculated, what to expect, and answers common questions.

## How savings are calculated <a href="#how-savings-are-calculated" id="how-savings-are-calculated"></a>

Autostopping calculates savings by comparing two scenarios:

1. **Potential Cost**: What you would have paid if the resource ran continuously without AutoStopping
2. **Actual Cost**: What you actually paid based on real usage with Autostopping enabled

```text
Savings = Potential Cost - Actual Cost
Savings = Potential Cost - Actual Cost
Savings Percentage = (Savings / Potential Cost) × 100  [undefined if Potential Cost = 0]
```

### Example: if a VM costs $0.10/hour and Autostopping kept it off for 20 hours in a day:

* Potential Cost: $0.10 × 24 hours = $2.40
* Actual Cost: $0.10 × 4 hours = $0.40
* **Savings: $2.00 (83%)**

***

## Savings modes <a href="#savings-modes" id="savings-modes"></a>

### Active mode (actual savings) <a href="#active-mode-actual-savings" id="active-mode-actual-savings"></a>

When an Autostopping rule is **enabled and actively managing life cycle of resources**, savings represent real money saved. The system tracks:

* When resources are started (warm-up)
* When resources are stopped (cool-down)
* Actual cloud billing costs during active periods

### Dry run mode (projected savings) <a href="#dry-run-mode-projected-savings" id="dry-run-mode-projected-savings"></a>

When a rule is in **dry run mode**, it monitors traffic patterns without actually stopping resources. Dry run savings show what you _would_ save if the rule were fully enabled.

### Use dry run mode to:

* Evaluate potential savings before enabling Autostopping
* Validate traffic patterns and idle detection
* Build confidence before production deployment

***

## Data sources <a href="#data-sources" id="data-sources"></a>

AutoStopping Savings calculations makes use of actual billing data from your cloud provider:

| Cloud Provider | Data Source                 |
| -------------- | --------------------------- |
| **AWS**        | Cost and Usage Report (CUR) |
| **GCP**        | BigQuery Billing Export     |
| **Azure**      | Cost Management API         |

### Cloud billing data delay <a href="#billing-data-delay" id="billing-data-delay"></a>

Cloud providers typically have a **24-72 hour delay** in billing data availability. This means:

* Savings for today may not appear until 1-3 days later
* This is normal cloud provider behavior, not an Autostopping limitation
* Historical savings are backfilled once billing data arrives

***

## Calculation timing <a href="#calculation-timing" id="calculation-timing"></a>

Costs (Spend/Savings/Freed-up Capacity) are calculated with a **2-day delay** to ensure data accuracy.

* **Daily recomputation** covers the previous 5 days
* **Monthly recomputation** between the 15th–20th adjusts the last 45 days to accommodate changes happened on already computed savings numbers

Cost numbers may change as billing data from your cloud provider finalizes.

***

## Savings dashboard <a href="#understanding-your-savings-dashboard" id="understanding-your-savings-dashboard"></a>

### Key metrics <a href="#key-metrics" id="key-metrics"></a>

| Metric                 | Description                                     |
| ---------------------- | ----------------------------------------------- |
| **Actual Savings**     | Money saved by Autostopping (active rules only) |
| **Potential Cost**     | What you would have paid without Autostopping   |
| **Actual Cost**        | What you actually paid                          |
| **Idle Hours**         | Hours resources were stopped                    |
| **Savings Percentage** | Efficiency of your Autostopping configuration   |
| **Unrealized Savings** | Projected savings from dry run rules            |

### Per-resource breakdown <a href="#per-resource-breakdown" id="per-resource-breakdown"></a>

Savings are tracked at the individual resource level:

* Each VM, database, or container service has its own savings record
* Resources can be grouped by rule, account, or cloud provider
* Drill down to see daily, weekly, or monthly trends

***

## Common scenarios <a href="#common-scenarios" id="common-scenarios"></a>

### Scenario 1: rule enabled mid-day <a href="#scenario-1-rule-enabled-mid-day" id="scenario-1-rule-enabled-mid-day"></a>

If you enable an Autostopping rule at 2:00 PM:

* Savings calculation starts from 2:00 PM
* Hours before enablement are not included in potential cost
* First day shows partial savings

### Scenario 2: rule disabled mid-day <a href="#scenario-2-rule-disabled-mid-day" id="scenario-2-rule-disabled-mid-day"></a>

If you disable a rule at 10:00 AM:

* Savings are calculated only until 10:00 AM
* Remaining hours are excluded from the calculation
* The rule must be re-enabled to resume savings tracking

### Scenario 3: resource running at midnight <a href="#scenario-3-resource-running-at-midnight" id="scenario-3-resource-running-at-midnight"></a>

If a resource is actively running when the day changes:

* The session continues seamlessly into the next day
* Savings calculation accounts for cross-day sessions
* No manual intervention required

### Scenario 4: multiple start/stop cycles <a href="#scenario-4-multiple-startstop-cycles" id="scenario-4-multiple-startstop-cycles"></a>

If a resource starts and stops multiple times per day:

* Each session is tracked individually
* Total savings = sum of all idle periods
* Frequent cycling may indicate idle time settings need adjustment

***

## Frequently asked questions <a href="#frequently-asked-questions" id="frequently-asked-questions"></a>

### Why are my savings showing as zero? <a href="#why-are-my-savings-showing-as-zero" id="why-are-my-savings-showing-as-zero"></a>

Savings may display as zero for several reasons:

1. **Billing data has not arrived yet**
   * Allow 1-3 days for cloud billing data to become available
   * Check back after the billing delay period
2. **Rule was disabled**
   * Disabled rules do not generate savings
   * Re-enable the rule to resume savings tracking
3. **Resources ran continuously**
   * If there was no idle time, there are no savings
   * Review your idle time settings and traffic patterns
4. **New rule**
   * Newly created rules need time to accumulate data
   * First savings appear after the billing delay period
5. **Resources covered by Reserved Instances or Savings Plans**
   * If your resources are fully covered by RI/SP commitments, the hourly cost may show as $0
   * Savings = Potential Cost - Actual Cost, so if both are $0, savings will be $0
   * This is expected behavior - you are already getting discounts through your commitments
   * Check your cloud billing to verify RI/SP coverage

### Why is there a delay in savings appearing? <a href="#why-is-there-a-delay-in-savings-appearing" id="why-is-there-a-delay-in-savings-appearing"></a>

Cloud providers (AWS, GCP, Azure) have inherent delays in their billing systems:

* **AWS CUR**: Updated every 24 hours, may take up to 72 hours for complete data
* **GCP BigQuery**: Typically 24-48 hour delay
* **Azure**: 24-72 hour delay depending on resource type

This is standard cloud provider behavior. Savings will appear once billing data is available.

### Can savings ever be negative? <a href="#can-savings-ever-be-negative" id="can-savings-ever-be-negative"></a>

No. Savings are always zero or positive. If actual costs temporarily exceed potential costs (due to billing adjustments, credits, or data corrections), savings display as zero rather than a negative value.

### How accurate are the savings calculations? <a href="#how-accurate-are-the-savings-calculations" id="how-accurate-are-the-savings-calculations"></a>

Savings calculations use actual billing data from your cloud provider, making them highly accurate. The system:

* Uses real hourly rates from your billing data
* Accounts for pricing tiers, discounts, and reserved instances
* Tracks actual resource state changes

### What happens during cloud provider outages? <a href="#what-happens-during-cloud-provider-outages" id="what-happens-during-cloud-provider-outages"></a>

If billing data is temporarily unavailable due to cloud provider issues:

* The system retries for up to 5 days
* Once data becomes available, savings are calculated normally
* No manual intervention is required

### How do I maximize my savings? <a href="#how-do-i-maximize-my-savings" id="how-do-i-maximize-my-savings"></a>

1. **Optimize idle time settings**: Set appropriate idle timeouts based on your usage patterns
2. **Use schedules**: Configure fixed schedules for predictable workloads
3. **Enable more rules**: Extend Autostopping to additional resources
4. **Review dry run data**: Use dry run mode to identify savings opportunities
5. **Monitor regularly**: Check the dashboard to identify underperforming rules

***

## Savings calculation examples <a href="#savings-calculation-examples" id="savings-calculation-examples"></a>

### Example 1: EC2 instance <a href="#example-1-ec2-instance" id="example-1-ec2-instance"></a>

**Setup:**

* Instance type: m5.large ($0.096/hour)
* Autostopping idle time: 15 minutes
* Typical usage: 8 hours/day active, 16 hours/day idle

**Daily Calculation:**

```text
Potential Cost: $0.096 × 24 hours = $2.304
Actual Cost: $0.096 × 8 hours = $0.768
Daily Savings: $2.304 - $0.768 = $1.536 (67%)
Monthly Savings: $1.536 × 30 = $46.08
```

### Example 2: RDS database <a href="#example-2-rds-database" id="example-2-rds-database"></a>

**Setup:**

* Instance type: db.r5.large ($0.24/hour)
* Development database used only during business hours
* Active: 10 hours/day (8 AM - 6 PM)

**Daily Calculation:**

```text
Potential Cost: $0.24 × 24 hours = $5.76
Actual Cost: $0.24 × 10 hours = $2.40
Daily Savings: $5.76 - $2.40 = $3.36 (58%)
Monthly Savings: $3.36 × 22 workdays = $73.92
```

### Example 3: Kubernetes workload <a href="#example-3-kubernetes-workload" id="example-3-kubernetes-workload"></a>

**Setup:**

* Namespace with 3 pods
* Each pod: 2 vCPU, 4GB memory
* Spot instances at $0.05/hour per pod
* Active 6 hours/day for testing

**Daily Calculation:**

```text
Potential Cost: $0.05 × 3 pods × 24 hours = $3.60
Actual Cost: $0.05 × 3 pods × 6 hours = $0.90
Daily Savings: $3.60 - $0.90 = $2.70 (75%)
Monthly Savings: $2.70 × 30 = $81.00
```

***

## Troubleshooting <a href="#troubleshooting" id="troubleshooting"></a>

<details>

<summary>Savings Not Appearing</summary>

1. **Check rule status**: Ensure the rule is enabled and not in error state
2. Ensure billing enabled connector is available
3. **Verify billing integration**: Confirm cloud billing data is being ingested
4. **Wait for billing delay**: Allow 1-3 days for data to arrive
5. **Check resource activity**: Verify resources are actually being stopped

</details>

<details>

<summary>Unexpected Savings Values</summary>

1. **Review resource changes**: Instance type changes affect hourly rates
2. **Check for billing adjustments**: Credits or refunds may affect calculations
3. **Verify rule configuration**: Ensure correct resources are associated with the rule

</details>

<details>

<summary>Missing Historical Data</summary>

1. **Check rule creation date**: Savings only exist from when the rule was created
2. **Verify billing data availability**: Historical billing data may have retention limits
3. **Review recalculation status**: Monthly recalculations may be in progress

</details>

## Glossary <a href="#glossary" id="glossary"></a>

| Term               | Definition                                                             |
| ------------------ | ---------------------------------------------------------------------- |
| **Actual Cost**    | The real cost incurred based on actual resource usage                  |
| **Potential Cost** | The cost that would have been incurred if resources ran 24/7           |
| **Idle Time**      | The configured duration of inactivity before resources are stopped     |
| **Warm-up**        | The process of starting stopped resources when traffic is detected     |
| **Cool-down**      | The process of stopping resources after the idle time expires          |
| **Dry Run**        | A mode that simulates Autostopping without actually stopping resources |
| **Hourly Rate**    | The per-hour cost of a resource from cloud billing data                |
| **Billing Delay**  | The time between resource usage and billing data availability          |

{% @harness-feedback/feedback %}
