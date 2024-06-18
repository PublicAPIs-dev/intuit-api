---
layout: default
title: EmployeeCompensation
nav_order: 1
parent: Schema Entities
---

## EmployeeCompensation

This resource allows your app to view the types of compensations provided from the employer to the employees. EmployeeCompensation allows you to determine which compensation types apply to a given employee. You can query a specific compensation type or all of them that apply.

### Operations for EmployeeCompensation entity

- Read - Query (POST)

### Endpoints

-   For production apps:  https://qb.api.intuit.com/graphql

### EmployeeCompensation Entry Fields

| #   | Field Name             | Data Type                          | Required | Field Definition                                                                          |
|-----|------------------------|------------------------------------|----------|-------------------------------------------------------------------------------------------|
| 1   | id                     | ID                                 |          | Unique identifier for the employee compensation.                                          |
| 2   | name                   | String                             |          | The name associated with this compensation.                                               |
| 3   | type                   |                                    |          | The corresponding compensation type.                                                      |
| 4   | type.key               | String                             |          | Employer compensation type key.                                                           |
| 5   | type.value             | String                             |          | Employer compensation type value.                                                         |

### Input Variables: 

| #   | Input Name             | Fields                             | Type | Field Definition                                                         |
|-----|------------------------|------------------------------------|------|--------------------------------------------------------------------------|
| 1   | employeeId             | String                             |      | The ID of the Employee. If passed in specific employeeId, the query will return all active compensations assigned/applicable to the employee |




### Sample query header

-   Content-type: **application/json**
-   Use the time scope for the authorization header: **[payroll.compensation.read (read only)]** 

### Sample query body

Do an [introspection query](../../graphql-concepts/introspection) to see the current schema for the EmployeeCompensation entity.
Here's an example query. Remember, with GraphQL you only need to query for the data you need:

Sample query (Query Time Entries):
```
query RequestEmployerInfo {
  payrollEmployerInfo {
    employerCompensations(filter: { employeeId: "1" }) {
      edges {
        node {
          id
          name
          type {
            key
            value
            description
          }
        }
      }
    }
  }
}



```

Variables:

```

```

Response:
``` 
{
    "data": {
        "payrollEmployerInfo": {
            "employerCompensations": {
                "edges": [
                    {
                        "node": {
                            "id": "1000027777",
                            "name": "Overtime Pay",
                            "type": {
                                "key": "OVERTIME",
                                "value": "OVERTIME",
                                "description": "OVERTIME"
                            }
                        }
                    },
                    {
                        "node": {
                            "id": "1000027778",
                            "name": "Double Overtime Pay",
                            "type": {
                                "key": "DOUBLE_OVERTIME",
                                "value": "DOUBLE_OVERTIME",
                                "description": "DOUBLE_OVERTIME"
                            }
                        }
                    },
                    {
                        "node": {
                            "id": "1000027783",
                            "name": "Holiday Pay",
                            "type": {
                                "key": "HOLIDAY_PAY",
                                "value": "HOLIDAY_PAY",
                                "description": "HOLIDAY_PAY"
                            }
                        }
                    },
                    {
                        "node": {
                            "id": "1000027784",
                            "name": "Bonus",
                            "type": {
                                "key": "BONUS",
                                "value": "BONUS",
                                "description": "BONUS"
                            }
                        }
                    },
                    {
                        "node": {
                            "id": "1000027785",
                            "name": "Commission",
                            "type": {
                                "key": "COMMISSION",
                                "value": "COMMISSION",
                                "description": "COMMISSION"
                            }
                        }
                    },
                    {
                        "node": {
                            "id": "1000027786",
                            "name": "Salary",
                            "type": {
                                "key": "SALARY",
                                "value": "SALARY",
                                "description": "SALARY"
                            }
                        }
                    }
                ]
            }
        }
    }
}

```

