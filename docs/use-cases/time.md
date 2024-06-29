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


### Operations for Time Entry

- [Read](../../schema-entities/timeentry/#query-time-entry) - Query (POST)
- [Create](../../schema-entities/timeentry/#create-time-entry) - Mutation (POST)
- [Update](../../schema-entities/timeentry/#update-time-entry) - Mutation (POST)
- [Delete](../../schema-entities/timeentry/#delete-time-entry) - Mutation (POST)

### API schema for Time entity

- [Time API schema](../../schema-entities/timeentry/)

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
- Query the QuickBooks [Accounting V3 Preferences API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/preferences#query-preferences) and check for `Preferences->OtherPrefs->NameValue` to identify if projects are supported for the company. Check if NameValue `ProjectsEnabled` is true.

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

##### Use [Create Time Entry(Mutation)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/timeentry/#create-time-entry) GraphQL API to create time entry by 

- Fetching required `employee->id` from [V3 Accounting Employee Query](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/employee#query-an-employee)  & set it in `timeFor -> { id, entityType:"EMPLOYEE""}` time entry object. 
- Use the above `employee->id` to fetch required `compensation ->id` from [Read EmployeeCompensation(Query)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/employeeCompensation/#sample-query-body) GraphQL API & set it in `employeeCompensationId` in time entry object.
- Fetching required `project -> Id` from [Read Project(Query)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#read-project) GraphQL API to read `project -> id` & set it in `timeAgainst->{id, entityType:"PROJECT""}` time entry object.
- Fetching required `Item->id` from [V3 Accounting Query Item API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/item)& set it in `serviceItemId` time entry object

### Use Case 2: Create Time entry with paytype
This use case is applicable for the customers who are enrolled to **QuickBooks Payroll** and **do not** have the **Projects** enabled in QuickBooks Online.

##### Use [Create Time Entry(Mutation)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/timeentry/#create-time-entry) GraphQL API to create time entry by

- Fetching required `employee->id` from [V3 Accounting Employee Query](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/employee#query-an-employee)  & set it in `timeFor -> { id, entityType:"EMPLOYEE""}` time entry object. 
- Fetching required `compensation ->id` from [Read EmployeeCompensation(Query)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/employeeCompensation/#sample-query-body) GraphQL API & set it in `employeeCompensationId` in time entry object.


### Use Case 3: Create Time Entry and link to projects
This use case is applicable for the customers who are **not** enrolled to **QuickBooks Payroll** and have the **Projects enabled** in QuickBooks Online.

##### Use [Create Time Entry(Mutation)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/timeentry/#create-time-entry) GraphQL API to create time entry by 

- Fetching required `employee->id` from [V3 Accounting Employee Query](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/employee#query-an-employee)  & set it in `timeFor -> { id, entityType:"EMPLOYEE""}` time entry object.  
- Fetching required `Item->id` from [V3 Accounting Query Item API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/item)  & set it in `serviceItemId` time entry object. 
- Fetching required `project -> Id` from [Read Project(Query)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#read-project) GraphQL API to read `project -> id` & set it in `timeAgainst->{id, entityType:"PROJECT""}` time entry object.

### Use Case 4: Create Time Entry for employee

This use case is applicable for the customers who are **not** enrolled to **QuickBooks Payroll** and do **not** have the **Projects enabled** in QuickBooks Online.

#### Use [Create Time Entry(Mutation)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/timeentry/#create-time-entry) by 

- Fetching required `employee->id` from [V3 Employee Query](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/employee#query-an-employee) & set `timeFor -> { id, entityType:"EMPLOYEE""}` time entry object 
- Fetching required `Item->id` from [V3 Accounting Query Item API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/item)& set it in `serviceItemId` time entry object


### Use Case 5: Create Time Entry for contractor

This use case is applicable to track time for contractors and for the customers who do **not** have the **Projects enabled** in QuickBooks Online.

#### Use [Create Time Entry (Mutation)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/timeentry/#create-time-entry) by
- Fetching required `Vendor -> Id` from [V3 Accounting Vendor Query API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/vendor#query-a-vendor) & set it in `timeFor -> { id, entityType:"VENDOR""}` time entry object.
- Fetching required `Customer -> id` from [V3 Accounting Customer Query API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/customer#query-a-customer) (optional) & set it in `timeAgainst ->{ id, entityType:"CUSTOMER"}` time entry object.
- Fetching required `Item->id` from [V3 Accounting Query Item API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/item)  (optional) to set `serviceItemId` time entry object.

### Use Case 6: Create Time Entry for contractor and link it to projects
This use case is applicable to track time for **contractors** and for customers who have the **Projects enabled** in QuickBooks Online.

#### Use [Create Time Entry (Mutation)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/timeentry/#create-time-entry) by

- Fetching required `Vendor -> Id` from [V3 Accounting Vendor Query API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/vendor#query-a-vendor) & set it in `timeFor -> { id, entityType:"VENDOR""}` time entry object.
- Fetching required `project -> Id` from [Read Project(Query)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#read-project) GraphQL API to read `project -> id` & set it in `timeAgainst->{id, entityType:"PROJECT""}` time entry object.


### Appendix

Reference of (Contractor) -  Vendor with `Vendor1099` true:
![](/intuit-api/assets/images/Vendor1099.png)
