---
layout: default
title: Project
nav_order: 16
parent: Use Cases
---

## Project

The APIs related to the Project entity allow you to manage and track projects.
The Project API provides support for create, read, update, and delete operations.
You can also add transactions to your project by configuring the ID for the project while creating the transaction.

This page outlines - 
- The Create/Read/Update/Delete/ operations for Projects.
- The CRUD operations for Invoice (sales transaction) with Projects. 
- The CRUD operations for Bill (purchase transaction) with Projects.

Currently, we have added support to a few transactions so that you can configure a project ID. 
These include a few transactions like - 

- Supported sales Transactions for your customers -
    - Estimate
    - Invoice
    - Invoice payment
    - Sales Receipt
    - Refund Receipt
    - Credit Memo
   
- Supported purchase transactions for your vendors -     
    - Bill
    - Expense
    - Purchase order
  

### Integration Diagram

![](/intuit-api/assets/images/Projects.png)


### Operations for Project entity

- Read - Query (POST)
- Create - Mutation (POST)
- Update - Mutation (POST)
- Delete - Mutation (POST)


### Scopes

-   project-management.project : Allows access to read and write projects data
-   com.intuit.quickbooks.accounting: For V3 Accounting REST API access


### Endpoints

-   GraphQL API:  https://qb.api.intuit.com/graphql 
-   V3 Accounting REST API: https://quickbooks.api.intuit.com/v3/company/{{realmid}}/entityname/ 


### Required headers

-   Content-type: **application/json**
-   Authorization: **Bearer access_token**
Note: Use tokens generated using scopes mentioned above in the authorization header for all other calls shown here.
 
### Use Cases

Pre-check: Query the QuickBooks company preferences by using V3 Accounting REST Preferences API and check for Preferences->OtherPrefs->NameValue> to identify if projects are supported for the company. Check if NameValue `ProjectsEnabled` is true.


#### Use Case 1: Create Project

##### Step 1: Use V3 Accounting Rest API to query customer and get the customer->id value.

```

```

##### Step 2: Use GraphQL API to create a Project for the above customer.
```

```


#### Use Case 2: Read Project

##### Read project by id.
```

```

##### Read all projects for a given date range.
```

```

#### Use Case 3: Add transaction to a Project

##### Step 1: Create or read project.
```

```

##### Step 2: Create transactions and add to projects

```

```



