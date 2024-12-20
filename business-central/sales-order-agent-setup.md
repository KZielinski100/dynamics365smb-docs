---
title: Set up Sales Order Agent
description: Learn how to activate the Sales Order Agent and manage user access.
ms.date: 11/27/2024
ms.topic: how-to
author: dmc-dk
ms.author: dmitrych
ms.reviewer: jswymer
ms.collection:
  - bap-ai-copilot
---
# Set up Sales Order Agent (preview)

[!INCLUDE [preview-banner](~/../shared-content/shared/preview-includes/preview-banner.md)]

The Sales Order Agent in Business Central automates processing sales orders from customer email requests. This article explains how to set up, activate, configure the Sales Order Agent, and manage user access.

Learn more about the agent in [Sales Order Agent overview](sales-order-agent.md).

[!INCLUDE [limited-public-preview](includes/limited-public-preview.md)]
<!--[!INCLUDE [preview-note](~/../shared-content/shared/preview-includes/production-ready-preview-dynamics365.md)]-->

## Prerequisites

- Turn on the Sales Order Agent capability

   When the Sales Order Agent app is installed on the **Extension Management** page, the capability is turned on by default. If the ![Shows the Sales Order Agent icon](media/soa-icon.png) **Sales Order Agent** icon appears in the navigation menu at the top, the agent capability is on but isn't active yet. It must be configured and activated to process customer requests for sales orders.

   You turn the agent capability on or off from the **Copilot & AI Capabilities** page, like other Copilot features in Business Central. The Sales Order Agent is listed under **Production ready previews**. Learn more in [Configure Copilot and AI capabilities](enable-ai.md).

- Set up the email account for sales receiving incoming requests for quotes and orders.

   The Sales Order Agent monitors incoming emails to this mailbox. The email account can be either a Microsoft 365 personal account or a shared mailbox in your organization. Learn more at [Set up e-mail](admin-how-setup-email.md).

   > [!IMPORTANT]
   > If you use a shared Microsoft 365 mailbox, set up mailbox delegation in the Exchange admin center to provide permissions to read and send emails from this mailbox to the users who can activate the agent. The same applies when you use a personal Microsoft 365 account. Provide other users with permission to read and manage emails unless you are the one activating the agent.
   >
   > When a user activates the agent, it runs as a background task in the context of that user and needs access to the shared mailbox to process emails. It might take a few hours for Exchange to propagate the permissions to the selected users.

## Activate and configure Sales Order Agent

1. In the navigation bar at the upper right of the role center, select ![Shows the Sales Order Agent icon](media/soa-icon.png) **Sales Order Agent** > **Activate**.

1. On the **Configure the Copilot agent** page, turn on the **Monitor incoming information** toggle, select the **Mailbox** check box, and then set **Mailbox** field to the email account you want the agent to monitor.

   ![Shows the Sales Order Agent configuration page](media/soa-configuration.png)

1. Select **Manage user access** to specify the users who can manage or interact with the agent. You can add more users now or later. Learn more in [Manage user access to the Sales Order Agent](#manage-agent-permissions-and-user-access).
1. Turn on the **Active** toggle.
1. On the right side of the page, select the **Go to next card** arrow to choose how the agent helps with inquiries, quotes, and orders.

    There are several options described in the following table:

    |Option|Description|Default|
    |-|-|-|
    |Select only available items|Specifies whether the agent considers item availability when searching for the customer's requested items.<br><br>When on, the agent checks inventory to determine whether the customer's requested item quantity is available based on their requested delivery date and location code. If there aren't enough items, the agent prepares the reply to the user that the requested items aren't available, even in situations when fewer items are available. <br><br>When off, the agent includes the customer's requested item quantity regardless of availability. That is, quotes can be created for the items which aren't available and their availability can become negative.<br><br>Learn more about item availability in [Get an availability overview](inventory-how-availability-overview.md) or open the [Item Availability page](https://businesscentral.dynamics.com?page=4410).|On|
    |Create sales documents|When on, the agent can create sales quotes and orders from email inquiries based on the remaining options in this table.<br><br>When off, the agent handles only email communication and doesn't create quotes or orders. The remaining options are irrelevant and can’t be set.|On|
    |Review quotes when created and updated|When on, the agent adds a review step for a Business Central user to review and confirm the sales quote before creating an outgoing email with the quote details and attachment. <br><br>When off, the agent creates or modifies sales quotes as requested and then automatically proceeds with creating an outgoing email with the quote as an attachment. The user must review and confirm the email before the agent sends it to the customer. |Off|
    |Make orders from quotes|When on, the agent converts confirmed sales quotes into orders after the customer agrees to the quote via email and the Business Central user confirms the email.<br><br>When off, you have to create the order manually.|On|
    |Review orders when created and updated|When on, the agent adds a review step for a Business Central user to review and confirm the sales order before creating an outgoing email with the order details and attachment. <br><br>When off, the agent creates the sales order as requested and then automatically proceeds with creating an outgoing email with the order as an attachment. The user must review and confirm the order before the agent sends it to the customer. |Off|

1. Select **Update** to complete the setup.

The **Sales Order Agent** icon changes to ![Shows the Sales Order Agent icon after configured](media/soa-activated-icon.png), indicating the agent is active and ready to handle incoming quote requests to the mailbox.

When the Sales Order Agent is active, a scheduled task added to job queue runs every 20 seconds on the mailbox. This task monitors new unread messages in the mailbox. If a new unread message is found, the Sales Order Agent imports the message into Business Central and verifies whether there's already a task for the mail thread. If a task for the thread already exists, the Sales Order Agent incorporates the new message into the existing task. Otherwise, it creates a new task for the message.

> [!NOTE]
> The ![Shows the Sales Order Agent icon when the agent is configure but not active](media/soa-not-activated-icon.png) icon indicates the agent is configured with mailbox, but it's not active. To activate it, select the icon, then select ![Shows the configuration icon for Sales Order Agent](media/soa-configure-icon.png) **Configure Sales Order Agent** to reopen the configuration page. From there, turn on the **Active** toggle.

## Manage agent permissions and user access

### Add agent users

As an administrator, you can specify which users have permission to use or configure the Sales Order Agent. There are two ways to add and configure agent users:

#### [From Configure Sales Order Agent](#tab/soaconfig)

1. Open the **Configure Sales Order Agent** page by selecting ![Shows the Sales Order Agent icon after configured](media/soa-activated-icon.png) **Sales Order Agent** > ![Shows the configuration icon for Sales Order Agent](media/soa-configure-icon.png) **Configure**.
1. Turn off the **Active** toggle.
1. Select **Manage user access**.
1. On the **Select users that can manage or interact with the Agent** page, you can do the following steps:

   - To add a user, select an empty line, select the **User Name** field, then select the user from the list.
   - To give a user permission to configure Sales Order Agent, select the **Can configure** check box. <br><br> The **Can configure** setting defines whether a user has access to update the agent configuration (for example, updating the designated mailbox, activating and deactivating the agent, and other settings) or only to work with the agent tasks (for example, reviewing and confirming agent steps).
   - To remove a user's access to the agent, select ![Shows the icon to show more option on a field](media/show-more-options-icon.png) **Show more options** next to the user name, and then select **Delete**.

#### [From Sales Order Agent card page](#tab/soapage)

1. To open the **Sales Order Agent** card page, search (<kbd>Alt</kbd>+<kbd>Q</kbd>) for  **Agents**, and then select **SALES ORDER AGENT - [COMPANY]**
1. Set **Status** to **Disabled** to deactivate the agent.
1. In the **User access** section, you can do the following steps:

   - To add a user, select an empty line, select the **User Name** field, then select the user from the list.
   - To give a user permission to configure Sales Order Agent, select the **Can configure** check box.
   - To remove a user's access to the agent, select ![Shows the icon to show more option on a field](media/show-more-options-icon.png) **Show more options** next to the user name, and then select **Delete**.

---

### Manage agent permissions to objects, data, and UI elements

The Sales Order Agent has a user account in Business Central, similar to other users. To access this account, search for and open the **Agents** page, and then select **SALES ORDER AGENT - [COMPANY]** to open the agent card page.

The **Agent Permission Sets** section lists all the permission sets currently assigned to the agent. By default, the Sales Order Agent has the **SOA AGENT – EDIT** permission set. This set restricts access to only the objects, data, and UI elements (such as pages, fields, and actions) necessary for handling sales quote requests.

You can't modify the **SOA AGENT – EDIT** permission set directly, because it's a system permissions set. However, you can create a copy of **SOA AGENT – EDIT** permission set, modify the copy to suit your needs, then add it to **Agent Permission Sets** section, along with any other permission sets.

Before you can add or delete permission sets applied to the agent, change the **State** to disabled. When you're done making changes, set it back to **Enabled**.

The following system permissions are available for controlling user access to the agent's functionality:

- **Configure All Agents** (ID 9665): Grants a user access to manage the configuration settings of the Sales Order Agent.
- **Manage Agent Tasks** (ID 9670): Allows a user to work with agent tasks displayed in the Copilot pane.

These system permissions are also included in the following permission sets, entitlements, and license types:

- The **SECURITY** permission set includes the **Configure All Agents** permission.
- The **System Execute - Basic** permission set includes the **Manage Agent Tasks** permission.
- The **System Tables - Basic** permission set includes all virtual tables used by the agent (labeled as "Agent *" tables).
- Essential and Premium license entitlements now include **Manage Agent Tasks** permissions.
- All license types include **Configure All Agents** permissions.

Users can configure the Sales Order Agent if they have the **Configure All Agents** permission or are listed as an agent user with the **Can Configure** field selected.

Users can work with agent tasks in the Copilot pane if they have the **Manage Agent Tasks** permission (either explicitly or as part of their Essential or Premium license permissions) and are listed as an agent user.

## Next steps

[Process sales quotes and orders with Sales Order Agent](sales-order-agent-process.md)

## Related information

[Sales order agent overview](sales-order-agent.md)  
[FAQ for Sales Order Agent](faqs-sales-order-taker-agent.md)
[Configure Copilot and AI capabilities](enable-ai.md)