---
layout: default
title: Project
nav_order: 1
parent: Schema Entities
---

## Project

The APIs related to the Project entity allow you to manage and track projects. The Project API provides support for create, read, update, delete, and recover operations. You can also add transactions to your project by configuring the ID for the project while creating the transaction.

### Operations supported for Project                
                                              
- Read - Query (POST)                         
- Create - Mutation (POST)                    
- Update - Mutation (POST)                    
- Delete - Mutation (POST)                    

We have added support to below transactions so that you can configure a project ID. This will allow you to read back information about the project to which this transaction belongs to (if any). These include a few transactions like -
To link below transactions make use of the "ProjectRef" field in the V3 entity which is available with minorVersion=69)

- Estimate (enhanced version of V3 estimate with 3 new fields - Line.CostAmount, Line.HomeCostAmount, Line.SalesItemLine.SalesItemLineDetail.UnitCostPrice)
- Invoice
- Receive Payment 
- Bill
- TimeEntry
- Expenses 
- Purchase Order
- Credit Memo
- Refund Receipt
- Sales Receipt
                                                                                                                           
### ProjectRef                                                                                                                       
Developer docs for reference: https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/invoice          
![](/intuit-api/assets/images/ProjectRef.png)       
                                                                                 
### Endpoints

-   For production apps:  https://qb.api.intuit.com/graphql

### Project Fields

| Field Name        | Type                                       | Required | 	Description                                                                                                  |
|-------------------|--------------------------------------------|----------|---------------------------------------------------------------------------------------------------------------|
| name              | String!                                    | yes	     | The name of the project                                                                                       |
| description       | String                                     | No       | The description of the project                                                                                |
| assignee          | ProjectManagement_PersonaInput             | No       | The assignee of the project                                                                                   |
| type              | String                                     | No       | The type of the project that user can define to differentiate between different types of projects and filter on them |
| status	         | ProjectManagement_Status                   | No       | The status of the project indicating the current state of the project                                         |
| priority          | Int                                        | No       | The priority of the project in the range of 0-9 where 0 is the lowest and 9 is the highest priority and filter on them | 
| client	         | ProjectManagement_ClientInput              | No       | The client for whom the project is created                                                                    |
| customer          | ProjectManagement_CustomerInput            | Yes	     | The customer for whom the project is created                                                                  |
| account           | ProjectManagement_CompanyInput             | Yes	     | The company for whom the project is created                                                                   |
| dueDate           | DateTime                                   | No       | The due date of the project                                                                                   |
| startDate         | DateTime                                   | No       | The start date of the project                                                                                 |
| completedDate	 | DateTime                                   | No       | The completed date of the project                                                                             |
| completedBy       | ProjectManagement_UserInput                | No       | The user completed the project                                                                                |
| completionRate    | Decimal                                    | No       | The rate of completion of project                                                                             |
| pinned            | Boolean                                    | No       | Pinned is used to tell if a project should be shown at the top of the projects list above those that are not pinned |
| emailAddress      | [Qb_EmailAddressInput]                     | No       | The email address of the project                                                                              |
| addresses         | [Qb_PostalAddressInput]                    | No       | The addresses of the project                                                                                  |
| externalReferences| [ProjectManagement_ExternalReferenceInput] | No       | References to the project                                                                                     |

 
### Input Variables: 

### ProjectManagement_PersonaInput
| Input Name | Fields | Type | Field Definition      |
|------------|--------|------|-----------------------|
| id         | ID!    |      | The id of the persona |

### ProjectManagement_Status
```
enum ProjectManagement_Status{
    OPEN
    IN_PROGRESS
    BLOCKED
    CANCELED
    COMPLETE
    OTHER
}
```
### ProjectManagement_ClientInput
| Field | Type | Required | Description                                                                                     |    
|-------|------|----------|-------------------------------------------------------------------------------------------------|
| id    | ID!  |          | The realm/companyId of the client for whom the project is created(only for accountant use-case) |

### ProjectManagement_CustomerInput 

| Input Name | Fields | Type | Description                                     |
|------------|--------|------|-------------------------------------------------|
| id         | ID!    |      | The customer Id for whom the project is created |

### ProjectManagement_CompanyInput

| Field | Type | Required | Description                                    |
|-------|------|----------|------------------------------------------------|
| id    | ID!  |          | The companyId  for whom the project is created |

### ProjectManagement_UserInput

| Field     | Type | Required | Description                                    |
|-----------|------|----------|------------------------------------------------|
| id        | ID!  |          | The companyId  for whom the project is created |


### Qb_EmailAddressInput
<<TODO>> 

### Qb_PostalAddressInput
<<TODO>>


### Sample query header

-   Content-type: **application/json**
-   Use the project scope for the authorization header
 
**[project-management.project (read & write)** | **qb.project.write (backward compatible to support old scope)]** 

### Sample query body

Do an [introspection query](../../graphql-concepts/introspection) to see the current schema for the Bill entity.
Here's an example query using every possible field. Remember, with GraphQL you only need to query for the data you need:

Sample query (Query list of projects):

```
 query projectManagementProjects(
   $first: Int!,
   $after: String,
   $filter: ProjectManagement_ProjectFilter!,
   $orderBy: [ProjectManagement_OrderBy!]
 ) {
   projectManagementProjects(
     first: $first,
     after: $after
     filter: $filter,
     orderBy: $orderBy
   ) {
     edges {
       node {
         id,
         name,
         description,
         type,
         status,
         dueDate,
         startDate,
         completedDate,
         dueDate,
         assignee{
             id
         },
         priority,
         customer{
             id
         },
         account{
             id
         }
     addresses {
             streetAddressLine1,
             streetAddressLine2,
             streetAddressLine3
             state,
             postalCode
         }   
       }
     }
     pageInfo {
       hasNextPage
       hasPreviousPage
       startCursor
       endCursor
     }
   }
 }
```
Variables:

```
 {
   "first": 2,
   "after": null,
   "filter": {
     "startDate": {
       "between": {
         "minDate": "2024-03-30T18:47:25.123456789-07:00",
         "maxDate": "2024-05-30T18:47:25.123456789-07:00"
       }
     }
   },
   "orderBy": ["DUE_DATE_DESC"]
 }
```

Response:
<<TODO>>

## Filter support:

You can choose to **query by dateRange** by including dueDate or startDate in the filter.

```
 "dueDate": {
      "between": {
        "minDate": "2024-04-01T18:47:25.123456789-07:00",
        "maxDate": "2024-12-05T18:47:25.123456789-07:00"```
      }
    }
```

 ### Create mutation
             
```
mutation ProjectManagementCreateProject($name: String!, 
    $dueDate: DateTime!, 
    $startDate: DateTime,
    $account: ProjectManagement_CompanyInput!, 
    $assignee: ProjectManagement_PersonaInput
    $description: String,
    $type: String,
    $priority: Int,
    $completionRate: Decimal,
    $pinned: Boolean,
    $emailAddress: [Qb_EmailAddressInput],
    $addresses: [Qb_PostalAddressInput],
    $status: ProjectManagement_Status,
) {
  projectManagementCreateProject(input:{
    name: $name,
    dueDate : $dueDate,
    startDate : $startDate,
    account: $account,
    assignee: $assignee,
    description: $description,
    type: $type,
    priority: $priority,
    completionRate: $completionRate,
    pinned: $pinned,
    emailAddress: $emailAddress,
    addresses: $addresses,
    status: $status
  }
    )
    {
           ... on ProjectManagement_Project {
        name, 
        dueDate,
        id,
        account {
            id
           }
        emailAddress {
            email
        }
        }
    }
  }

```
 
Sample Variables: 
``` 
{
  "name": "TestJamDemoProject", 
  "description": "DemoProject", 
  "startDate:": "2024-06-01T00:00:00.000Z", 
  "dueDate": "2024-08-23T00:00:00.000Z", 
  "account": { 
    "id": "9341452164365778" 
    }, 
   "priority": 1, 
   "completionRate": 99.00, 
   "pinned": false, 
   "emailAddress": [ 
        { 
        "email":"jacobsolis123@gmail.com", 
        "name": "Jacob Solis", 
        "variation": { 
            "purpose": "BILLING", 
            "usage": "HOME", 
            "Common_Ordinal": "PRIMARY" 
        } 
        } 
    ], 
   "addresses": [ 
       { 
        "streetAddressLine1": "3000 17th street", 
        "postalCode": "94114", 
        "city": "San Francisco", 
        "state": "California", 
        "country": "USA", 
        "variation": { 
            "purpose": "BILLING", 
            "usage": "HOME", 
            "Common_Ordinal": "PRIMARY" 
        } 
       } 
   ] 
  }
```    

Sample response:

<<TODO>>
<<TODO>>

### Update mutation

### projectManagementUpdateName
```
 mutation projectManagementUpdateName($id: ID!, 
     $name: String!)
 {
     projectManagementUpdateName(input:{
         id: $id,
         name: $name,
     })
 
     {
     ... on ProjectManagement_Project {
             name, 
             id,
             description
         }
     }
 }
```

Variables:
For reference including all the fields here but it will update just the project name. We have 

```
{
  "id": 53115889,
  "name": "Test project updated",  // updating name
  "description": "A sample project",
  "startDate:": "2023-12-15T00:00:00.000Z",
  "dueDate": "2024-12-15T00:00:00.000Z",
  "realm": "9130357863496756",
  "status": "OPEN",
  "account": {
    "id": "9130357863496756"
    },
   "assignee": {
    "id": "9130357863496776"
    },
    "client":{
        "id":"djQuMTo5MTMwMzU3ODYzNDk2NzU2OjlkNjk5ZTk2MDg:27"
    },
   "type": "CONTACT",
   "priority": 1,
   "completionRate": 99.00,
   "pinned": false,
   "emailAddress": [
        {
        "email":"jacobsolis123@gmail.com",
        "name": "Jacob Solis",
        "variation": {
            "purpose": "BILLING",
            "usage": "HOME",
            "Common_Ordinal": "PRIMARY"
        }
        }
    ],
   "addresses": [
       {
        "streetAddressLine1": "3000 17th street",
        "postalCode": "94114",
        "city": "San Francisco",
        "state": "California",
        "country": "USA",
        "variation": {
            "purpose": "BILLING",
            "usage": "HOME",
            "Common_Ordinal": "PRIMARY"
        }
       }
   ]
  }
```

Response:
<<TODO>>

### projectManagementUpdateProject                                    
``` 
mutation projectManagementUpdateProject($id: ID!, $name: String!)
{
    projectManagementUpdateProject(input:{
        id: $id,
        name: $name,
    })

    {
    ... on ProjectManagement_Project {
            id,
            name
        }
    }
}
```

### projectManagementUpdateStatus
```
mutation projectManagementUpdateStatus($id: ID!, 
    $status: ProjectManagement_Status!)
{
    projectManagementUpdateStatus(input:{
        id: $id,
        status: $status,
    })

    {
    ... on ProjectManagement_Project {
            name, 
            id,
            status
        }
    }
}
```

### projectManagementUpdateDescription
```
mutation projectManagementUpdateDescription($id: ID!, 
    $description: String!)
{
    projectManagementUpdateDescription(input:{
        id: $id,
        description: $description,
    })

    {
    ... on ProjectManagement_Project {
            name, 
            id,
            description
        }
    }
}
```

###  projectManagementUpdateDueDate
 
```
mutation projectManagementUpdateDueDate($id: ID!, 
    $dueDate: DateTime!)
{
    projectManagementUpdateDueDate(input:{
        id: $id,
        dueDate: $dueDate,
    })

    {
    ... on ProjectManagement_Project {
            id,
            dueDate
        }
    }
}
```

###  projectManagementUpdateStartDate
```
mutation projectManagementUpdateStartDate($id: ID!, 
    $startDate: DateTime!)
{
    projectManagementUpdateStartDate(input:{
        id: $id,
        startDate: $startDate,
    })

    {
    ... on ProjectManagement_Project {
            id,
            startDate
        }
    }
}
```

###  projectManagementUpdateCustomer 
```
mutation projectManagementUpdateCustomer($id: ID!, 
    $customer: ProjectManagement_CustomerInput!)
{
    projectManagementUpdateCustomer(input:{
        id: $id,
        customer: $customer,
    })

    {
    ... on ProjectManagement_Project {
            id
        }
    }
}
``` 

### projectManagementUpdateCompletionRate

```
mutation projectManagementUpdateCompletionRate($id: ID!, 
    $completionRate: Decimal!)
{
    projectManagementUpdateCompletionRate(input:{
        id: $id,
        completionRate: $completionRate,
    })

    {
    ... on ProjectManagement_Project {
            id,
            completionRate
        }
    }
}
```

### projectManagementUpdateEmailAddress
 
```
mutation projectManagementUpdateEmailAddress($id: ID!, 
    $emailAddress: [Qb_EmailAddressInput]!
    )
{
    projectManagementUpdateEmailAddress(input:{
        id: $id,
        emailAddress: $emailAddress,
    })

    {
    ... on ProjectManagement_Project {
            id,
            completionRate
        }
    }
}
```

### projectManagementUpdateAddresses
``` 
mutation projectManagementUpdateAddresses($id: ID!, 
    $addresses: [Qb_PostalAddressInput]!
    )
{
    projectManagementUpdateAddresses(input:{
        id: $id,
        addresses: $addresses,
    })

    {
    ... on ProjectManagement_Project {
            id
        }
    }
}
```

                                                                                                                                              
### Delete Mutation                                                                                                                           
                                                                                                                                              
```                                                                                                                                           
mutation projectManagementDeleteProject($id: ID!)                                                                                             
{                                                                                                                                             
    projectManagementDeleteProject(input:{                                                                                                    
        id: $id,                                                                                                                              
    })                                                                                                                                        
                                                                                                                                              
    {                                                                                                                                         
    ... on ProjectManagement_Project {                                                                                                        
            id,                                                                                                                               
            deleted                                                                                                                           
        }                                                                                                                                     
    }                                                                                                                                         
                                                                                                                                              
```                                                                                                                                           
                                                                                                                                              
Variables:                                                                                                                                    
```                                                                                                                                           
{                                                                                                                                             
  "id": 53115889                                                                                                                              
}                                                                                                                                             
```                                                                                                                                           
                                                                                                                                              
Response:                                                                                                                                     
<<TODO>>                                                                                                                                      
                                                                                                                                              
                                                                                                                                              