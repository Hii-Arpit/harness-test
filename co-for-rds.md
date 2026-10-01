---
description: Learn how to use Commitment Orchestrator to optimize your AWS RDS costs
---


# Commitment Orchestrator For RDS

Commitment Orchestrator for RDS helps you optimize your Amazon RDS (Relational Database Service) costs by automatically managing your Reserved Instance (RI) commitments. It analyzes your RDS usage patterns and recommends the most cost-effective combination of Reserved Instances.

## Key features <a href="#key-features" id="key-features"></a>

* **Automated RI Management**: Automatically purchases and exchanges RDS Reserved Instances based on your usage patterns
* **Multi-Account Support**: Manages RDS commitments across all your AWS accounts from a single master account
* **Smart Coverage Optimization**: Intelligently determines the optimal coverage percentage for your RDS instances
* **Flexible Instance Family Support**: Supports various RDS instance families and database engines

## Prerequisites <a href="#prerequisites" id="prerequisites"></a>

Before setting up Commitment Orchestrator for RDS, ensure you have:

1. A Harness account with CACM module enabled
2. AWS master account with appropriate permissions

## Steps to configure <a href="#steps-to-configure" id="steps-to-configure"></a>

### Permissions for visibility <a href="#permissions-for-visibility" id="permissions-for-visibility"></a>

```text
"ec2:DescribeReservedInstancesOfferings",
"ce:GetSavingsPlansUtilization",
"ce:GetReservationUtilization",
"ec2:DescribeInstanceTypeOfferings",
"ce:GetDimensionValues",
"ce:GetSavingsPlansUtilizationDetails",
"ec2:DescribeReservedInstances",
"ce:GetReservationCoverage",
"ce:GetSavingsPlansCoverage",
"savingsplans:DescribeSavingsPlans",
"organizations:DescribeOrganization"
"ce:GetCostAndUsage"
```

### Permissions for orchestration <a href="#permissions-for-orchestration" id="permissions-for-orchestration"></a>

```text
"ec2:PurchaseReservedInstancesOffering",
"ec2:GetReservedInstancesExchangeQuote",
"ec2:DescribeInstanceTypeOfferings",              
"ec2:AcceptReservedInstancesExchangeQuote",              
"ec2:DescribeReservedInstancesModifications",   
"ec2:ModifyReservedInstances",
"ce:GetCostAndUsage",
"savingsplans:DescribeSavingsPlansOfferings",
"savingsplans:CreateSavingsPlan"
```

### Permissions for RDS <a href="#permissions-for-rds" id="permissions-for-rds"></a>

```text
"rds:PurchaseReservedDBInstancesOffering",
"rds:DescribeReservedDBInstancesOfferings",
"pricing:GetProducts"
```

{% @harness-feedback/feedback %}
