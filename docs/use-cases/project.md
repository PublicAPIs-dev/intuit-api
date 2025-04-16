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
    - Invoice
    - Invoice payment
    - Sales Receipt
    - Refund Receipt
    - Credit Memo
    - Vendor Credit
    - Journal Entry
   
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

##### Sample request/response payloads:

###### 1. Invoice creation using "ProjectRef" (from minorVersion=69):

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

###### 2. Sample Vendor Credit creation with "ProjectRef" at line level:

```
{
  "TotalAmt": 90.0, 
  "TxnDate": "2025-1-23", 
  "Line": [
    {
      "DetailType": "AccountBasedExpenseLineDetail", 
      "Amount": 90.0, 
      "ProjectRef": {
        "value": "435514740"
      }, 
      "Id": "1", 
      "AccountBasedExpenseLineDetail": {
        "TaxCodeRef": {
          "value": "TAX"
        }, 
        "AccountRef": {
      
          "value": "2"
        }, 
        "BillableStatus": "Billable", 
        "CustomerRef": {
    
          "value": "1"
        }
      }
    }
  ], 
  "APAccountRef": {
    "name": "Accounts Payable (A/P)", 
    "value": "1150040000"
  }, 
  "VendorRef": {

    "value": "5"
  }
}
```
Response:
```
{
    "VendorCredit": {
        "VendorAddr": {
            "Id": "6"
        },
        "Balance": 90.00,
        "domain": "QBO",
        "sparse": false,
        "Id": "16",
        "SyncToken": "0",
        "MetaData": {
            "CreateTime": "2025-04-16T09:53:41-07:00",
            "LastUpdatedTime": "2025-04-16T09:53:41-07:00"
        },
        "TxnDate": "2025-04-16",
        "CurrencyRef": {
            "value": "USD",
            "name": "United States Dollar"
        },
        "LinkedTxn": [
            {
                "TxnId": "17",
                "TxnType": "ReimburseCharge"
            }
        ],
        "Line": [
            {
                "Id": "1",
                "LineNum": 1,
                "Amount": 90.00,
                "LinkedTxn": [
                    {
                        "TxnId": "17",
                        "TxnType": "ReimburseCharge"
                    }
                ],
                "DetailType": "AccountBasedExpenseLineDetail",
                "AccountBasedExpenseLineDetail": {
                    "CustomerRef": {
                        "value": "1",
                        "name": "Test Customer"
                    },
                    "AccountRef": {
                        "value": "2",
                        "name": "Uncategorized Expense"
                    },
                    "BillableStatus": "Billable",
                    "TaxCodeRef": {
                        "value": "TAX"
                    }
                }
            }
        ],
        "VendorRef": {
            "value": "5",
            "name": "Test Vendor"
        },
        "APAccountRef": {
            "value": "1150040000",
            "name": "AP"
        },
        "TotalAmt": 90.00
    },
    "time": "2025-04-16T09:53:40.894-07:00"
}
```

###### 3. Sample Journal Entry creation with ProjectRef at line Level:

```
{
  "Line": [ 
    {
      "JournalEntryLineDetail": {
        "PostingType": "Credit", 
        "AccountRef": {
          "name": "Uncategorized Asset", 
          "value": "4"
        }, 
      "Entity": {
            "Type": "Customer", 
            "EntityRef": {
              "value": "1"
        }
      }
      },
      "ProjectRef": {
          "value": "435515005"
        },
      "DetailType": "JournalEntryLineDetail", 
      "Amount": 100.0, 
      "Description": "nov portion of rider insurance"
    },
    {
      "JournalEntryLineDetail": {
        "PostingType": "Debit", 
        "AccountRef": {
          "name": "Uncategorized Expense", 
          "value": "3"
        }, 
        "Entity": {
            "Type": "Customer", 
            "EntityRef": {
              "value": "1"
        }
      }
    },
    "ProjectRef": {
          "value": "435515005"
        },
      "DetailType": "JournalEntryLineDetail", 
      "Amount": 200.0, 
      "Id": "0", 
      "Description": "nov portion of rider insurance"
    }, 
    {
      "JournalEntryLineDetail": {
        "PostingType": "Credit", 
        "AccountRef": {
          "name": "Uncategorized Asset", 
          "value": "4"
        }, 
      "Entity": {
            "Type": "Customer", 
            "EntityRef": {

              "value": "1"
        }
      }
      },
      "ProjectRef": {
          "value": "435515005"
        },
      "DetailType": "JournalEntryLineDetail", 
      "Amount": 100.0, 
      "Description": "nov portion of rider insurance"
    }
  ]
}
```

Response:
```
{
    "JournalEntry": {
        "Adjustment": false,
        "TotalAmt": 0,
        "domain": "QBO",
        "sparse": false,
        "Id": "18",
        "SyncToken": "0",
        "MetaData": {
            "CreateTime": "2025-04-16T10:36:10-07:00",
            "LastUpdatedTime": "2025-04-16T10:36:10-07:00"
        },
        "TxnDate": "2025-04-16",
        "CurrencyRef": {
            "value": "USD",
            "name": "United States Dollar"
        },
        "Line": [
            {
                "Id": "0",
                "Description": "nov portion of rider insurance",
                "Amount": 100.00,
                "DetailType": "JournalEntryLineDetail",
                "JournalEntryLineDetail": {
                    "PostingType": "Credit",
                    "AccountRef": {
                        "value": "4",
                        "name": "Billable Expense Income"
                    }
                },
                "ProjectRef": {
                    "value": "435515005"
                }
            },
            {
                "Id": "1",
                "Description": "nov portion of rider insurance",
                "Amount": 200.00,
                "DetailType": "JournalEntryLineDetail",
                "JournalEntryLineDetail": {
                    "PostingType": "Debit",
                    "AccountRef": {
                        "value": "3",
                        "name": "Uncategorized Asset"
                    }
                },
                "ProjectRef": {
                    "value": "435515005"
                }
            },
            {
                "Id": "2",
                "Description": "nov portion of rider insurance",
                "Amount": 100.00,
                "DetailType": "JournalEntryLineDetail",
                "JournalEntryLineDetail": {
                    "PostingType": "Credit",
                    "AccountRef": {
                        "value": "4",
                        "name": "Billable Expense Income"
                    }
                },
                "ProjectRef": {
                    "value": "435515005"
                }
            }
        ]
    },
    "time": "2025-04-16T10:36:09.960-07:00"
}
```

