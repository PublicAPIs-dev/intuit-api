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
- Query the QuickBooks company preferences by using V3 Accounting REST Preferences API and check for Preferences->OtherPrefs->NameValue> to identify if projects are supported for the company. Check if NameValue `ProjectsEnabled` is true.


#### Use Case 1: Create Timesheet with paytype and link it to a Project
This usescase is applicable for customers who are enrolled to QuickBooks Payroll and have the Projects setting enabled in QuickBooks Online.

##### Step 1: Use V3 Accounting Rest API to query employee and get the employee->id value. 

```

```

##### Step 2: Use GraphQL API to query compensation data.
```

```

##### Step 3: Use GraphQL API to read projects.
```

```

##### Step 4: Use GraphQL API to create timesheet.
Pre-steps:
- Use V3 Accounting API to query item (optional) 
- Use the compensation id from Step 2 to record paytype.
- Use the project id from step 3 to set ProjectRef 


```

```



#### Use Case 2: Create Timesheet with paytype
This usescase is applicable for customers who are enrolled to QuickBooks Payroll and do not have the Projects setting enabled in QuickBooks Online.

##### Step 1: Use V3 Accounting Rest API to query employee and get the employee->id value. 

```

```

##### Step 2: Use GraphQL API to query compensation data.
```

```


##### Step 3: Use GraphQL API to create timesheet.
Pre-steps:
- Use V3 Accounting API to query item (optional) 
- Use the compensation id from Step 2 to record paytype.


```

```


#### Use Case 3: Create Timesheet and link to projects
This usescase is applicable for customers who are not enrolled to QuickBooks Payroll and have the Projects setting enabled in QuickBooks Online.

##### Step 1: Use GraphQL API to read projects.
```

```


##### Step 2: Use GraphQL API to create timesheet.
Pre-steps:
- Use V3 Accounting API to query employee, item (optional) 
- Use the project id from step 1 to set ProjectRef 


```

```


#### Use Case 4: Create Timesheet for employee
This usescase is applicable for customers who are not enrolled to QuickBooks Payroll and do not have the Projects setting enabled in QuickBooks Online.

Pre-steps:
- Use V3 Accounting API to query employee, item (optional) 

```

```

#### Use Case 5: Create Timesheet for contractor
This usescase is applicable to track time for contractors and for customers who do not have the Projects setting enabled in QuickBooks Online.

Pre-steps:
- Use V3 Accounting API to query vendor (1099flag), customer, Item (optional) 

```

```

#### Use Case 5: Create Timesheet for contractor and link it to projects
This usescase is applicable to track time for contractors and for customers who have the Projects setting enabled in QuickBooks Online.


##### Step 1: Use GraphQL API to read projects.
```

```

##### Step 2: Use GraphQL API to create timesheet.
Pre-steps:
- Use V3 Accounting API to query vendor and identify contractors by checking the `vendor->1099flag` is true. 
- Use the project id from step 1 to set ProjectRef 

```

```

