---
description: >-
  AutoStopping Rules make sure that your non-production resources run only when
  used, and never when idle. This topic describes how to use AutoStopping
  Dashboard.
---


# View AutoStopping Rules summary page

AutoStopping Rules make sure that your non-production resources run only when used, and never when idle. It also allows you to run your workloads on fully orchestrated spot instances without any worry of spot interruptions.

AutoStopping dashboard allows you to view a summary of all the AutoStopping rules you have created in a simple and intuitive interface. The following are the key features of the AutoStopping Rules dashboard:

* View total savings in your setup after AutoStopping rules are created
* View total spend in your setup after AutoStopping rules are created
* Number of instances managed using AutoStopping rules
* State of the instances where AutoStopping rules are applied
* Number of active AutoStopping rules in your setup

You can also perform the following actions from the AutoStopping dashboard view:

* Start the instances that are in a stopped state
* Edit or delete an AutoStopping rule
* Enable or disable an AutoStopping rule

{% hint style="info" %}
CACM now allows bulk processing of AutoStopping rules i.e. multiple rules can be selected at once to be disabled, enabled, and dry run.
{% endhint %}

![](../../.gitbook/assets/autostopping-dashboard-new.png)

#### Visual summary <a href="#visual-summary" id="visual-summary"></a>

{% embed url="https://youtu.be/CDSRjRC_vY4" %}

#### View the AutoStopping rules dashboard <a href="#view-the-autostopping-rules-dashboard" id="view-the-autostopping-rules-dashboard"></a>

The **AutoStopping Summary of Rules dashboard** displays data as a chart and table. You can view, understand, and analyze your usage and cost data using either of them. However, the table allows you to view granular details.

1.  In **AutoStopping Rules**, in **Summary of Rules**, click the instance for which you want to view the details.

    ![](../../.gitbook/assets/autostopping-dashboard-32.png)

    You can view the details of the AutoStopping rule that you have created.

![](../../.gitbook/assets/autostopping-dashboard-33.png)

2. In **Spend vs Saving**, view the spend and savings data of seven days for the selected rule. The savings are calculated as the following:

*   The actual and potential costs of the VM are used to calculate savings.

    * Your actual hourly cost multiplied by 24 equals your potential daily cost.
    * The actual cost is the amount paid to the cloud provider for the number of hours actually used. This information comes from the AutoStopping usage records.

    ```text
    Potential cost = Actual hourly cost * 24
    ```

    With potential and actual cost, the savings are calculated.

    ```text
    Savings = Potential cost - Actual cost
    ```

    ![](../../.gitbook/assets/autostopping-dashboard-34.png)

3.  In **Logs and Usage Time**, view usage details and logs for the selected rule.

    1. **Usage Time**: This shows the details of the time at which the rule started and ended for seven days.

    ![](../../.gitbook/assets/autostopping-dashboard-35.png)

    2. **Logs**: This shows the different states of the rule with the timestamp. For example, active, warming up, and cooling down.

    ![](../../.gitbook/assets/autostopping-dashboard-36.png)
4. In **Details**, click the instance to go to the cloud provider page to modify any of the settings.

![](../../.gitbook/assets/autostopping-dashboard-37.png)

#### Start the instances from the dashboard <a href="#start-the-instances-from-the-dashboard" id="start-the-instances-from-the-dashboard"></a>

You can start an instance from the Summary of Rules Page or from the Details page of a selected rule.

**From the Summary of Rules page**

1.  In **AutoStopping Rules**, in **Summary of Rules**, click on the **Custom Domain** or **Hostname** of the selected instance.

    ![](../../.gitbook/assets/autostopping-dashboard-38.png)
2.  The instance starts to warm up. This takes about 30 seconds.

    ![](../../.gitbook/assets/autostopping-dashboard-39.png)
3.  Once the instance is up and running, the status of the instance changes from stopped to running in the dashboard.

    ![](../../.gitbook/assets/autostopping-dashboard-40.png)

**From the Details page**

1. In **AutoStopping Rules**, in **Summary of Rules**, click the instance that you want to start.
2.  In **Details**, click the **Hostname** or **Domain name** to warm up the instance.

    ![](../../.gitbook/assets/autostopping-dashboard-41.png)

#### Enable or disable an AutoStopping rule from the dashboard <a href="#enable-or-disable-an-autostopping-rule-from-the-dashboard" id="enable-or-disable-an-autostopping-rule-from-the-dashboard"></a>

You can enable or disable an AutoStopping rule from the Summary of Rules Page or from the details page of a selected rule.

**From the Summary of Rules page**

1. In **AutoStopping Rules**, in **Summary of Rules**, select the instance that you want to enable or disable.
2. Click the three-dot menu and click **Disable**.

![](../../.gitbook/assets/autostopping-dashboard-42.png)

3. Click **Disable**.

![](../../.gitbook/assets/autostopping-dashboard-43.png)

**From the Details page**

1. In **AutoStopping Rules**, in **Summary of Rules**, click the instance that you want to enable or disable.
2.  Toggle the button to disable or enable the rule.

    ![](../../.gitbook/assets/autostopping-rule-disable.png)

#### Edit an AutoStopping rule from the dashboard <a href="#edit-an-autostopping-rule-from-the-dashboard" id="edit-an-autostopping-rule-from-the-dashboard"></a>

You can edit an AutoStopping rule from the Summary of Rules Page or from the details page of a selected rule.

**From the Summary of Rules Page**

1. In **AutoStopping Rules**, in **Summary of Rules**, select the instance that you want to enable or disable.
2.  Click the three-dot menu and click **Edit**.

    ![](../../.gitbook/assets/autostopping-dashboard-42.png)
3. The AutoStopping Rules setting appears. Follow the steps in [Create AutoStopping Rules for AWS](../../cost-optimization/autostopping-rules/autostopping-rules/#aws) and [Create AutoStopping Rules for Azure](../../cost-optimization/autostopping-rules/autostopping-rules/#azure).

**From the Details Page**

1. In **AutoStopping Rules**, in **Summary of Rules**, click the instance that you want to edit.
2. Click the **Edit** button. The AutoStopping Rules setting appears. You can edit details from here. ![](../../.gitbook/assets/rule-summary-page.png)

#### Delete an AutoStopping rule from the dashboard <a href="#delete-an-autostopping-rule-from-the-dashboard" id="delete-an-autostopping-rule-from-the-dashboard"></a>

You can delete an AutoStopping rule from the Summary of Rules Page or from the details page of a selected rule.

**From the Summary of Rules page**

1. In **AutoStopping Rules**, in **Summary of Rules**, select the instance that you want to enable or disable.
2.  Click the three-dot menu and click **Delete**.

    ![](../../.gitbook/assets/autostopping-dashboard-42.png)
3. Click **Delete**.

**From the Details page**

1. In **AutoStopping Rules**, in **Summary of Rules**, click the instance that you want to delete.
2.  Click the **Delete** button.

    ![](../../.gitbook/assets/rule-summary-page.png)
3.  Click **Delete**.

    ![](../../.gitbook/assets/autostopping-dashboard-49.png)

#### Overlapping schedules <a href="#overlapping-schedules" id="overlapping-schedules"></a>

Harness AutoStopping Rules now support overlapping schedules, offering enhanced flexibility for resource management. Users can define multiple fixed schedules within a single AutoStopping rule, even if they overlap. The resulting schedule is determined based on a customizable priority order, which can be adjusted using a drag-and-drop interface.

Overlapping schedules are particularly useful for organizations with teams operating in different time zones or for scenarios where temporary overrides, such as maintenance windows, need to be added to an existing schedule. By prioritizing schedules, users can ensure that the most critical rules are applied at the right time without modifying or deleting existing configurations.

<figure><img src="../../.gitbook/assets/overlapping-schedules-one.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

<figure><img src="../../.gitbook/assets/overlapping-schedules-two.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

{% @harness-feedback/feedback %}
