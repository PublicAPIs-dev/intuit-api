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

- Read - Query (POST)
- Create - Mutation (POST)
- Update - Mutation (POST)
- Delete - Mutation (POST)

### Endpoint

-   For production apps:  https://qb.api.intuit.com/graphql

### Time Entry Fields

| Field                  | Type                               | Required | Description                                                                               |
|------------------------|------------------------------------|----------|-------------------------------------------------------------------------------------------|
| startDate              | Date!                              | yes      | The date on which this time entry begin.                                                  |
| duration               | Int!                               | yes      | The number of seconds recorded by this time entry.                                        |
| timeFor                | TimeTracking_TrackTimeForInput!    | yes      | The ID and type whose time is recorded for on this time entry.                            |
| timeAgainst            | TimeTracking_TrackTimeAgainstInput | no       | The ID and type of entity that this time entry is tracking time against.                  |
| notes                  | String                             | no       | Notes associated with this time entry (2048 character limit).                             |
| classId                | ID                                 | no       | The ID of the class that this time entry is tracking time against.                        |
| employeeCompensationId | ID                                 | no       | The ID of the Payroll_EmployeeCompensation that this time entry is tracking time against. |
| serviceItemId          | ID                                 | no       | The ID of the Service Item(Product/Services) that applies to this time entry.             |
| billable               | Boolean                            | no       | Whether the time is billable.                                                             |

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
        timezone
        notes
        attachedFileCount {
          attachmentCount
          signatureCount
        }
        employeeCompensation {
          id
        }
        billable
        state
        locked
        lockedReasons
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
     "ids":[664243412,664243674],
     "states": ["OPEN", "CLOSED", "APPROVED"]
    },
    "orderBy": ["START_ASC","START_DESC","END_ASC",
    "END_DESC","DURATION_ASC","DURATION_DESC"
    ] 
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
                        "id": "664243412",
                        "duration": 30,
                        "entryMethod": "DURATION",
                        "startDate": "2024-06-11",
                        "startTime": null,
                        "endTime": null,
                        "timezone": "",
                        "notes": "test note employee",
                        "attachedFileCount": {
                            "attachmentCount": 0,
                            "signatureCount": 0
                        },
                        "employeeCompensation": null,
                        "billable": true,
                        "state": "CLOSED",
                        "locked": false,
                        "lockedReasons": []
                    }
                }
            ],
            "pageInfo": {
                "hasNextPage": true,
                "hasPreviousPage": false,
                "startCursor": "djI6OjE6OjE6OnsiaWRzIjpbIjY2NDI0MzQxMiIsIjY2NDI0MzY3NCJdLCJzdGF0ZXMiOlsiT1BFTiIsIkNMT1NFRCIsIkFQUFJPVkVEIl19OjpbWyJzdGFydCIsIkFTQyJdLFsic3RhcnQiLCJERVNDIl0sWyJlbmQiLCJBU0MiXSxbImVuZCIsIkRFU0MiXSxbImR1cmF0aW9uIiwiQVNDIl0sWyJkdXJhdGlvbiIsIkRFU0MiXV0=",
                "endCursor": "djI6OjE6OjE6OnsiaWRzIjpbIjY2NDI0MzQxMiIsIjY2NDI0MzY3NCJdLCJzdGF0ZXMiOlsiT1BFTiIsIkNMT1NFRCIsIkFQUFJPVkVEIl19OjpbWyJzdGFydCIsIkFTQyJdLFsic3RhcnQiLCJERVNDIl0sWyJlbmQiLCJBU0MiXSxbImVuZCIsIkRFU0MiXSxbImR1cmF0aW9uIiwiQVNDIl0sWyJkdXJhdGlvbiIsIkRFU0MiXV0="
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
input TimeTracking_TimeEntryInputFilter  {
    """
    A list of time entry IDs to filter by. Example: "ids": [123, 456, 789]
    """
    ids: [ID!]

    """
    Specify whether to limit your query on time entries with a date falling in a date range
    """
    dateRange: TimeTracking_DatePeriod

    """
    Filters retrieved time entries by state. If not specified, will return all states except OPEN.
    """
    states: [TimeTracking_TimeEntryState]

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

### TimeTracking_TimeEntryState

```
"""
Indicates the current state of a time entry (`[OPEN, CLOSED, SUBMITTED, or APPROVED]`).
NOTE: A time entry may only be in one of these four states at a time, they're mutually
exclusive of one another.
"""
enum TimeTracking_TimeEntryState  {
    """
    If the state is `OPEN`, it means that the entity associated with the time entry is currently on the clock, thus the time entry is `OPEN`.
    """
    OPEN

    """
    If the state is `CLOSED`, then the entity has clocked out of the time entry and thus it has both a start and an end time. Duration only time entries are always `CLOSED`.
    """
    CLOSED

    """
    If the state is `SUBMITTED` then the entity has submitted the time frame that encompasses this time entry.
    """
    SUBMITTED

    """
    If the state is `APPROVED`, then a manager or admin has approved the time frame that encompasses this time entry.
    """
    APPROVED
}
```

### Sample Filter
```
{
"after":null,
"first":1,
    "filter": {
     "dateRange": {"beginDate": "2024-06-01", "endDate": "2024-06-30"
     },
     "states": ["OPEN", "CLOSED", "APPROVED"]
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
        attachedFileCount {
          attachmentCount
          signatureCount
        }
        employeeCompensation {
          id
        }
        billable
        state
        locked
        lockedReasons
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
      "timeFor": { "id": 3, 
      "entityType": "EMPLOYEE" 
       } ,
      "startDate": "2024-06-12",
      "duration": 30,
      "timeAgainst": { 
        "id": 1, 
        "entityType": "CUSTOMER"
       },
      "employeeCompensationId":null,
      "serviceItemId": 2,
      "billable": true,
      "notes": "test note employee"
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
                "id": "666350208",
                "meta": {
                    "updatedAt": "2024-06-17T05:00:24.000Z"
                },
                "timeFor": {
                    "id": "3"
                },
                "entryMethod": "DURATION",
                "startDate": "2024-06-12",
                "startTime": null,
                "endTime": null,
                "notes": "test note employee",
                "serviceItem": {
                    "id": "2"
                },
                "attachedFileCount": {
                    "attachmentCount": 0,
                    "signatureCount": 0
                },
                "employeeCompensation": null,
                "billable": true,
                "state": "CLOSED",
                "locked": false,
                "lockedReasons": []
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
    "id": 666350208,
     "duration": 40, 
    "serviceItemId": 1,
    "employeeCompensationId": null,
    "timeAgainst": {
        "id": 1, 
        "entityType": "CUSTOMER"
        } ,
     "billable": true, 
     "notes": "update mutation testing"
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
                "id": "666350208",
                "startDate": "2024-06-12",
                "duration": 40,
                "billable": true,
                "serviceItem": {
                    "id": "1"
                },
                "classValue": null,
                "employeeCompensation": null
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
                        timeEntry {
                            id
                        }
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
    "id": 666350223
  }
}
```

Response:
```
{
    "data": {
        "timeTrackingDeleteTimeEntry": {
            "successCode": "SUCCESS",
            "timeEntry": {
                "id": "666350208"
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