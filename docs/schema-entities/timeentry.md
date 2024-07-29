---
layout: default
title: Time Entry
nav_order: 1
parent: Schema Entities
---

## Time Entry

The APIs related to the Time Entry allow you to track & manage time entries for Employee/Vendor Contractor(1099) against the customer/project.
The Time Entry API supports  create, read, update and delete operations.

### Operations for Time Entry

- [Read](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/timeentry/#query-time-entry) - Query (POST)
- [Create](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/timeentry/#create-time-entry) - Mutation (POST)
- [Update](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/timeentry/#update-time-entry) - Mutation (POST)
- [Delete](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/timeentry/#delete-time-entry) - Mutation (POST)

### Endpoint

-   For production apps:  https://qb.api.intuit.com/graphql

### Time Entry Fields

| Field                  | Type                                                                                                                                                  | Required | Description                                                                               |
|------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------|----------|-------------------------------------------------------------------------------------------|
| startDate              | Date!                                                                                                                                                 | yes      | The date on which this time entry begin.                                                  |
| duration               | Int!                                                                                                                                                  | yes      | The number of seconds recorded by this time entry.                                        |
| timeFor                | [TimeTracking_TrackTimeForInput](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/timeentry/#timetracking_tracktimeforinput)!        | yes      | The ID and type whose time is recorded for on this time entry.                            |
| timeAgainst            | [TimeTracking_TrackTimeAgainstInput](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/timeentry/#timetracking_tracktimeagainstinput) | no       | The ID and type of entity that this time entry is tracking time against.                  |
| notes                  | String                                                                                                                                                | no       | Notes associated with this time entry (2048 character limit).                             |
| classId                | ID                                                                                                                                                    | no       | The ID of the class that this time entry is tracking time against.                        |
| employeeCompensationId | ID                                                                                                                                                    | no       | The ID of the Payroll_EmployeeCompensation that this time entry is tracking time against. |
| serviceItemId          | ID                                                                                                                                                    | no       | The ID of the Service Item(Product/Services) that applies to this time entry.             |
| billable               | Boolean                                                                                                                                               | no       | Whether the time is billable.                                                             |

### Input Variables:

### TimeTracking_TrackTimeForInput

```
"""
Input arguments whose time is being tracked
"""
input TimeTracking_TrackTimeForInput  {
    """
    The ID of the entity
    """
    id: ID!

    """
    The entity type that is being tracked. If not set defaults to Employee
    """
    entityType: TimeTracking_TimeTrackForType! = EMPLOYEE 
}
```

### TimeTracking_TimeTrackForType

```
"""
Indicates the type for which time is being tracked
NOTE: This list may be extended in the future and is used as input only.
"""
enum TimeTracking_TimeTrackForType {
    """
    The ID references an WorkerManagement_Employee
    """
    EMPLOYEE

    """
    The ID references a Commerce_Vendor
    """
    VENDOR
}
```

### TimeTracking_TrackTimeAgainstInput
```
"""
Input arguments for the entity that is being tracked against
"""
input TimeTracking_TrackTimeAgainstInput {
    """
    The ID of the entity (e.g. customer, project)
    """
    id: ID!

    """
    The entity type that is being tracked against
    """
    entityType: TimeTracking_TrackTimeAgainstType!
}
```

### TimeTracking_TrackTimeAgainstType
```
"""
Indicates the type of the entity being tracked against. (`[CUSTOMER, PROJECT]`).
"""
enum TimeTracking_TrackTimeAgainstType  {
    """
    The ID references a Customer
    """
    CUSTOMER

    """
    The ID references a Project
    """
    PROJECT
}
```

### Query Time Entry

Read Time Entries:
```
query readMinimalTimeSheet (
 $after: String,
  $first: Int,
  $filter: TimeTracking_TimeEntryInputFilter,
  $orderBy: [TimeTracking_TimeEntryOrderBy]
)  {
  timeTrackingTimeEntries( 
    after: $after,
    first: $first,
    filter: $filter,
    orderBy: $orderBy
    )
   {
    edges {
      node {
        id
        duration
        entryMethod
        startDate
        entryMethod
        startTime
        endTime
        duration
        notes
        employeeCompensation {
          id
        }
        billable
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
"after":null,
"first":1,
    "filter": {
     "ids":[119043230]
    },
    "orderBy": ["START_ASC"] 
 }

```

Response:
``` 
{
    "data": {
        "timeTrackingTimeEntries": {
            "edges": [
                {
                    "node": {
                        "id": "119043230",
                        "duration": 28800,
                        "entryMethod": "DURATION",
                        "startDate": "2024-07-22",
                        "startTime": null,
                        "endTime": null,
                        "notes": "",
                        "employeeCompensation": {
                            "id": "623972114"
                        },
                        "billable": true
                    }
                }
            ],
            "pageInfo": {
                "hasNextPage": false,
                "hasPreviousPage": false,
                "startCursor": "djI6OjE6OjE6OnsiaWRzIjpbIjExOTA0MzIzMCJdfTo6W1sic3RhcnQiLCJBU0MiXSxbInN0YXJ0IiwiREVTQyJdLFsiZW5kIiwiQVNDIl0sWyJlbmQiLCJERVNDIl0sWyJkdXJhdGlvbiIsIkFTQyJdLFsiZHVyYXRpb24iLCJERVNDIl1d",
                "endCursor": "djI6OjE6OjE6OnsiaWRzIjpbIjExOTA0MzIzMCJdfTo6W1sic3RhcnQiLCJBU0MiXSxbInN0YXJ0IiwiREVTQyJdLFsiZW5kIiwiQVNDIl0sWyJlbmQiLCJERVNDIl0sWyJkdXJhdGlvbiIsIkFTQyJdLFsiZHVyYXRpb24iLCJERVNDIl1d"
            }
        }
    }
}
```

### Filter support:

### TimeTracking_TimeEntryInputFilter

```
"""
Input filter arguments for a timeTrackingTimeEntries query
"""
input TimeTracking_TimeEntryInputFilter @tag(name: "qb") @tag(name: "qb-external") {
    """
    A list of time entry IDs to filter by. Example: "ids": [123, 456, 789]
    """
    ids: [ID!]

    """
    The time against entities to filter by
    """
    timeAgainst: TimeTracking_TimeAgainstFilter

    """
    Specify whether to limit your query on time entries with a date falling in a date range
    """
    dateRange: TimeTracking_DatePeriod
}
```

### TimeTracking_TimeAgainstFilter
```
"""
Input filter arguments for the timeTrackingTimeEntries query to filter by the time against entities
"""
input TimeTracking_TimeAgainstFilter @tag(name: "qb") @tag(name: "qb-external") {
    """
    A list of project IDs to filter by. Example: "ids": [123, 456, 789]
    """
    projectIds: [ID!]

    """
    A list of customer IDs to filter by. Example: "ids": [123, 456, 789]
    """
    customerIds: [ID!]
}

```

### TimeTracking_DatePeriod

```
"""
Period of time represented by two dates (e.g. 2015-03-05 through 2015-03-11)
"""
input TimeTracking_DatePeriod {
    beginDate: Date!
    endDate: Date!
}
```

### Sample Filter
```
{
"after":null,
"first":1,
    "filter": {
     "dateRange": {"beginDate": "2024-06-01", "endDate": "2024-06-30"
     }
    },
    "orderBy": ["START_ASC"] 
 }
```

### TimeTracking_TimeEntryOrderBy

```

"""
Indicates the order of the timeTrackingTimeEntries query results
"""
enum TimeTracking_TimeEntryOrderBy {
    """
    Order query results by the start date/time of the time entry in ascending fashion
    """
    START_ASC

    """
    Order query results by the start date/time of the time entry in descending fashion
    """
    START_DESC

    """
    Order query results by the end date/time of the time entry in ascending fashion
    """
    END_ASC

    """
    Order query results by the end date/time of the time entry in descending fashion
    """
    END_DESC

    """
    Order query results by the time entry duration in ascending fashion
    """
    DURATION_ASC

    """
    Order query results by the time entry duration in descending fashion
    """
    DURATION_DESC
}

```

### Create Time Entry
 
Mutation:
 
```
mutation createTimeEntryByDuration ($input: TimeTracking_CreateTimeEntryByDurationInput!) {
  timeTrackingCreateTimeEntryByDuration(input: $input) {
    ... on TimeTracking_CreateTimeEntryByDurationPayload {
      successCode
      timeEntry {
        id
        meta {
          updatedAt
        }
        timeFor {
          ... on WorkerManagement_Employee {
            id
          }
        }
        entryMethod
        startDate
        startTime
        endTime
        notes
        serviceItem {
          id
        }
        employeeCompensation {
          id
        }
        billable
      }
    }
    ... on TimeTracking_CreateTimeEntryByDurationError {
      errorCode
    }
  }
}
```

Required fields:

- startDate - The date on which this time entry begins.
- duration - The number of seconds recorded by this time entry
- timeFor - The ID and type whose time is recorded for on this time entry [EMPLOYEE, VENDOR]

### TimeTracking_CreateTimeEntryByDurationInput 

```
"""
Input arguments for creating a duration based time entry
"""
input TimeTracking_CreateTimeEntryByDurationInput {
    """
    The date on which this time entry begins
    """
    startDate: Date!

    """
    The number of seconds recorded by this time entry
    """
    duration: Int!

    """
    The ID and type whose time is recorded for on this time entry
    """
    timeFor: TimeTracking_TrackTimeForInput!

    """
    The ID and type of entity that this time entry is tracking time against.
    """
    timeAgainst: TimeTracking_TrackTimeAgainstInput

    """
    Notes associated with this time entry (2048 character limit)
    """
    notes: String

    """
    The ID of the AppFoundations_CustomDimensionValue that this time entry is tracking time against.
    """
    classId: ID

    """
    The ID of the Payroll_EmployeeCompensation that this time entry is tracking time against.
    """
    employeeCompensationId: ID

    """
    The ID of the Service Item Commerce_ProductVariant that applies to this time entry
    """
    serviceItemId: ID

    """
    Whether the time is billable
    """
    billable: Boolean
}

```
 
Sample Variables: 
``` 
   {
 "input": {
      "timeFor": { "id": 4, 
      "entityType": "EMPLOYEE" 
       } ,
      "startDate": "2024-07-29",
      "duration": 28800,
      "timeAgainst": { 
        "id": 411939876,
   	     "entityType": "PROJECT"
       },
      "employeeCompensationId": 623972114, 
      "serviceItemId": 1,
      "billable": true,
      "notes": "test note vendor timeentry with project"
    }
 }

```    

Sample response:
```
{
    "data": {
        "timeTrackingCreateTimeEntryByDuration": {
            "successCode": "SUCCESS",
            "timeEntry": {
                "id": "122533318",
                "meta": {
                    "updatedAt": "2024-07-29T21:42:25.000Z"
                },
                "timeFor": {
                    "id": "4"
                },
                "entryMethod": "DURATION",
                "startDate": "2024-07-29",
                "startTime": null,
                "endTime": null,
                "notes": "test note vendor timeentry with project",
                "serviceItem": {
                    "id": "1"
                },
                "employeeCompensation": {
                    "id": "623972114"
                },
                "billable": true
            }
        }
    }
}

```

### Update Time Entry

Mutation:

``` 
mutation updateTimesheetByDuration ($input: TimeTracking_UpdateTimeEntryByDurationInput!) {
              timeTrackingUpdateTimeEntryByDuration (input: $input) {
                ... on TimeTracking_UpdateTimeEntryByDurationPayload {
                  successCode
                  timeEntry {
                    id
                    startDate
                    duration
                    billable
                    serviceItem {
                      id
                    }
                    classValue {
                      id
                    }
                    employeeCompensation {
                      id
                    }
                  }
                }
                ... on TimeTracking_UpdateTimeEntryByDurationError {
                  errorCode
                }
              }
            }

```
Required field:

- id: ID of an time entry

Variables:
```
  {
  "input": {
    "id": 122533318,
     "duration": 28800, 
    "serviceItemId": 1,
    "employeeCompensationId": 623972114,
    "timeAgainst": {
        "id": 411939876, 
        "entityType": "PROJECT"
        } ,
     "billable": true, 
     "notes": "update notes as time with employeeCompensation for employee"
  }
}
 

```

Response:
```
{
    "data": {
        "timeTrackingUpdateTimeEntryByDuration": {
            "successCode": "SUCCESS",
            "timeEntry": {
                "id": "122533318",
                "startDate": "2024-07-29",
                "duration": 28800,
                "billable": true,
                "serviceItem": {
                    "id": "1"
                },
                "classValue": null,
                "employeeCompensation": {
                    "id": "623972114"
                }
            }
        }
    }
}
```

### TimeTracking_UpdateTimeEntryByDurationInput

```
"""
Input arguments for updating a duration based time entry
"""
input TimeTracking_UpdateTimeEntryByDurationInput {
    """
    The ID of the time entry to be updated.
    """
    id: ID!

    """
    The date on which this time entry begins
    """
    startDate: Date

    """
    The total number of seconds to record for this time entry
    """
    duration: Int

    """
    The ID and type of entity that this time entry is tracking time against.
    """
    timeAgainst: TimeTracking_TrackTimeAgainstInput

    """
    Notes associated with this time entry (2048 character limit)
    """
    notes: String

    """
    The ID of the AppFoundations_CustomDimensionValue that this time entry is tracking time against.
    """
    classId: ID

    """
    The ID of the Payroll_EmployeeCompensation that this time entry is tracking time against.
    """
    employeeCompensationId: ID

    """
    The ID of the Service Item Commerce_ProductVariant that applies to this time entry
    """
    serviceItemId: ID

    """
    Whether the time is billable
    """
    billable: Boolean
}
```
### Delete Time Entry

Mutation:

``` 
mutation timeTrackingDeleteTimesheet($input: TimeTracking_DeleteTimeEntryInput!) {
                timeTrackingDeleteTimeEntry (input: $input) {
                    ...on TimeTracking_DeleteTimeEntryPayload {
                        successCode
                        deletedTimeEntryId
                    }
                    ... on TimeTracking_DeleteTimeEntryError {
                      errorCode
                    }
                }
            }

```
Required field:

- id: ID of an time entry

Variables:
``` 
{
  "input": {
    "id": 119043230
  }
}
```

Response:
```
{
    "data": {
        "timeTrackingDeleteTimeEntry": {
            "successCode": "SUCCESS",
                "deletedTimeEntryId": "119043230"
            }
        }
    }
}
```

### TimeTracking_DeleteTimeEntryInput

```
"""
Input arguments for deleting a time entry
"""
input TimeTracking_DeleteTimeEntryInput @tag(name: "qb") @tag(name: "qb-external") {
    """
    The ID of the time entry to be deleted.
    """
    id: ID!
}

```