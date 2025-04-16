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


### Operations for Time Activity

- [Query](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity#query-a-timeactivity-object)
- [Create](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity#create-a-timeactivity-object)
- [Update](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity#full-update-a-timeactivity-object)
- [Delete](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity#delete-a-timeactivity-object)

### API schema for Time Activity

- [Time Activity Object](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity#the-timeactivity-object)

### Scopes

-   payroll.compensation.read : Allows access to read Pay types (i.e. compensation) [Compensation data can only be queried for customers using QuickBooks Payroll]
-   project-management.project : Allows access to read and write projects data
-   com.intuit.quickbooks.accounting: For V3 Accounting REST API access [For Read only use cases]


### Endpoints

-   GraphQL API:  https://qb.api.intuit.com/graphql 
-   V3 Accounting REST API: https://quickbooks.api.intuit.com/v3/company/{realmid}/{entityname}?minorversion=70

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

### Use Case 1: Create Time Activity with pay type and link it to a Project

This use case is applicable for the customers who are enrolled to **QuickBooks Payroll** and have the **Projects enabled** in QuickBooks Online.

##### Use [V3 Create TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity#create-a-timeactivity-object) API to create time activity by 

- Fetching `employee->Id` by calling [V3 Accounting Employee Query](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/employee#query-an-employee)  & set it in `EmployeeRef` in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object with `NameOf` field as `Employee`.
- Filter by the `employee->Id` from above step to fetch the list of employee compensation Ids `compensation ->id` from [Read EmployeeCompensation(Query)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/employeeCompensation/#sample-query-body) GraphQL Query 
  & set it in `PayrollItemRef` in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object.
- Fetch `project -> id` from [Read Project(Query)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#read-project) GraphQL Query to read `project -> id` & set it in `ProjectRef` in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object.
- Fetch `Customer -> id` from [V3 Accounting Customer Query API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/customer#query-a-customer) (optional) & set it in `CustomerRef` in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object.
- Fetch  `Item->id` from [V3 Accounting Query Item API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/item) & set it in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object.

##### Sample request payload:

```
  {
    "TxnDate": "2024-08-01T12:00:00Z",
    "NameOf": "Employee",
    "EmployeeRef": { "value": "1" },
    "PayrollItemRef": {
      "value": "626270109"
    },
    "CustomerRef": {
        "value": "2"
    },
    "ProjectRef": {
        "value":"416296152"
    },
    "ItemRef": { "value": "1" },
    "Hours": 8,
    "Minutes": 0,
    "Description": "Construction:DailyWork"
  }

```

### Use Case 2: Create Time Activity with pay type
This use case is applicable for the customers who are enrolled to **QuickBooks Payroll** and **do not** have the **Projects** enabled in QuickBooks Online.

##### Use [V3 Create TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity#create-a-timeactivity-object) API to create time activity by

- Fetch `employee->id` from [V3 Accounting Employee Query](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/employee#query-an-employee) & set it in `EmployeeRef` in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object with `NameOf` field as `Employee`. 
- Fetching `compensation ->id` from [Read EmployeeCompensation(Query)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/employeeCompensation/#sample-query-body) GraphQL API & set it in `PayrollItemRef` in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object.

##### Sample request payload:

```
  {
    "TxnDate": "2024-08-01T12:00:00Z",
    "NameOf": "Employee",
    "EmployeeRef": { "value": "1" },
    "PayrollItemRef": {
      "value": "626270109"
    },
    "Hours": 8,
    "Minutes": 0,
    "Description": "Construction:TimeOff"
  }

```

### Use Case 3: Create Time Activity and link to projects
This use case is applicable for the customers who are **not** enrolled to **QuickBooks Payroll** and have the **Projects enabled** in QuickBooks Online.

##### Use [V3 Create TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity#create-a-timeactivity-object) API to create time activity by 

- Fetch `employee->Id` by calling [V3 Accounting Employee Query](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/employee#query-an-employee)  & set it in `EmployeeRef` in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object with `NameOf` field as `Employee`.
- Fetch  `Item->id` from [V3 Accounting Query Item API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/item) & set it in `ItemRef` in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object.
- Fetch `project -> Id` from [Read Project(Query)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#read-project) GraphQL API to read `project -> id` & set it in `ProjectRef` in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object.
- Fetch `Customer -> id` from [V3 Accounting Customer Query API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/customer#query-a-customer) (optional) & set it in `CustomerRef` in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object.

##### Sample request payload:

```
  {
    "TxnDate": "2024-08-01T12:00:00Z",
    "NameOf": "Employee",
    "EmployeeRef": { "value": "1" },
    "ItemRef": { "value": "1" },
    "CustomerRef": {
        "value": "2"
    },
    "ProjectRef": {
        "value":"416296152"
    },
    "Hours": 8,
    "Minutes": 0,
    "Description": "Construction:DailyWork"
  }

```

### Use Case 4: Create Time Entry for employee

This use case is applicable for the customers who are **not** enrolled to **QuickBooks Payroll** and do **not** have the **Projects enabled** in QuickBooks Online.

#### Use [V3 Create TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity#create-a-timeactivity-object) API to create time activity by 

- Read `employee->Id` by calling [V3 Accounting Employee Query](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/employee#query-an-employee)  & set it in `EmployeeRef` in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object with `NameOf` as `Employee`.
- Fetch  `Item->id` from [V3 Accounting Query Item API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/item) & set it in `ItemRef` in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object.

##### Sample request payload:

```
  {
    "TxnDate": "2024-08-01T12:00:00Z",
    "NameOf": "Employee",
    "EmployeeRef": { "value": "1" },
    "ItemRef": { "value": "1" },
    "Hours": 8,
    "Minutes": 0,
    "Description": "Construction:DailyWork"
  }

```

### Use Case 5: Create Time Activity for contractor

This use case is applicable to track time for contractors and for the customers who do **not** have the **Projects enabled** in QuickBooks Online.

#### Use [V3 Create TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity#create-a-timeactivity-object) API to create time activity by
- Fetch `Vendor -> Id` from [V3 Accounting Vendor Query API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/vendor#query-a-vendor) & set it in `VendorRef` in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object with `NameOf` as `Vendor`.
- Fetch  `Item->id` from [V3 Accounting Query Item API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/item) & set it in `ItemRef` in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object.

##### Sample request payload:

```
  {
    "TxnDate": "2024-08-01T12:00:00Z",
    "NameOf": "Vendor",
    "VendorRef": { "value": "5" },
    "ItemRef": { "value": "1" },
    "Hours": 8,
    "Minutes": 0,
    "Description": "Construction:DailyWork"
  }

```

### Use Case 6: Create Time Activity for contractor and link it to projects
This use case is applicable to track time for **contractors** and for customers who have the **Projects enabled** in QuickBooks Online.

#### Use [V3 Create TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity#create-a-timeactivity-object) API to create time activity by

- Fetch `Vendor -> Id` from [V3 Accounting Vendor Query API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/vendor#query-a-vendor) & set it in `VendorRef` in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object with `NameOf` as `Vendor`.
- Fetch `project -> Id` from [Read Project(Query)](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#read-project) GraphQL API to read `project -> id` & set it in `ProjectRef` in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object.
- Fetch `Customer -> id` from [V3 Accounting Customer Query API](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/customer#query-a-customer) (optional) & set it in `CustomerRef` in the [TimeActivity](https://developer.intuit.com/app/developer/qbo/docs/api/accounting/all-entities/timeactivity) object.

##### Sample request payload:

```
  {
    "TxnDate": "2024-08-01T12:00:00Z",
    "NameOf": "Vendor",
    "VendorRef": { "value": "5" },
    "CustomerRef": {
        "value": "2"
    },
    "ProjectRef": {
        "value":"416296152"
    },
    "ItemRef": { "value": "1" },
    "Hours": 8,
    "Minutes": 0,
    "Description": "Construction:DailyWork"
  }

```

### Appendix

Reference of (Contractor) -  Vendor with `Vendor1099` true:
![](/intuit-api/assets/images/Vendor1099.png)
