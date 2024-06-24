---
layout: default
title: Time Entry
nav_order: 1
parent: Schema Entities
---

## Time Entry

The APIs related to the Time Entry allow you to manage Time so that you can track your Time entry for Employee/Vendor Contractor(1099) against the customer/project.
The Time API provides support for create, read, update and delete operations.

### Operations for Time Entry

- Read - Query (POST)
- Create - Mutation (POST)
- Update - Mutation (POST)
- Delete - Mutation (POST)

### Endpoints

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

### TimeTracking_CreateTimeEntryByDurationInput 

| Field                  | Type                               | Required | Description                                                              |
|------------------------|------------------------------------|----------|--------------------------------------------------------------------------|
| startDate              | Date!                              | yes      | The date on which this time entry begins.                                |
| duration               | Int!                               | yes      | The total number of seconds to record for this time entry.               |
| timeFor                | TimeTracking_TrackTimeForInput!    | yes      | The ID and type whose time is recorded for on this time entry.           |
| timeAgainst            | TimeTracking_TrackTimeAgainstInput | no       | The ID and type of entity that this time entry is tracking time against. |
| notes                  | String                             | no       | Notes associated with this time entry (2048 character limit).            |
| classId                | ID                                 | no       | The ID of the class that this time entry is tracking time against.       |
| employeeCompensationId | ID                                 | no       | The ID of the Employee that this time entry is tracking time against.    |
| serviceItemId          | ID                                 | no       | The ID of the Service Item that applies to this time entry.              |
| billable               | Boolean                            | no       | Whether the time is billable.                                            |


### TimeTracking_UpdateTimeEntryByDurationInput ( all the inputs as CreateInput applies just with additional input Id)

| Field | Type | Required | Description                             |
|-------|------|----------|-----------------------------------------|
| id    | ID!  | yes      | The ID of the time entry to be updated. |

### TimeTracking_DeleteTimeEntryInput (just the ID)

| Field | Type | Required | Description                             |
|-------|------|----------|-----------------------------------------|
| id    | ID!  | yes      | The ID of the time entry to be updated. |

### TimeTracking_TrackTimeForInput!

| Field      | Type                          | Required | Description                                  |
|------------|-------------------------------|----------|----------------------------------------------|
| id         | ID!                           | yes      | The ID of the entity (e.g. Employee, Vendor) |
| entityType | TimeTracking_TimeTrackForType | no       | The entity type that is being tracked        |

### TimeTracking_TimeTrackForType

| eNum     | Description                   |
|----------|-------------------------------|
| EMPLOYEE | The ID references an Employee |
| VENDOR   | The ID references a Vendor    |

### TimeTracking_TrackTimeAgainstInput

| Field      | Type                               | Required | Description                                   |
|------------|------------------------------------|----------|-----------------------------------------------|
| id         | ID!                                | yes      | The ID of the entity (e.g. customer, project) |
| entityType | TimeTracking_TrackTimeAgainstType! | yes      | TThe entity type that is being tracked        |

### TimeTracking_TrackTimeAgainstType

| eNum     | Description                  |
|----------|------------------------------|
| CUSTOMER | The ID references a Customer |
| PROJECT  | The ID references a Project  |


### Sample query header

-   Content-type: **application/json**
-   Use the time scope for the authorization header  
**[time-tracking.time-entry (read & write)** | **time-tracking.time-entry.read (read only)]** 

### Sample query body

Sample query (Query Time Entries):
```
query readMinimalTimeSheet {
  timeTrackingTimeEntries(
    filter: {ids:[timeentry1,timeentry2]}
  ) {
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

## Filter support:

Currently, the filters supported on time entry includes -

   - `ids` - A list of time entry IDs to filter by. Example: "ids": [123, 456, 789]
   - `dateRange` : TimeTracking_DatePeriod - Specify whether to limit your query on time entries with a date falling in a date range
      - `beginDate` - beginning date of the time entry.
      -  `endDate` -  ending date of the time entry.
   - `states` : [TimeTracking_TimeEntryState] - Filters retrieved time entries by state. If not specified, will return all states except OPEN. Possible values
      - `OPEN` - it means that the entity associated with the time entry is currently on the clock.
      - `CLOSED` - the entity has clocked out of the time entry and thus it has both a start and an end time. Duration only time entries are always "CLOSED".
      - `SUBMITTED` - the entity has submitted the time frame that encompasses this time entry.
      - `APPROVED` - then a manager or admin has approved the time frame that encompasses this time entry.

You can create a joint filter by including more than one filterable field. Sample filter **query by dateRange** & states in the filter. 
For filtering by time entry Ids use [ eg: "ids":[664243412,664243674]]

```
{
"after":null,
"first":1,
    "filter": {
     "dateRange": {"beginDate": "2024-06-01", "endDate": "2024-06-30"
     },
     "states": ["OPEN", "CLOSED", "APPROVED"]
    },
    "orderBy": ["START_ASC","START_DESC","END_ASC",
    "END_DESC","DURATION_ASC","DURATION_DESC"
    ] 
 }


```

### Create mutation
 
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

### Update mutation

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
### Delete Mutation

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