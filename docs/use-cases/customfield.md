---
layout: default
title: Custom Fields
nav_order: 18
parent: Use Cases
---

## Custom Fields

The APIs related to the Custom Fields allow you to manage and sync custom fields into QuickBooks Online.
The Custom Fields API provides support for create, read, update, and disable operations.
You can also add custom fields to transactions and other entities by configuring the custom field definition ID while creating the transaction.

This page outlines - 
- The Create/Read/Update operations for Custom Fields.
- The Create operation for Invoice (sales transaction) with Custom Fields. 
- The Create operation for Customer with Custom Fields.

Currently, custom fields are supported for the following transactions and entities.

- Supported Transactions -
    - Estimate
    - Invoice
    - Sales Receipt
    - Refund Receipt
    - Credit Memo
    - Bill
    - Expense
    - Purchase Order
   
- Supported entities -     
    - Customer
    - Vendor

  

### Integration Diagram

![](/intuit-api/assets/images/CustomField.png)


### Operations for Custom Fields entity

- Read - Query (POST)
- Create - Mutation (POST)
- Update - Mutation (POST)
- Disable - Mutation (POST)


### Scopes

-   app-foundations.custom-field-definitions.read: Allows access to read Custom field definitions data
-   app-foundations.custom-field-definitions.write: Allows access to read and write Custom field definitions data
-   com.intuit.quickbooks.accounting: For V3 Accounting REST API access


### Endpoints

-   GraphQL API:  https://qb.api.intuit.com/graphql 
-   V3 Accounting REST API: https://quickbooks.api.intuit.com/v3/company/{{realmid}}/entityname/ 


### Required headers

-   Content-type: **application/json**
-   Authorization: **Bearer access_token**
Note: Use tokens generated using scopes mentioned above in the authorization header for all other calls shown here.
 
### Use Cases




#### Use Case 1: Read Custom Fields Definitions
The output would list the custom fields that’s being set up in the company along with data types and the entities it’s for.


```

```


#### Use Case 2: Create Custom Fields Definitions


```

```

#### Use Case 3: Update Custom Fields Definitions


```

```


#### Use Case 4: Create transactions with custom fields


##### Step 1: Create or read custom field definition.
```

```

##### Step 2: Create invoice with custom fields

```

```

#### Use Case 5: Create entity with custom fields


##### Step 1: Create or read custom field definition.
```

```

##### Step 2: Create customer with custom fields

```

```

