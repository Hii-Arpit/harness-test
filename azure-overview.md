---
description: "Explore the Azure Cost Dashboard in CACM to track total spend by region, meter category, service, and resource group"
hidden: true
---


# Azure

{% @harness-package-selector/package-selector platforms="%5B%7B%22label%22%3A%22Kubernetes%22%2C%22slug%22%3A%22kubernetes%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-reporting%2Fbi-dashboards%2Foverview%2Fkubernetes-overview%22%7D%2C%7B%22label%22%3A%22AWS%22%2C%22slug%22%3A%22aws%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-reporting%2Fbi-dashboards%2Foverview%2Faws-overview%22%7D%2C%7B%22label%22%3A%22GCP%22%2C%22slug%22%3A%22gcp%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-reporting%2Fbi-dashboards%2Foverview%2Fgcp-overview%22%7D%2C%7B%22label%22%3A%22Azure%22%2C%22slug%22%3A%22azure%22%2C%22path%22%3A%22cloud-cost-management%2Fcost-reporting%2Fbi-dashboards%2Foverview%2Fazure-overview%22%7D%5D" selectedPlatform="azure" %}

### View Azure Cost dashboard <a href="#view-azure-cost-dashboard" id="view-azure-cost-dashboard"></a>

<figure><img src="../../../.gitbook/assets/azure.png" alt=""><figcaption><p>Click to view full size image</p></figcaption></figure>

Perform the following steps to view Azure Cost Dashboard:

1. In the **Harness** application, click **Dashboards**.
2. Select **By Harness** and then, click **Azure Cost Dashboard**. The Azure Cost Dashboard is displayed.

| **Dimension**                                 | **Description**                                                                                                                          |
| --------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------- |
| Total Cost                                    | The total Azure cloud cost with cost trend.                                                                                              |
| Total Cost by Meter Category                  | The total cost of the meter. For example, Cloud services, Networking, and so on.                                                         |
| Total Cost by Region                          | The total cost of each Azure region you are currently running services in.                                                               |
| Total Cost by Month                           | The total Azure cloud cost by month.                                                                                                     |
| Most Expensive Service by Month               | The monthly cost of the most expensive Azure services, for example, Virtual Machines, Azure App Service, Azure DNS, etc.                 |
| Most Expensive Resource Types                 | The cost of the most expensive resource types.                                                                                           |
| Monthly Cost and Difference by Meter Category | The monthly meter cost and the differences in the cost from the previous month.                                                          |
| Monthly Cost by Instance ID                   | The monthly cost of the instance ID. Each VM in a scale set gets an instance ID that uniquely identifies it.                             |
| Monthly Cost by Service Name                  | The monthly cost of Azure services, for example, Virtual Machines, Azure App Service, Azure DNS, etc.                                    |
| Monthly Cost by Resource Group                | The monthly cost of the resource group. A resource group is a container that holds related resources that you want to manage as a group. |

3. Select **Time Range** to filter the data based on pre-defined time range filters. The available filters are:
   * Last 7 Days
   * Last 30 Days
   * Last 90 Days
   * Last 12 months
   * Last 24 months
4. Once you have selected the **Time Range**, click **Update**. The data is refreshed with the latest data from the database. By default, **Last 30 Days** is selected.
5. Hover on the chart to see the cost details.
6. In the **Cost by Meter Category** and **Total Cost by Region** click the up or down arrow button to scroll up or down the list. The list shows the percentage of each meter category with respect to the cost contribution.
7. You can further filter and customize your result in the Dashboard. Click on the individual dimension and field for which you want to see the data. For example, select **Virtual Machines** in **Azure Meter Category** and **US East** in the **Region**. The dashboard displays the data based on the set filter.
8. Click the Filter icon to hide or show the filters.

{% @harness-feedback/feedback %}
