---
title: Query Service Packaging
description: The following document outlines the packaging of capabilities and products available for Query Service and highlights the differences between ad hoc and batch queries.
exl-id: ba472d9e-afe6-423d-9abd-13ecea43f04f
---
# Query Service packaging

This document outlines the different types of packaging and query execution capabilities available in Query Service.

Adobe Experience Platform Query Service can be divided into two capabilities based on the query patterns that can be executed:

- **Ad hoc queries** are SQL queries used to explore ingested datasets for verification, validation, experimentation, and so on. These queries do not write data back into the Platform data lake.
- **Batch queries** are SQL queries used to perform the post-ingestion processing of ingested datasets. These queries clean, shape, manipulate, and enrich data, the results of which are written back to the Platform data lake. These queries can be scheduled, managed, and monitored as batch jobs.

Query Service capabilities are packaged with the following products and add-ons:  

- **Platform-based applications** (Adobe Real-Time Customer Data Platform, Adobe Customer Journey Analytics, and Adobe Journey Optimizer): Query Service access to execute ad hoc queries is provided from the outset with every variation and tier of Platform-based applications. 
- **[!DNL Data Distiller]** (add-on package that can be purchased with Adobe Real-Time CDP, Customer Journey Analytics and Adobe Journey Optimizer): Query Service access to execute batch queries is provided with [!DNL Data Distiller].

## Terminology {#terminology}

The following section provides definitions for key terms related to Query Service packaging:

- **Data lake storage**: The data lake primarily serves the following purposes:
    - Acts as the staging area for onboarding data into Experience Platform;
    - Acts as the long-term data storage for all Experience Platform data;
    - Enables use cases such as data analytics and data science.
- **Data export**: The [dataset](https://experienceleague.adobe.com/docs/experience-platform/catalog/datasets/overview.html) types you can export depending on your application, product tier, and any add-ons purchased. Derived datasets can be created through the Query Service Data Distiller add-on, and can be [exported from Adobe Experience Platform](https://experienceleague.adobe.com/docs/experience-platform/destinations/ui/activate/activation-overview.html) to a wide range of destinations including [cloud storage destinations](/help/destinations/ui/export-datasets.md).
- **Accelerated queries**: Accelerated queries return results based on aggregated data to reduce the wait time for the results and provide a more interactive exchange of information. Stateless queries made to the accelerated store are only available as part of the Data Distiller add-on.
- **Compute hours**: Compute hours are the metric used to track the scanning and writing of data with batch queries using the Query Service API. It is calculated in hours per year and is measured across all sandboxes. The number of Compute hours provided to your organization is defined in the deal scoping process.

## Entitlements {#entitlements}

The following table outlines the key Query Service entitlements based on how they are packaged:

| Query Service Entitlement | Packaged with Platform-based applications | Packaged with [!DNL Data Distiller] |
|---|---|---|
| Query Pattern Supported | Ad hoc queries only | Batch query |
| Use Case Supported | <ul><li>Exploration​</li><li>Data Discovery​</li><li>Data Validation</li><li>Experimentation</li></ul> | <ul><li>Cleaning</li><li>Shaping</li><li>Manipulating</li><li>Enriching</li></ul> |
| Semantics Supported | <ul><li>SELECT queries</li></ul> | <ul><li>CTAS and ITAS queries</li></ul> |
| Maximum Execution Time  | 10 minutes  | 24 hours |
| License Metric | **Query User Concurrency**: <ul><li>1 concurrent user (Real-Time CDP, Adobe Journey Optimizer)​</li><li>5 concurrent users (Customer Journey Analytics)​</li></ul> **Query Concurrency**: <ul><li>1 concurrent running query (all applications)​</li></ul> **Additional ad hoc query users pack add-on** can be purchased to increase customers' authorized ad hoc query entitlements. <ul><li>+5 additional concurrent users per pack</li><li>+1 additional concurrent running query per pack</li></ul> | **Compute Hours**: <ul><li>Variable (scoped based on customer's application entitlements)</li></ul> **Compute Hours** is a measure of the amount of time taken by the Query Service engine to read, process, and write data back into the data lake when a batch query is executed. |
| Accelerated query and reporting usage | No | Yes - Concurrent accelerated queries allow you to read data from the accelerated store and display within your dashboards. A dedicated entitlement for storing reporting models and datasets in the accelerated store is also provided.|
| Data lake storage capacity | Yes - Your total storage entitlement is dependant on your platform-based applications licenses. For example, Real-Time CDP, AJO, CJA, and so on. | Yes - An additional storage entitlement is provided to persist your raw and derived datasets for Data Distiller use cases beyond a seven-day data expiration date.<br>Your data lake storage capacity is measured in terabytes (TB) and depends on the quantity of Compute hours that you have bought. |
| Data export allowance | Yes - Your total export entitlement is dependant on your platform-based applications licenses. For example, Real-Time CDP, AJO, CJA, and so on. | Yes - Additional export entitlements are provided to allow for the export of derived datasets created using Data Distiller.<br>Your annual data export allowance is measured in terabytes (TB) and depends on the quantity of Compute hours that you have bought. |
| Query Execution Interface | <ul><li>Query Service UI</li><li>Third-party client UI</li><li>[!DNL PostgresSQL] client UI</li></ul> | <ul><li>Query Service UI </li><li>Third-party client UI</li><li>[!DNL PostgresSQL] client UI</li><li>REST APIs</li></ul> |
| Query Results Returned Via | Client UI | Derived dataset stored in data lake |
| Result Limit | <ul><li>Query Service UI - 100 rows</li><li>Third-party clients - 50,000</li><li>[!DNL PostgresSQL] client - 50,000</li></ul> | <ul><li>Query Service UI - The number of output rows can be [configured with a UI setting](./ui/user-guide.md#result-count) to between 50-500 rows.</li><li>Third-party clients  (no upper limit to rows)</li><li>[!DNL PostgresSQL] client  (no upper limit to rows)</li><li>REST APIs (no upper limit to rows)</li></ul> |
| Read Dataset Capacity | Yes | Yes |
| Write Dataset Capacity | No  | Yes |
| Scheduling Capacity | No  | Yes |
| Monitoring Capacity | Yes | Yes |
| Query Alert Setup Capacity | No | Yes |

{style="table-layout:auto"}

## Access control {#access-control}

Access control for Experience Platform is administered through the [Adobe Admin Console](https://adminconsole.adobe.com/) where product profiles link users with permissions and sandboxes. See the [access control overview](../access-control/home.md) for more information. 

In order to use Query Service, the [!DNL Manage Queries] permission must be enabled within Admin Console. This permission allows users to execute ad hoc and batch queries. Detailed instructions for requesting access to the product profile [!DNL Manage Queries] permission have been outlined in the [manage permissions for a product profile](../access-control/ui/permissions.md) and [manage users for a product profile](../access-control/ui/users.md) documents.

After purchasing the [!DNL Data Distiller] add-on, the [!DNL Write Dataset] permission must be granted. This permission allows [!DNL Data Distiller] users to execute batch queries.

The following table outlines the effects of the [!DNL Manage Queries] permission:

| Permission | Function |
|---|---|
| [!DNL Manage Queries] (without write data permission)| Provides access to execute ad hoc queries |
| [!DNL Manage Queries] (with write data permission) | Provides access to execute batch queries |

{style="table-layout:auto"}

## Sandbox support {#sandbox-support}

Sandboxes are virtual partitions within a single instance of Experience Platform. Each Platform instance supports multiple production and non-production sandboxes, each maintaining its own library of Platform resources. Non-production sandboxes allow you to test features, run experiments, and make custom configurations without impacting your production sandboxes. For more information on sandboxes, see the [sandboxes overview](../sandboxes/home.md). All Query Service entitlements are shared across all sandboxes.

## Next steps

By reading this document, you should have a better understanding of the different packaging types and query execution capabilities available in Query Service. To learn more about Query Service, such as well-known industry use cases, read the [use case documentation](./use-cases/abandoned-browse.md). For more general information, visit the [Query Service overview](./home.md).
