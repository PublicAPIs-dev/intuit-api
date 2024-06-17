---
layout: default
title: Time
nav_order: 1
parent: Schema Entities
---

## Time

The APIs related to the Time entity allow you to manage Time so that you can track your Time entry for the project and employee.
The Time API provides support for create, read, update and delete operations.

### Operations for Time entity

- Read - Query (POST)
- Create - Mutation (POST)
- Update - Mutation (POST)
- Delete - Mutation (POST)

### Endpoints

-   For production apps:  https://qb.api.intuit.com/graphql

### Time Entry Fields

| #   | Field Name             | Data Type                          | Required | Field Definition                                                                          |
|-----|------------------------|------------------------------------|----------|-------------------------------------------------------------------------------------------|
| 1   | startDate              | Date!                              |          | The date on which this time entry begin.                                                  |
| 2   | duration               | Int!                               |          | The number of seconds recorded by this time entry.                                        |
| 3   | timeFor                | TimeTracking_TrackTimeForInput!    |          | The ID and type whose time is recorded for on this time entry.                            |
| 4   | timeAgainst            | TimeTracking_TrackTimeAgainstInput |          | The ID and type of entity that this time entry is tracking time against.                  |
| 5   | notes                  | String                             |          | Notes associated with this time entry (2048 character limit).                             |
| 6   | classId                | ID                                 |          | The ID of the class that this time entry is tracking time against.                        |
| 7   | employeeCompensationId | ID                                 |          | The ID of the Payroll_EmployeeCompensation that this time entry is tracking time against. |
| 8   | serviceItemId          | ID                                 |          | The ID of the Service Item(Product/Services) that applies to this time entry.             |
| 9   | billable               | Boolean                            |          | Whether the time is billable.                                                             |

### Input Variables:

### TimeTracking_CreateTimeEntryByDurationInput 

| #   | Input Name             | Fields                             | Type | Field Definition                                                         |
|-----|------------------------|------------------------------------|------|--------------------------------------------------------------------------|
| 2   | startDate              | Date                               |      | The date on which this time entry begins.                                |
| 2   | duration               | Int                                |      | The total number of seconds to record for this time entry.               |
| 2   | timeFor                | TimeTracking_TrackTimeForInput!    |      | The ID and type whose time is recorded for on this time entry.           |
| 2   | timeAgainst            | TimeTracking_TrackTimeAgainstInput |      | The ID and type of entity that this time entry is tracking time against. |
| 2   | notes                  | String                             |      | Notes associated with this time entry (2048 character limit).            |
| 2   | classId                | ID                                 |      | The ID of the class that this time entry is tracking time against.       |
| 2   | employeeCompensationId | ID                                 |      | The ID of the Employee that this time entry is tracking time against.    |
| 2   | serviceItemId          | ID                                 |      | The ID of the Service Item that applies to this time entry.              |
| 2   | billable               | Boolean                            |      | Whether the time is billable.                                            |


### TimeTracking_UpdateTimeEntryByDurationInput ( all the inputs as CreateInput applies just with additional input Id)

| #   | Input Name | Fields | Type | Field Definition                        |
|-----|------------|--------|------|-----------------------------------------|
| 1   | id         | ID!    |      | The ID of the time entry to be updated. |

### TimeTracking_DeleteTimeEntryInput (just the ID)

| #   | Input Name | Fields | Type | Field Definition                        |
|-----|------------|--------|------|-----------------------------------------|
| 1   | id         | ID!    |      | The ID of the time entry to be updated. |

### TimeTracking_TrackTimeForInput!

| #   | Input Name | Fields                        | Type | Field Definition                             |
|-----|------------|-------------------------------|------|----------------------------------------------|
| 1   | id         | ID!                           |      | The ID of the entity (e.g. Employee, Vendor) |
| 2   | entityType | TimeTracking_TimeTrackForType |      | The entity type that is being tracked        |

### TimeTracking_TimeTrackForType

| #   | eNum     | Field Definition              |
|-----|----------|-------------------------------|
| 1   | EMPLOYEE | The ID references an Employee |
| 2   | VENDOR   | The ID references a Vendor    |

### TimeTracking_TrackTimeAgainstInput

| #   | Input Name | Fields                             | Type | Field Definition                              |
|-----|------------|------------------------------------|------|-----------------------------------------------|
| 1   | id         | ID!                                |      | The ID of the entity (e.g. customer, project) |
| 2   | entityType | TimeTracking_TrackTimeAgainstType! |      | TThe entity type that is being tracked        |

### TimeTracking_TrackTimeAgainstType

| #   | eNum     | Field Definition             |
|-----|----------|------------------------------|
| 1   | CUSTOMER | The ID references a Customer |
| 2   | PROJECT  | The ID references a Project  |


### Sample query header

-   Content-type: **application/json**
-   Use the time scope for the authorization header  
**[time-tracking.time-entry (read & write)** | **time-tracking.time-entry.read (read only)]** 

### Sample query body

Do an [introspection query](../../graphql-concepts/introspection) to see the current schema for the Bill entity.
Here's an example query using every possible field. Remember, with GraphQL you only need to query for the data you need:

Sample query (Query Time Entries):
```
query readMinimalTimeSheet {
  timeTrackingTimeEntries(
    filter: {ids:[projectId1,projectId2]}
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


NA
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

You can choose to **query by dateRange** by including dateRange in the filter.

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