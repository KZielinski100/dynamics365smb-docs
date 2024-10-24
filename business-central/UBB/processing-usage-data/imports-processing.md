---
title: Import data in usage based billing
description: You can import and process data in usage based billing.
author: brentholtorf
ms.author: bholtorf
ms.reviewer: bholtorf
ms.topic: conceptual
ms.search.keywords: 
ms.search.form: 
ms.date: 08/14/2024
ms.service: dynamics-365-business-central
ms.custom: bap-template
---

# Import data in usage based billing

This article explains how to import and process usage data, and discusses the different [methods for pricing](#methods-for-pricing) usage data.

## Import usage data

The first step is to create a new import for a supplier and import the usage data file from the [Usage Data Suppliers](../masterdata/suppliers.md) page. Use the **Usage Data Imports** action, and then the **New Import** action.

In addition to the data generated by the system, enter a description for each import in the **Description** field. The **Processing Step**, **Processing Status**, and **Reason (Preview)** (if there's an error) fields show the processing status.

After you process the usage data, you can bill the customer.

### Use the Usage Data Imports page

The following table describes the actions on the **Usage Data Imports** page.

|Action  |Description  |
|---------|---------|
|New Import    |  Prepare a new import.       |
|Process     |  * **Import file** Opens a file dialog to add the import file to the selected import.<br>* **Process Data** processes the imported usage data, but doesn't create invoices. The individual steps **Create Imported Lines**, **Process Imported Lines**, **Create Usage Data Billing**, and **Process Usage Data Billing** automatically run one after the other.       |
|Create Customer Invoices     | Create a sales invoice for all contract elements in the selected import. This action creates a separate invoice document for each customer contract. The contract invoices post directly if you choose **Post documents**.        |
|Create Vendor Invoices     |  Create a purchase invoice for all contract components in the selected import. This action creates a separate invoice document for each vendor contract. Because a vendor invoice number is required to post purchase invoices, the option to post the generated documents directly isn't available.       |
|Manual Processing     |  As an alternative to automatic processing of usage data by using the **Process Data** action, you can open the individual actions separately.       |
|Create Imported Lines     | Generate the usage data as raw data based on the import file. The data uses the [data exchange definitions](../masterdata/dataexchangedefinitions.md) assigned to the [usage data supplier](../masterdata/suppliers.md). To open the imported lines, use the lookup in the **No. of Imported Lines** field.        |
|Process Imported Lines     |  When you process the imported lines, [!INCLUDE [prod_short](../../includes/prod_short.md)] tries to create a link between the usage data and the service commitments or service objects. If errors occur during processing, you can use the **No. of Imported Line Errors** field to explore them. After you correct the errors, you can run the processing step again.       |
|Create Usage Data Billing     | Generate the data you need to bill customers based on the imported lines. This depends on your configuration.        |
|Process Usage Data Billing     | Determine pricing for the usage data. Pricing depends on the [methods for pricing](#methods-for-pricing) you use and the period. If errors occur during processing, you can use the lookup in the **No. of Usage Data Billing Errors** field to access the errors. After you correct the errors, you can run the processing step again.       |
|Remove Usage Data Lines & Billing     |  Delete the usage data that the import file generated, and then regenerate it. This is useful, for example, if errors occur during processing that you can resolve by using a different [data exchange definition](../masterdata/dataexchangedefinitions.md).       |
|Usage Data Billing     | * **Customer Contracts** opens an overview of the customer contracts in the import.<br>* **Customer Contract Invoices** opens an overview of the customer contract invoices created for the import, but aren't posted.<br>* **Posted Customer Contract Invoices** opens an overview of the customer contract invoices created and posted for the import.<br>* **Vendor Contracts** opens an overview of the vendor contracts in the import.<br>* **Vendor Contract Invoices** opens an overview of the vendor contract invoices created for the import, but aren't posted.<br>* **Posted Vendor Contract Invoices** opens an overview of the vendor contract invoices created and posted for the import.        |

> [!NOTE]
> **Imported Lines** <br/>
> The **Usage Data Generic Import** page displays the raw data generated from the import files. This usually includes supplier information about the customers, the subscriptions, and the billing period. The **Unit Cost** and **Cost Amount** fields let you import unit prices and total prices. If the total price is available in the **Cost Amount** field, the **Unit Cost** is calculated based on the quantity. If the total price isn't available, it will be calculated based on the quantity and the **Unit Cost**.
>
> To open the **Usage Data Generic Import** page, use the lookup in the **No. of Imported Lines** field. To learn more about the processing steps, go to [Process: Import usage data](#import-usage-data).

The **Usage Data Billing** page contains the data that serves as the basis for billing. Depending on how you set up the services commitments, customer usage data is generated for each vendor usage data. In addition, customer pricing and the updating of prices in the service commitments happens here in the last processing step. Customer pricing depends on the setting in the **Unit Price from Import** field on the [usage data suppliers](../masterdata/suppliers.md).

To open the **Usage Data Billing** page, use the lookup in the **No. of Usage Data Billing** field. To learn more about the processing steps, go to [Process: Process usage data](#process-usage-data).

### Import usage data via an API

In addition to importing usage data in a file, you can also use an API to import the data. When you open the page, a new import number is automatically assigned. This number is then used for each transferred data record and to assign the records to an import.

Authentication (Service-to-Service, S2S) must happen when you open the API page. To learn more about setting up authentication, go to [Service-to-Service Authentication](/dynamics365/business-central/dev-itpro/administration/automation-apis-using-s2s-authentication). In summary, you need to create an Entra ID application, enter it in [!INCLUDE [prod_short](../../includes/prod_short.md)], and assign appropriate access rights (just as you would for users). The article describes [Calling API and web services OAuth2Flows](/dynamics365/business-central/dev-itpro/administration/automation-apis-using-s2s-authentication#calling-api-and-web-services-oauth2flows), and offers a nice explanation of how to test authentication and the web service.

The following example shows the link to open the page externally: <br/>

```html
https://api.businesscentral.dynamics.com/v2.0/<tenant_id>/<environment_name>/api/singhammerITConsulting/dyce/v1.0/companies(<company_id>)/usageDataImports?$expand=usageDataGenericImports
```

* tenant_id is from the customer system
* environment_name is from the customer system
* company_id is from the customer system

To learn more, go to [API Page Type](/dynamics365/business-central/dev-itpro/developer/devenv-api-pagetype) and [Enabling APIs](/dynamics365/business-central/dev-itpro/api-reference/v2.0/enabling-apis-for-dynamics-nav).

> [!TIP]
> If you import usage data by using API, in the settings for the supplier, you should turn on the **Process without Usage Data Blobs** toggle. To learn more, go to [Settings on the supplier](../masterdata/suppliers.md#settings-on-the-supplier).

## Methods for pricing

The purchasing side is when **Vendor** is selected in the **Partner** field on the **Usage Data Billing** page. The prices that the supplier provided are used, because suppliers often also provide an invoice that matches the usage data. Because the vendor-side invoicing creates a vendor contract invoice, it must match the usage data and supplier invoice.

The customer price calculation, on the other hand, depends mainly on two things:

* The value in the **Usage Based Pricing** field of the [service commitments](../masterdata/service-commitments.md) for the usage data.
* The setting of the **Unit Price from Import** field at the [Usage data suppliers](../masterdata/suppliers.md).

If you turn on the **Unit Price from Import** toggle in the supplier settings, the import file must contain this data. In addition, the [data exchange definition](../masterdata/dataexchangedefinitions.md) must take this information into account. In this case, price calculation doesn't happen because the supplier of the usage data includes the sales prices, and this toggle overrides price calculation.

If you don't turn on the **Unit Price from Import** toggle, one of the following methods for customer pricing is used.

* Usage Quantity
* Fixed Quantity
* Unit Cost Surcharge
* Consumed Quantity

These methods are described in the following sections.

### Usage Quantity

Calculate the price based on the quantity contained in the usage data. The service commitments find the service object, and thus the related item. If needed, the appropriate price scale is determined through the quantity. The deciding factor here is the contract partner (in the **Sell-to Customer No.** field on the **Customer Contract** page), not the invoice recipient. Based on the period to be billed (which the **Charge Start Date** and **Charge End Date** fields define), the partial period is calculated to the day, with a corresponding daily price in the case of a pro-rata billing period.

### Fixed Quantity

When you process usage data, the original quantity remains fixed. The customer is always charged the original fixed quantity, and quantities aren't adjusted based on usage data. This pricing serves the flat-rate billing of service commitments, which, however, only happens if usage data is available.

### Unit Cost Surcharge

This pricing method disregards the imported usage quantity. The cost price plus the surcharge specified in the service commitments is charged to the customer. This option is often used for consumption-based billing. In this case, all individual prices of the usage data for a subscription are aggregated and the surcharge is calculated on the total.

### Consumed Quantity

This pricing method also disregards the quantity in the service object. In contrast to the **Usage Quantity** method, you can also process non-integer consumption quantities with decimal places. However, all usage data isn't aggregated in one service commitment, as with the **Unit Cost Surcharge** method. It's a combination of the two pricing methods. Similarly, the sales price per unit is also determined on a quantity-dependent basis using the related item (via the service commitment and the service object), and the contract partner (**Sell-to Customer No.** field) in the customer contract.

### Pricing for partial periods

In addition to these methods, partial periods are also taken into account when determining prices. The following are a few things to note about partial periods.

* Partial periods exist if the time between **Charge Start Date** + **Dateformula** from the **Billing Base Period** field of the service differs from the time span between **Charge Start Date** and (inclusive) **Charge End Date** fields. For example, if there are one or more quantity changes within a billing period, or if it's not a complete billing period.
* Because months have different numbers of days, the month in which the **Charge Start Date** on the **Usage Data Billing** page lies determines the daily price to use as the basis for calculating the partial period.
* Provided that it's a full billing period, it doesn't matter whether the billing period is congruent with a calendar month or overlaps.

Usage data describing a change in quantity within a billing period usually contains several data records, with the periods matching the corresponding quantity in each case.

When calculating prices for partial periods, [!INCLUDE [prod_short](../../includes/prod_short.md)] calculates with the daily price. The number of days in the **Billing Base Period** (from the service commitments) is determined, in which the **Charge Start Date** lies. The billing period price is then multiplied, taking into account the quantity scale, based on the ratio of the two time periods.

For a billing period of 1 month (the **Billing Base Period** field contains **1M**), the daily prices in a calendar month are always identical. Consequently, for two partial periods whose **Charge Start Date** is in two calendar months with different number of days, the daily price is also different.

> [!CAUTION]
> If the monthly price is the same, the daily price in a month with 31 days (for example, January) is lower than in a month with only 28 days (such as February). However, this only applies if it's a partial period. The price of full billing periods, however, is always identical.
>
> For a partial period that extends over several months with different numbers of days, the daily price of the respective month always applies. This ensures that for several partial periods within a month, the sum of the daily prices always corresponds to the monthly amount.

#### Examples of pricing

The following example shows partial periods within a calendar month.

|Charge Start Date|Charge End Date|Number of days|Unit price month|Quantity|Price partial period|
|:--|:--|:--|:--|:--|:--|
|01.05.2022|10.05.2022|10|35|2|22,58|
|11.05.2022|31.05.2022|21|35|5|118,55|

Total price for billing period: **141,13**

The following example shows overlapping partial periods.

|Charge Start Date|Charge End Date|Number of days|Unit price month|Quantity|Price partial period|
|:--|:--|:--|:--|:--|:--|
|11.01.2022|02.02.2022|23|35|5|138,55|
|03.02.2022|10.02.2022|8|35|8|80|

Total price for billing period: **218,55**

> [!NOTE]
> The amount for the first subperiod is made up of the daily rates for 21 days in January (21/31 * 35 * 5 = 118,55) and two days in February (2/28 * 35 * 8 = 20,00)*.

### Billing period from usage data

It's typical, but not mandatory, that the prices for usage-based service commitments are monthly. At the same time, there might be differences between the **Billing Base Period** from service commitments and the actual billed period from usage data. To take both into account, both periods are compared when calculating the prices. To learn more, go to [Process usage data](#process-usage-data). If they aren't identical, the ratio is used as a factor in price calculation.

<!--## How to import usage data

I can't complete these steps because I don't have a file. When I choose New Import, the supplier is not set automatically. It also seems to point to the API way of importing.

1. Select the ![Lightbulb that opens the Tell Me feature 3.](media/ui-search/search_small.png "Tell me what you want to do") icon, enter **Usage Data Imports**, and then select the related link.
2. To create a new import, choose the **New Import** action. The supplier is set automatically.
3. Choose the **Import file** action to open a file dialog with which the import file can be added to the selected import.
4. The file will be stored as a BLOB. The details can be viewed via the lookup in the **No. of Usage Data Blobs** field.
Then, the [usage data is processed](#process-usage-data).-->

## Process usage data

On the **Usage Data Imports** page, use the **Process Data** action to process the usage data. The process involves several steps.

1. After you import the reconciliation file, the raw data is generated as individual lines based on the import file via **Create Imported Lines**. You can access the details by using the lookup in the **No. of Imported Lines** field.
2. When you process the imported lines (**Process Imported Lines**), the imported usage data merges with the service commitments and thus the service objects. [Subscriptions](../masterdata/customers-subscriptions.md) are the links that establish the connection. If required, you can set up the supplier to create these records automatically. To learn more, go to [Settings on the supplier](../masterdata/suppliers.md#settings-on-the-supplier). Specifically, the usage data contains the ID of the subscription to which it belongs. Each subscription has a [usage data supplier reference](../masterdata/references.md) associated with the service commitments through the **Supplier Reference Entry No.** field. When usage data merges with service commitments, among other things, the validity of the service commitments is checked. If it isn't valid, an error shows. If you get an error, you can use the lookup in the **No. of Imported Line Errors** field to access the records.

If there isn't a connection between a subscription and a service commitment, you can create one using the [Linking subscription with service object](connect-subscription-service-object.md) feature, provided that the service commitment and the service object exist. If this isn't the case, you can create them using the [Extend Contract](extend-contract.md) action on the customer contract.

Usage data is typically supplier-side, which means it's probably missing the data you need for customer-side billing. You create the data for a customer partner for each vendor usage data record when you generate the **Usage Data Billing** (via **Create Usage Data Billing**) to use later for customer-side billing. You can use the lookup in the **No. of Usage Data Billing** field to access the details.

Records in usage based billing are vendor neutral, meaning all usage data is normalized regardless of the original source.

1. When you process usage data billing (via **Process Usage Data Billing**), the data in the respective vendor or customer Contract lines, in the service objects, and in the service commitments are updated based on the new usage data (quantities and prices). In addition, the sales price is calculated for each data record where the **Partner** field contains **Customer** (see [methods for pricing](#methods-for-pricing)). If you get an error, use the lookup in the **No. of Usage Data Billing Errors** field to access the details.

The next step is billing, which typically happens as follows:

* First, with the incoming invoice accompanying the usage data of the supplier.
* Then, with billing with customer contract invoices for the customers.

## Vendor and customer usage data

In addition to the import of vendor usage data, it might be that only customer usage data is available. This is usually the case when the service commitments to bill are internal services. Both the service commitments in the item master data and the service commitments for the service objects are configured so that there are only service commitments where **Customer** is chosen in the **Partner** field.

The connections between subscriptions and service commitments is always made through the **Supplier Reference** field, whereby a vendor service commitment is always prioritized. However, if there is only one customer service commitment, the connection can also be made.

When you process usage data, the assignment is made either via the vendor service commitments or, if a vendor service commitment isn't available, then through the customer service commitments.

> [!NOTE]
> If only a customer service commitment exists, only those where **Customer** is chosen in **Partner** field are created when processing the related usage data.

## See also

[Linking subscription with service object](connect-subscription-service-object.md)  
[Usage based billing customers and subscriptions](../masterdata/customers-subscriptions.md)  
[Extension of service commitments](../masterdata/service-commitments.md)  