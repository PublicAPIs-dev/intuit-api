---
layout: default
title: Time
nav_order: 16
parent: Use Cases
---

## Time

The APIs related to the Time entity allow you to track time for employees and contractors inclusive of PayItems (Paytypes).
The Time API provides support for create, read and update operations.
You can also link the Time entries to a project within QuickBooks by configuring the ID for the project while creating the time entries.

This page outlines - 
- The Create operation for tracking time for an employee with PayType
- The Create operation for tracking time for an employee with PayType and linking it to a Project
- The Create operation for tracking time for a contractor


### Integration Diagram

![](/intuit-api/assets/images/Time.png)


### Operations for Time entity

- Read - Query (POST)
- Create - Mutation (POST)
- Update - Mutation (POST)

### API schema for Time entity

- [Time API schema](../../schema-entities/time/)

### Scopes

-   payroll.compensation.read : Allows access to read Pay types (i.e. compensation) [Compensation data can only be queried for customers using QuickBooks Payroll]
-   project-management.project : Allows access to read and write projects data
-   time-tracking.time-entry : Allows access to read and write timesheet data
-   time-tracking.time-entry.read : Allows access to read timesheet data
-   com.intuit.quickbooks.accounting: For V3 Accounting REST API access [For Read only use cases]


### Endpoints

-   GraphQL API:  https://qb.api.intuit.com/graphql 
-   V3 Accounting REST API: https://quickbooks.api.intuit.com/v3/company/{{realmid}}/entityname/ 


### Required headers

-   Content-type: **application/json**
-   Authorization: **Bearer access_token**

Note: Use tokens generated using scopes mentioned above in the authorization header for all other calls shown here.
 
### Use Cases

Pre-check: 
- Query the QuickBooks [Accounting V3 Preferences API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/preferences#query-preferences) and check for Preferences->OtherPrefs->NameValue to identify if projects are supported for the company. Check if NameValue `ProjectsEnabled` is true.

Sample name value pair response for reference:
```
 {
       "Name": "TimeTrackingFeatureEnabled",
       "Value": "true"
      }
      {
       "Name": "ProjectsEnabled",
       "Value": "true"
      }
```

### Use Case 1: Create Timesheet with paytype and link it to a Project
This use case is applicable for the customers who are enrolled to **QuickBooks Payroll** and have the **Projects enabled** in QuickBooks Online.

##### Step 1: Use V3 Accounting Rest API to query employee and get the employee->id value. 

Refer to the [V3 Query Employee](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/employee#query-an-employee) to get list of Employees

##### Step 2: Use GraphQL API to query compensation data.
Refer to the [Read Compensation API](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/employeeCompensation/#sample-query-body)
- Use the compensation Id from the response to record paytype.

##### Step 3: Use GraphQL API to read projects.
Refer to the [Read Project](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#read-project)

##### Step 4: Use GraphQL API to create time entry.

Pre-steps:
- Use [V3 Accounting Query Item API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/item) to get serviceItemId (optional) 
- Use the compensation Id from Step 2 to record paytype.
- Use the project id from step 3 to set ProjectRef 

With above all steps [create time entry](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/timeentry/#create-time-entry) 


### Use Case 2: Create Time entry with paytype
This usescase is applicable for the customers who are enrolled to **QuickBooks Payroll** and **do not** have the **Projects** enabled in QuickBooks Online.

##### Step 1: Use V3 Accounting Rest API to query employee and get the employee->id value. 
Refer to the [V3 Query Employee](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/employee#query-an-employee) to get list of Employees


##### Step 2: Use GraphQL API to query compensation data.
Refer to the [Read Compensation API](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/employeeCompensation/#sample-query-body)
- Use the compensation Id from the response to record paytype.

##### Step 3: Use GraphQL API to create timeentry

Pre-steps:
- Use [V3 Accounting Query Item API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/item) to get serviceItemId (optional) 
- Use the compensation Id from Step 2 to record paytype.


### Use Case 3: Create Time Entry and link to projects
This use case is applicable for the customers who are **no**t enrolled to **QuickBooks Payroll** and have the **Projects enabled** in QuickBooks Online.

##### Step 1: Use GraphQL API to read projects.
Refer to the [Read Project](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#read-project)

##### Step 2: Use GraphQL API to create timeentry.
Pre-steps:
- Use [V3 Query Employee](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/employee#query-an-employee) to employee Id
- Use [V3 Accounting Query Item API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/item) to get serviceItemId (optional)
- Use the project id from step 1 to set ProjectRef 

With above pre-steps as inputs [create time entry](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/timeentry/#create-time-entry) 

### Use Case 4: Create Timeentry for employee
This use case is applicable for the customers who are **not** enrolled to **QuickBooks Payroll** and do **not** have the **Projects enabled** in QuickBooks Online.

Pre-steps:
- Use [V3 Query Employee](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/employee#query-an-employee) to employee Id
- Use [V3 Accounting Query Item API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/item) to get serviceItemId (optional)

With above pre-steps as inputs [create time entry](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/timeentry/#create-time-entry) 


#### Use Case 5: Create Time Entry for contractor
This use case is applicable to track time for contractors and for the customers who do **not** have the **Projects enabled** in QuickBooks Online.

Pre-steps:
- Use [V3 Accounting Vendor Query API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/vendor#query-a-vendor) [filter by vendors by `Vendor1099=true` field ]
- Use [V3 Accounting Customer Query API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/customer#query-a-customer) (optional)
- Use [V3 Accounting Query Item API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/item) to get serviceItemId (optional)

Reference Vendor1099:
![](/intuit-api/assets/images/Vendor1099.png)

With above pre-steps as inputs [create time entry](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/timeentry/#create-time-entry)

### Use Case 5: Create Time Entry for contractor and link it to projects
This use case is applicable to track time for **contractors** and for customers who have the **Projects enabled** in QuickBooks Online.

##### Step 1: Use GraphQL API to read projects.
Refer to the [Read Project](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#read-project)

##### Step 2: Use GraphQL API to create time entry.
Pre-steps:
- Use [V3 Accounting Vendor Query API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/vendor#query-a-vendor) [filter by vendors by `Vendor1099=true` field ]
- Use the project id from step 1 to set ProjectRef 

With above pre-steps as inputs [create time entry](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/timeentry/#create-time-entry)

