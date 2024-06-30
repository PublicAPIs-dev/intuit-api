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
- The Create/Read operations for Projects.
  - The Create operation for Invoice (sales transaction) with Projects. 

Currently, we have added support to a few transactions so that you can configure a project ID. 
These include a few transactions like - 

- Supported sales Transactions for your customers -
    - Estimate
    - Project Cost Estimate
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

- [Read](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#read-project) - Query (POST)                         
- [Create](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#create-project) - Mutation (POST)                    
- [Update](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#update-project) - Mutation (POST)                    
- [Delete](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#delete-project) - Mutation (POST)   


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


### Use Case 1: Create Project

##### Step 1: Use V3 Accounting Rest API to query customer and get the customer->id value.
Refer to the [V3 Customer Query](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/customer#query-a-customer)

##### Step 2: Use GraphQL API to create a Project for the above customer.
Refer to the [Create Project (Mutation)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#create-project)

### Use Case 2: Read Project

##### Read project by id / for a given date range using required filters
Refer to the [Read Project (Query)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#read-project)

### Use Case 3: Add transaction to a Project

##### Step 1: Create or read project
Refer to the sample [Create](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#create-project) / [Read](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#read-project) project:

##### Step 2: Create transactions using V3 accounting REST API and send ProjectRef with values from use-case 1 or 2 above.

Refer to the [V3 developer docs](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/invoice) how to reference Project to a transaction.
         
![](/intuit-api/assets/images/ProjectRef.png) 


Sample Invoice creation using "ProjectRef" (from minorVersion=69):

```
{
    "TxnDate": "2024-06-26",
    "CurrencyRef": {
        "value": "USD",
        "name": "United States Dollar"
    },
    "LinkedTxn": [],
    "Line": [
        {
            "Id": "1",
            "LineNum": 1,
            "Description": "Test1",
            "Amount": 50.99,
            "DetailType": "SalesItemLineDetail",
            "SalesItemLineDetail": {
                "ItemRef": {
                    "value": "2",
                    "name": "Hours"
                },
                "UnitPrice": 50.99,
                "Qty": 1,
                "ItemAccountRef": {
                    "value": "5",
                    "name": "Sales"
                },
                "TaxCodeRef": {
                    "value": "NON"
                }
            }
        },
        {
            "Amount": 50.99,
            "DetailType": "SubTotalLineDetail",
            "SubTotalLineDetail": {}
        }
    ],
    "Tag": [],
    "ProjectRef": {
        "value": "393363026"
    },
    "CustomerRef": {
        "value": "1",
        "name": "Test Customer"
    },
    "BillAddr": {
        "Id": "4",
        "Lat": "INVALID",
        "Long": "INVALID"
    },
    "FreeFormAddress": false,
    "SalesTermRef": {
        "value": "3",
        "name": "Net 30"
    },
    "TotalAmt": 50.99,
    "Balance": 50.99
}
```

Response:
```
{
    "Invoice": {
       
        "Id": "5",
        "SyncToken": "0",
        "MetaData": {
            "CreateTime": "2024-06-26T09:20:16-07:00",
            "LastModifiedByRef": {
                "value": "9341452110907680"
            },
            "LastUpdatedTime": "2024-06-26T09:20:16-07:00"
        },
        "TxnDate": "2024-06-26",
        "CurrencyRef": {
            "value": "USD",
            "name": "United States Dollar"
        },
        "LinkedTxn": [],
        "Line": [
            {
                "Id": "1",
                "LineNum": 1,
                "Description": "Test1",
                "Amount": 50.99,
                "DetailType": "SalesItemLineDetail",
                "SalesItemLineDetail": {
                    "ItemRef": {
                        "value": "2",
                        "name": "Hours"
                    },
                    "UnitPrice": 50.99,
                    "Qty": 1,
                    "ItemAccountRef": {
                        "value": "5",
                        "name": "Sales"
                    },
                    "TaxCodeRef": {
                        "value": "NON"
                    }
                }
            },
            {
                "Amount": 50.99,
                "DetailType": "SubTotalLineDetail",
                "SubTotalLineDetail": {}
            }
        ],
        "ProjectRef": {
            "value": "393363026"
        },
        "CustomerRef": {
            "value": "1",
            "name": "Test Customer"
        },
        "BillAddr": {
            "Id": "2"
        },
        "ShipAddr": {
            "Id": "2"
        },
        "FreeFormAddress": true,
        "ShipFromAddr": {
            "Id": "5",
            "Line1": "2600 Marine Way",
            "Line2": "Mountain view, CA  94043 US"
        },
        "SalesTermRef": {
            "value": "3",
            "name": "Net 30"
        },
        "DueDate": "2024-07-26",
        "TotalAmt": 50.99,
        "Balance": 50.99
    },
    "time": "2024-06-26T09:20:16.310-07:00"
}
```



