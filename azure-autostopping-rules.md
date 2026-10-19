---
description: "Create and manage AutoStopping rules for Azure VMs and scale sets to stop idle resources on a schedule or when idle, and restart VMs when traffic is detected using Azure App Gateway or AutoStopping Proxy"
hidden: true
---

# Azure

{% @harness-package-selector/package-selector platforms="%5B%7B%22label%22%3A%22Kubernetes%22%2C%22slug%22%3A%22kubernetes%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-optimization%2Fautostopping-rules%2Fautostopping-rules%2Fkubernetes-autostopping-rules%22%2C%22logo%22%3A%22kubernetes-logo.svg%22%7D%2C%7B%22label%22%3A%22AWS%22%2C%22slug%22%3A%22aws%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-optimization%2Fautostopping-rules%2Fautostopping-rules%2Faws-autostopping-rules%22%2C%22logo%22%3A%22aws-logo.svg%22%7D%2C%7B%22label%22%3A%22GCP%22%2C%22slug%22%3A%22gcp%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-optimization%2Fautostopping-rules%2Fautostopping-rules%2Fgcp-autostopping-rules%22%2C%22logo%22%3A%22gcp-logo.svg%22%7D%2C%7B%22label%22%3A%22Azure%22%2C%22slug%22%3A%22azure%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-optimization%2Fautostopping-rules%2Fautostopping-rules%2Fazure-autostopping-rules%22%2C%22logo%22%3A%22azure-logo.svg%22%7D%5D" selectedPlatform="azure"  iconLibrary="https://developer.harness.io/provider-logos" %}

## Supported resources and traffic management

The following table shows the resources that AutoStopping supports and the traffic management you can use for each resource type.

| Azure Resource | AutoStopping Proxy | Azure App Gateway (Load Balancer) |
| -------------- | ------------------ | --------------------------------- |
| Azure VM       | ✅                  | ✅                                 |
| Azure Scale Set | ❌                  | ❌                                 |

{% hint style="info" %}
Azure scale sets do not support the AutoStopping proxy or Azure App Gateway. A scale set rule starts and stops the instances on a fixed schedule, or when another rule that depends on it warms up. Go to [Azure scale set behavior](#azure-scale-set-behavior) for details.
{% endhint %}

## Prerequisite: Set up proxy and/or load balancer

This prerequisite applies to Azure VMs only. Set up a proxy or load balancer that will intercept and manage traffic to your resources. This component is what enables the seamless start/stop functionality.

{% columns %}
{% column %}
{% hint style="info" %}
**Azure App Gateway**

Azure Application Gateway is a web traffic load balancer that enables you to manage traffic to your web applications. Application Gateway can make routing decisions based on additional attributes of an HTTP request, for example URI path or host headers.
{% endhint %}
{% endcolumn %}

{% column %}
{% hint style="success" %}
**AutoStopping Proxy**

This proxy VM sits in front of your virtual machines and intelligently starts or stops them based on incoming traffic. It supports both HTTP(S) and TCP connections. Built on the proven, open-source Envoy Proxy, the AutoStopping Proxy is capable of managing traffic for multiple AutoStopping-managed VMs from a single instance.
{% endhint %}
{% endcolumn %}
{% endcolumns %}

{% tabs %}
{% tab title="Setup Proxy" %}
1. In the AutoStopping overview page, click **Load Balancers** in the top right. Click **Create new Load Balancer** and then select **Create AutoStopping Proxy**.
2. Enter a name and select **Azure** in **Cloud Provider**
3. Choose a cloud connector or create a [new one](../../../new-to-cacm/quickstart.md).
4. Enter **Application Gateway Configuration**.
   * Region: The region where your target VM or the cloud resource is hosted.
   * Resource Group: A Resource Group (RG) in Azure is a logical container that holds related resources for a solution. In the context of an Azure Application Gateway, the resource group serves as the container for the various resources associated with the Application Gateway, such as: Virtual Machines (VMs), Storage Accounts, Networking Resources, etc.
   * Virtual Network: Azure Virtual Network is a service that provides the fundamental building block for your private network in Azure. VNet allows you to create and manage virtual private networks (VPNs) in the Azure cloud.
   * Subnet: AppGateway subnet should only contain AppGateway, no other resources can be placed in this subnet.
   * Security Group to define the security rules that determine the inbound and outbound traffic.
   * **TLS Certificate Configuration**:
     * **TLS Certificate Secret Version**: Enter the value displayed in the **Secret Identifier** field on the Azure console.
     * **TLS Private Key Secret Version**: Create another secret for the private key and enter the value in this field.

{% hint style="info" %}
It is recommended to create the secret using the Azure CLI, and not using the Generate/Import option on the UI. This is to avoid some unwanted characters that get added to the certificate value. For more information, go to [Set and retrieve a secret from Azure Key Vault using Azure CLI](https://learn.microsoft.com/en-us/azure/key-vault/secrets/quick-create-cli).

<details>

<summary>Example:</summary>

```text
 key-vault % az keyvault secret set --vault-name "sandy-test" --name "MultilineSecret" --file "secretfile.crt"
{
  "attributes": {
    "created": "2022-11-23T10:00:45+00:00",
    "enabled": true,
    "expires": null,
    "notBefore": null,
    "recoveryLevel": "Recoverable+Purgeable",
    "updated": "2022-11-23T10:00:45+00:00"
  },
  "contentType": null,
  "id": "https://sandy-test.vault.azure.net/secrets/MultilineSecret/1ddef90227664720a8a4604782a15f38",
  "kid": null,
  "managed": null,
  "name": "MultilineSecret",
  "tags": {
    "file-encoding": "utf-8"
  },
  "value": "This is my\nmulti-line\nsecret\n"
}
sandeepbhat@Sandeep Bhat key-vault % az keyvault secret set --vault-name "sandy-test" --name "PrivateKeytest" --file "private-key.pem"
{
  "attributes": {
    "created": "2022-11-23T10:02:03+00:00",
    "enabled": true,
    "expires": null,
    "notBefore": null,
    "recoveryLevel": "Recoverable+Purgeable",
    "updated": "2022-11-23T10:02:03+00:00"
  },
  "contentType": null,
  "id": "https://sandy-test.vault.azure.net/secrets/PrivateKeytest/20e60b7dde6340d7b17e9d446abfc984",
  "kid": null,
  "managed": null,
  "name": "PrivateKeytest",
  "tags": {
    "file-encoding": "utf-8"
  },
  "value": "-----BEGIN PRIVATE KEY-----\\\\XXXXXXXXXXXXXXXXXXXXXXXXXXX\\\\n-----END PRIVATE KEY-----\\\n"

```

</details>
{% endhint %}

- **Machine type**: Select the type of VM that you want to set the AutoStopping rule for.
- **Key Pair**: Enter the SSH key pair.
  - This key can be used to access the machine over SSH with the *ubuntu* user
- **API Key**: Enter the NG API key. Choose **No Expiration** in the Expiration dropdown list while creating this API key. For more information, go to [Create an API Key](https://developer.harness.io/harness-ai/use-harness-platform/automation/api/api-quickstart).
- **[OPTIONAL] Allocate Static IP**: Enable to assign an elastic IP address
  - Makes the proxy publicly accessible
  - Remember to update your DNS records to point to this IP

5\. Click on **Save AutoStopping Proxy**.
{% endtab %}

{% tab title="Setup Load Balancer" %}
1. In the AutoStopping overview page, click **Load Balancers** in the top right
2. Enter a name and select **Azure** in **Cloud Provider**
3. Choose a cloud connector or create a [new one](../../../new-to-cacm/quickstart.md).
4. Enter **Application Gateway Configuration**.
   * Region: The region where your target VM or the cloud resource is hosted.
   * Resource Group: A Resource Group (RG) in Azure is a logical container that holds related resources for a solution. In the context of an Azure Application Gateway, the resource group serves as the container for the various resources associated with the Application Gateway, such as: Virtual Machines (VMs), Storage Accounts, Networking Resources, etc.
   * Certificate (Optional)
   * Virtual Network: Azure Virtual Network is a service that provides the fundamental building block for your private network in Azure. VNet allows you to create and manage virtual private networks (VPNs) in the Azure cloud.
   * Subnet: AppGateway subnet should only contain AppGateway, no other resources can be placed in this subnet.
   * Frontend IP
   * SKU
   * Azure Function Region
5. Click on **Save Load Balancer**.

This newly created application gateway can be imported in any new Autostopping rule.

{% hint style="info" %}
Harness recently launched support for Azure WAF Gateways. Currently, only import functionality is available for this gateway type.
{% endhint %}
{% endtab %}
{% endtabs %}

***

## Permissions required

Harness starts and stops the instances of a scale set by calling the Azure API with the service principal of your Azure connector, so the connector needs additional permissions for scale sets.

To grant Harness only the permissions that scale set rules need, follow these steps when you create or edit the Azure connector:

1. In **Choose Requirements**, select **Optimization by AutoStopping**, and then turn on **Limit to specific permissions**.

   ![The Choose Requirements step of the Azure connector wizard, with Optimization by AutoStopping selected and Limit to specific permissions turned on](../../../.gitbook/assets/azure-connector-choose-requirements.png)
2. Click **Continue**.
3. In **AutoStopping Permissions**, select **Scale Sets**. **Schedules only** is the only option available for scale sets.

   ![The AutoStopping Permissions step of the Azure connector wizard, with Scale Sets selected and Schedules only as the single option for scale sets](../../../.gitbook/assets/azure-connector-autostopping-permissions.png)
4. Click **Continue**. **Create Service Principal** lists the commands that create the service principal and a custom role named **Harness Autostopping**. For scale sets, the role includes these permissions:
   * `Microsoft.Compute/virtualMachineScaleSets/read`: List scale sets.
   * `Microsoft.Compute/virtualMachineScaleSets/start/action`: Start scale set instances.
   * `Microsoft.Compute/virtualMachineScaleSets/deallocate/action`: Stop scale set instances.
   * `Microsoft.Compute/virtualMachineScaleSets/virtualMachines/read`: List scale set instances.

   ![The Create Service Principal step of the Azure connector wizard, showing the commands to run](../../../.gitbook/assets/azure-connector-create-service-principal.png)
5. Run the commands in Azure Cloud Shell or a bash terminal.

{% hint style="info" %}
The **with App Gateway** and **with AutoStopping Proxy** options apply to virtual machines only.
{% endhint %}

Go to [Azure permissions](../../../references/best-practices/azure/azure-permissions.md#autostopping) to review the permissions for each AutoStopping option.

## Create AutoStopping rule

* In Harness, go to **Cloud & AI Cost Management** > **AutoStopping Rules** and click **New AutoStopping Rule**.
* Select **Azure**, choose the Azure connector from the account drop-down list, and click **Next**.

### Rule configuration

The **1. Configuration** tab has three sections.

**Step 1 of 3: Define your AutoStopping rule**

* Under **AutoStopping Type**, select how the rule manages your resources. You can select both options.
    * **Schedule-based**: Resources start and stop at scheduled times, regardless of traffic.
    * **Traffic-based**: Resources stop when idle and restart on demand. Traffic-based rules add a **Setup Access** tab, which is described in the sections that follow.

    When you select both, schedules always take priority over traffic-based detection.

* In **Name your Rule**, enter a name for the rule.
* In **Idle Time (mins)**, enter how long the resources must be inactive before the rule stops them. The default is 15.
* Under **State Sync Trigger**, select **Follow account level setting** or **Override for this rule**.

**Step 2 of 3: Resources to be managed**

Select the type of resource the rule manages: **VM(s)**, **Azure Scale Sets**, or **Kubernetes Cluster**.

* To manage Azure VMs, select **VM(s)**, then:
    * Choose how the VMs are handled once idle: **Shut down** or **Hibernate**.
    * Select the VMs you want to onboard.
    * Choose whether the instances convert to spot or remain on-demand: **On-demand** or **Spot**.
* To manage an Azure scale set, select **Azure Scale Sets**, then:
    1. Click **+ Add a Scale Set**.
    2. In **Select Azure Scale Set**, select a resource group from **Select resource group**. The dialog lists the scale sets in that group with their name and ID, the number of instances, and the region. Use **Search** to filter the list, or click **Refresh** to reload it.
    3. Select a scale set, then click **Add Selected**.

    ![The Select Azure Scale Set dialog, listing the scale sets in the selected resource group with their instances and region](../../../.gitbook/assets/azure-scale-set-selection.png)

    A rule manages one scale set. To manage more than one scale set, create a separate rule for each. For how AutoStopping handles scale set instances, go to [Azure scale set behavior](#azure-scale-set-behavior).

**Step 3 of 3: Fixed schedules, alerts, and other advanced configuration (optional)**

Use this section to add fixed schedules and alerts, to set run options, and to add dependencies. Go to [Fixed schedule](#fixed-schedule), [Alerts](#alerts), [Hide progress page](#hide-progress-page), [Dry run](#dry-run), and [Add dependency](#add-dependency) for details.

Click **Next**. For a schedule-based rule, the **2. Review** tab opens. For a traffic-based rule, the **2. Setup Access** tab opens first, and **3. Review** follows. The Setup Access tab asks which workload types run on the target and is optional, so you can click **Skip for now**.

### Azure scale set behavior

AutoStopping starts and stops the instances in an Azure scale set. It does not scale the scale set, so AutoStopping never changes the number of instances that the scale set defines.

* When a schedule or the idle time stops the rule, AutoStopping stops (deallocates) the instances of the scale set.
* When a schedule starts the rule, AutoStopping starts the instances again.
* When another rule depends on the scale set rule, warming up that rule also starts the instances. Go to [Add dependency](#add-dependency) to link rules.

Azure scale sets do not support the AutoStopping proxy or Azure App Gateway, so traffic does not start the instances.

While the instances start, the rule is **Warming up**, and it becomes **Active** when all the instances run. While the instances stop, the rule is **Cooling down**, and Azure shows the scale set as deallocating until the instances are stopped.

On the **AutoStopping Rules** page, the **Resources** column lists the instances of the scale set and labels them as **Azure scale sets**. The **Resource details** column shows whether the rule is **Running** or **Stopped**. To find rules by state, click **Add filter**, select **Resource activity status**, and choose **Warming up**, **Active**, **Cooling down**, **Stopped**, or **Error**.

### Fixed schedule

Create fixed uptime or downtime schedules for the resources managed by this AutoStopping Rule. When a resource is configured to go up or down on a fixed schedule, it is unaffected by activity or idleness during that time period.

In certain scenarios, you would not want your resources to go down or up. For example, every Friday at 5 p.m. you want your `ABC` resource to go down. You can schedule downtime for your `ABC` resource. During this window, the resource is forced to go down regardless of the defined rule. You can choose to specify uptime for your resources in the same way.

{% hint style="info" %}
The fixed schedule takes precedence over the defined AutoStopping Rule.
{% endhint %}

{% hint style="info" %}
Harness executes scheduled rules using [Dkron](https://dkron.io/), an open-source workload automation service.
{% endhint %}

To create a fixed schedule for your rule, do the following:

1. In step 3 of **1. Configuration**, under **Fixed Schedules**, click **Add Fixed Schedule**.
2. In **New Fixed Schedule**, enter a **Name** for your schedule.
3. In **Type**, select the type for your schedule. You can schedule an **Uptime** or **Downtime** for your rule. As per your schedule, the resources go up or down.
4. Select the **Time Zone** from the drop-down list.
5.  In **Set schedule period**, use the date picker to set the start and end time for your schedule.

    1. In **Begins on**, select the start date and time for your schedule. You can select a date and specify the time.
    2. In **Ends on**, select the end date and time for your schedule. You can select a date and specify the time. Ensure that **Never ends** checkbox is unselected to set the end time.

    If you do not specify an end time, the schedule continues to run until you manually update the settings or remove the schedule.
6. Select the checkbox **Never ends** if you do not want to set end time for your schedule.
7. You can also set a recurring schedule for the rule. If you want to set a recurring schedule, in **Uptime/Downtime in the selected period**, in **Repeats**, select the repeat frequency.
   1. Select the days of the week on which your schedule repeats. You can choose any day between Sunday and Saturday.
   2. Select **Everyday**, to set the schedule for all seven days of the week.
   3. Set your repeat schedule's beginning and ending time. In the **Time** field, specify the start and end time for the fixed schedule.
   4. Select **All Day**, if you wish to set your schedule for the entire day. If you choose All Day for your schedule, you will not be able to choose a start and end time.
8. Click **Apply**.

### Alerts

Alerts notify you when something happens to the rule, such as a failed warm-up.

1. In step 3 of **1. Configuration**, under **Alerts**, click **Add Alert**.
2. In **Create Rule Status Alert**, enter a **Name**.
3. Under **Select events to receive notifications**, select one or more events: **Rule update**, **Rule delete**, **Rule warm up failures**, **Rule cool down failures**, **Rule warm up success**, or **Rule cool down success**.
4. Under **Set up notification channels**, turn on a channel:
   * **Slack**: enter a webhook URL in **Specify webhook url**. Click **Add** to enter more webhook URLs.
   * **e-mail**: enter comma-separated email addresses.
5. Click **Save**.

### Hide progress page

Under **Run Options** in step 3 of **1. Configuration**, turn on **Hide progress page** to disable the progress page that appears while the resources warm up. This option is useful when an automation system invokes the service, because it prevents the system from mistaking the progress page for the response of the onboarded service. When the progress page is hidden, the first response after a downtime is delayed until the service is running.

### Dry run

Under **Run Options** in step 3 of **1. Configuration**, turn on **Dry Run** to evaluate the rule without stopping your cloud resources.

### Add dependency

Set dependencies between two or more AutoStopping rules when you want one rule to make one or more rules active when it warms up. For example for an application server dependent on a database server, create two AutoStopping Rules managing both the servers. Add a dependency on the Rule managing the application server to be dependent on the Rule managing the database server.

1. In step 3 of **1. Configuration**, under **Dependencies**, click **+ Add dependency**.
2. In **rules**, select the rule from the drop-down list.
3. In **DELAY IN SECS**, enter the number of seconds that rule should wait after warming up the dependent rule. The default is 5. For example, you have Rule 1 dependent on Rule 2 and you have set 5 seconds delay. In that case, when the request is received to warm up Rule 1, then first Rule 2 (dependent rule) is warmed up, and then there is a delay of 5 seconds before warming up Rule 1.

### Setup access for traffic-based rules

The **2. Setup Access** tab appears only for traffic-based rules. Azure scale sets do not support it, so skip this section for a rule that manages a scale set.

Under **What type of workload/s are running in the target VM? (optional)**, select one or both cards. Each card opens a panel below it. If you do not need either, click **Skip for now**.

* **TCP Workload or SSH/RDP**: select this card when users reach the VM over TCP, SSH, or RDP. In **Setup TCP**:
  1. In **Specify AutoStopping Proxy**, select a proxy from the drop-down list. To add one, click **Create new AutoStopping Proxy**. AutoStopping Proxy is recommended for SSH/RDP-based rule setup.
  2. Under **Specify listening Ports**, turn on **SSH** or **RDP**, and then enter the **Source port no.** (optional) and the **Target port no.** (mandatory). If you leave the source port empty, Harness generates one when you save the rule.
  3. To expose other TCP ports, click **Add port**.
* **HTTP/HTTPS Workload**: select this card when users reach the VM through an HTTP or HTTPS URL. In **Setup HTTP/HTTPS**:
  1. In **Selected connector**, confirm the Azure connector to which your Application Gateway or load balancer belongs.
  2. In **Specify Application Gateway (or AutoStopping Proxy)**, select an Application Gateway or an AutoStopping Proxy. To add a proxy, click **Create new AutoStopping Proxy**.

Go to [Set up proxy and/or load balancer](#prerequisite-set-up-proxy-and-or-load-balancer) to create the proxy or Application Gateway first.

### Save the rule

Click **Save Rule** on the **Review** tab to create the rule. To change a setting before you save, click **EDIT** next to the section or click **Previous**.

Your AutoStopping rule is listed on the **AutoStopping Rules** page.

## Troubleshooting

<details>

<summary>The resource(s) you have selected are already associated with another Autostopping rule</summary>

When you click **Save Rule** on a rule for an Azure scale set, Harness shows the error `The resource(s) you have selected are already associated with another Autostopping rule with id <id>`. An Azure scale set can belong to only one AutoStopping rule, and the error names the rule that already manages it.

To resolve this issue, do one of the following:

* Select a different scale set for the new rule.
* Go to the **AutoStopping Rules** page, find the rule with the ID in the error, and edit that rule instead of creating a new one.
* Delete the existing rule if you no longer need it, and then save the new rule.

</details>

## Azure AutoStopping savings computation

Billing data from Azure's amortized billing export will be used to compute savings for Azure VM-based AutoStopping rules. Ensure the connector has amortized billing export enabled.

### Important points to remember:

* Savings numbers will become precise only after the savings numbers are finalized after the 15th of the next month (after the final settlement). Savings will be recomputed for the previous month on the 15th of the next month to ensure any updates to CUR/billing-export are considered in the final savings numbers for the month.
* GCP billing export configured in the billing connector needs to be "detailed".
* Azure billing export configured in the billing connector needs to be "amortized".
* For cluster-based AutoStopping rules, the corresponding billing-enabled connector of the CSP should be configured in Harness; otherwise, savings computation will be based on public pricing data.

{% @harness-feedback/feedback %}
