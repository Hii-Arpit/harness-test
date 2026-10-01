---
description: >-
  This usecase provides the steps to configure an AutoStopping proxy as a
  downstream system of an ALB.
---


# Configure AutoStopping proxy as a downstream of ALB

Learn how to configure an AutoStopping proxy as a downstream system of an Application Load Balancer in AWS, as illustrated by this network architecture diagram:

![](../../.gitbook/assets/autostopping-alb-workflow-diagram.png)

### Create a target group for the proxy VM with a health check configuration <a href="#create-a-target-group-for-the-proxy-vm-with-a-health-check-configuration" id="create-a-target-group-for-the-proxy-vm-with-a-health-check-configuration"></a>

On the AWS console, you need to create a target group for the proxy VM with a health check configuration, and then edit ALB rules and add forwarding action to the proxy target group.

1. On the AWS console, navigate to **Target Groups** and [create a target group](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-target-group.html).
2. Choose the proxy VM and register it as a target.
3. Make sure the port matches the port details that the application is using.
4.  Configure the [health check settings](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/target-group-health-checks.html) as per the port information.

    ![](../../.gitbook/assets/configure-health-check-settings.png)
5.  If the proxy needs to handle multiple ports (80, 443), create one target group for each of the ports.

    ![](../../.gitbook/assets/create-target-group.png)
6.  Edit ALB rules and add forwarding action to the proxy target group.

    All the URLs configured in the AutoStopping rules should point to the AutoStopping proxy target group.

    ![](../../.gitbook/assets/edit-alb-rules.png)

### Create an AutoStopping rule with custom domains <a href="#create-an-autostopping-rule-with-custom-domains" id="create-an-autostopping-rule-with-custom-domains"></a>

In Harness, create AutoStopping rule for AWS with HTTP/HTTPS workload and configure custom domains in the AutoStopping rule.

![](../../.gitbook/assets/create-autostopping-rule-with-custom-domain.png)

{% @harness-feedback/feedback %}
