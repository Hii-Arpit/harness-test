---
description: This topic describes how to add and manage access control for CACM Dashboards.
---


# Manage Access Control for CACM Dashboards

Harness provides Role-Based Access Control (RBAC) that enables you to control user and group access to Harness Resources according to their role assignment.

This topic describes how to add and manage access control for CACM Dashboards.

## Before you begin <a href="#before-you-begin" id="before-you-begin"></a>

* [RBAC in Harness](https://developer.harness.io/harness-ai/use-harness-platform/platform-access-control)

## CACM dashboards roles and permissions <a href="#cacm-dashboards-roles-and-permissions" id="cacm-dashboards-roles-and-permissions"></a>

The following roles are needed for CACM Dashboards:

* **Dashboard - Static Editor**: To add, edit, and delete CACM Dashboards
* **Dashboard - All View**: To view all the **By Harness** and **Custom** dashboards

| **Roles**                 | **Scope** | **Permissions**                                                                                  |
| ------------------------- | --------- | ------------------------------------------------------------------------------------------------ |
| Dashboard - Static Editor | Folder    | <ul><li>Add Dashboard</li><li>Add Tile</li><li>Edit Dashboard</li><li>Delete Dashboard</li></ul> |
| Dashboard - All View      | Folder    | View CACM Dashboards                                                                             |

## Add and manage dashboard - static editor role <a href="#add-and-manage-dashboard-static-editor-role" id="add-and-manage-dashboard-static-editor-role"></a>

Perform the following steps to add and manage permissions **Dashboard - Static Editor** role.

1. In **Harness**, click **Account Settings**, and then click **Access Control**.
2. Click **Roles**.
3. Click **New Role**. The New Role settings appear.
4.  In **Name**, enter **Dashboard - Static Editor** and click **Save**.

    ![](../../.gitbook/assets/manage-access-control-for-ccm-dashboards-00.png)
5. Click **Shared Resources** for the role that you created.
6.  Select the **View** and **Manage** checkbox. This allows you to add dashboards, add tiles, edit dashboards, and delete dashboards.

    ![](../../.gitbook/assets/manage-access-control-for-ccm-dashboards-01.png)
7. Click **Apply Changes**.

## Add and manage dashboard - all view role <a href="#add-and-manage-dashboard-all-view-role" id="add-and-manage-dashboard-all-view-role"></a>

Perform the following steps to add and manage permissions **Dashboard - All View** role.

1. In **Harness**, click **Account Settings**, and then click **Access Control**.
2. Click **Roles**.
3. Click **New Role**. The New Role settings appear.
4.  In **Name**, enter **Dashboard - All View** and click **Save**.

    ![](../../.gitbook/assets/manage-access-control-for-ccm-dashboards-02.png)
5. Click **Shared Resources** for the role that you created.
6.  Select the **View** checkbox. This will allow you to view all the dashboards.

    ![](../../.gitbook/assets/manage-access-control-for-ccm-dashboards-03.png)
7. Click **Apply Changes**.

## Add and manage access control for resource groups <a href="#add-and-manage-access-control-for-resource-groups" id="add-and-manage-access-control-for-resource-groups"></a>

Perform the following steps to limit access to specific Dashboards.

1.  In **Harness**, click **Account Settings**, and then click **Access Control**.

    ![](../../.gitbook/assets/manage-access-control-for-ccm-dashboards-04.png)
2.  In **Resource Groups**, click your Resource Group. For more information, go to [Manage Resource Groups](https://developer.harness.io/harness-ai/use-harness-platform/platform-access-control/manage-resource-groups).

    This section uses **Dashboard - All** as an example.
3. In **Shared Resources**, select **Dashboards**.

By default, **All Dashboards** is selected.

![](../../.gitbook/assets/manage-access-control-for-ccm-dashboards-05.png)

4. Click **Add Dashboards**.
5. In **Add Dashboards**, select the folders for which you want to limit the access.

The selected folder may have more than one dashboard. All the dashboards in the selected folders will have the same access.

![](../../.gitbook/assets/manage-access-control-for-ccm-dashboards-06.png) 6. Click **Apply Changes**.

## Add and manage access control for users <a href="#add-and-manage-access-control-for-users" id="add-and-manage-access-control-for-users"></a>

Perform the following steps to limit the access to specific Dashboards for different users.

1.  In **Harness**, click **Access Control**.

    ![](../../.gitbook/assets/manage-access-control-for-ccm-dashboards-04.png)
2. In **User**, in **New User**, select the User for which you want to add or modify the access control. For more information, go to [Manage users](https://developer.harness.io/harness-ai/use-harness-platform/platform-access-control/add-users).
3. In **Assign Roles**, select the **Role** from the drop-down list. You can select either **Dashboard - Static Editor** or **Dashboard - All View**.
4.  In **Resource Groups**, select the resource group for which you want to add or modiy the access control.

    ![](../../.gitbook/assets/manage-access-control-for-ccm-dashboards-08.png)
5. Click **Save**.

{% @harness-feedback/feedback %}
