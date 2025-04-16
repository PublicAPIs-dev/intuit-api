---
layout: default
title: EmployeeCompensation
nav_order: 1
parent: Schema Entities
---

## EmployeeCompensation

This resource allows your app to view the types of compensations provided from the employer to the employees. EmployeeCompensation allows you to determine which compensation types apply to a given employee. You can query a specific compensation type or all of them that apply.

### Operations for EmployeeCompensation entity

- [Read](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/employeeCompensation/#read-employeecompensationid) - Query (POST)

### Endpoints

-   For production apps:  https://qb.api.intuit.com/graphql

### EmployeeCompensation Entry Fields

| #   | Field Name           | Data Type                     | Required | Field Definition                                                  |
|-----|----------------------|-------------------------------|----------|-------------------------------------------------------------------|
| 1   | id                   | ID                            |          | Unique identifier for the employee compensation.                  |
| 2   | employerCompensation | Payroll_EmployerCompensation! |          | The employer compensation item associated with this compensation. |
| 3   | active               | Boolean!                      |          | Whether this compensation is currently active.                    |


### Payroll_EmployerCompensation

| #   | Field Name | Data Type                    | Required | Field Definition                            |
|-----|------------|------------------------------|----------|---------------------------------------------|
| 1   | id         | ID                           |          | The employer compensation id.               |
| 2   | name       | String!                      |          | The name associated with this compensation. |
| 3   | type       | Payroll_VariableStringField! |          | The corresponding compensation type.        |

### Payroll_VariableStringField

| #   | Field Name  | Data Type | Required | Field Definition                              |
|-----|-------------|-----------|----------|-----------------------------------------------|
| 1   | key         | String!   |          | Key of the variable string field.             |
| 2   | description | String!   |          | Key description of the variable string field. |
| 3   | value       | String!   |          | Value of the variable string field.           |

###  Payroll_EmployeeCompensationsFilter!

| #   | Field Name | Data Type | Required | Field Definition                                           |
|-----|------------|-----------|----------|------------------------------------------------------------|
| 1   | active     | Boolean!  |          | Filter active employee compensations.                      |
| 2   | employeeId | ID        |          | unique identifier of the employee to filter compensations. |


### Input Variables: 

| #   | Input Name | Fields   | Type | Field Definition                                                                                                                             |
|-----|------------|----------|------|----------------------------------------------------------------------------------------------------------------------------------------------|
| 1   | employeeId | String   |      | The ID of the Employee. If passed in specific employeeId, the query will return all active compensations assigned/applicable to the employee |
| 2   | active     | Boolean! |      | Whether this compensation is currently active.                                                                                               |


### Read EmployeeCompensationId 

Query Employee Compensation:

```
query getEmployeeCompensations($filter: Payroll_EmployeeCompensationsFilter!) {
  payrollEmployeeCompensations(filter: $filter ){
  	edges {
    	node {
      	  id
          active
      	  employerCompensation {
        id
           name
           type {
            key
            description
            value
           }
    	  }
  	    }
      }
    }
  }

```

Input Variable:

```
{
     "filter": {
        "employeeId": "1", 
        "active": true
    }
 }
 

```

Response:
``` 
{
    "data": {
        "payrollEmployeeCompensations": {
            "edges": [
                {
                    "node": {
                        "id": "624712157",
                        "active": true,
                        "employerCompensation": {
                            "id": "61897726",
                            "name": "OvrPay",
                            "type": {
                                "key": "OVERTIME",
                                "description": "OVERTIME",
                                "value": "OVERTIME"
                            }
                        }
                    }
                },
                {
                    "node": {
                        "id": "624712160",
                        "active": false,
                        "employerCompensation": {
                            "id": "61897727",
                            "name": "DblOvrPay",
                            "type": {
                                "key": "DOUBLE_OVERTIME",
                                "description": "DOUBLE_OVERTIME",
                                "value": "DOUBLE_OVERTIME"
                            }
                        }
                    }
                },
                {
                    "node": {
                        "id": "624712156",
                        "active": true,
                        "employerCompensation": {
                            "id": "61897728",
                            "name": "SickPay",
                            "type": {
                                "key": "SICK_PAY",
                                "description": "SICK_PAY",
                                "value": "SICK_PAY"
                            }
                        }
                    }
                },
                {
                    "node": {
                        "id": "624712164",
                        "active": true,
                        "employerCompensation": {
                            "id": "61897729",
                            "name": "VacPay",
                            "type": {
                                "key": "VACATION_PAY",
                                "description": "VACATION_PAY",
                                "value": "VACATION_PAY"
                            }
                        }
                    }
                },
                {
                    "node": {
                        "id": "624712158",
                        "active": true,
                        "employerCompensation": {
                            "id": "61897730",
                            "name": "PaidTO",
                            "type": {
                                "key": "PAID_TIME_OFF",
                                "description": "PAID_TIME_OFF",
                                "value": "PAID_TIME_OFF"
                            }
                        }
                    }
                },
                {
                    "node": {
                        "id": "624712159",
                        "active": true,
                        "employerCompensation": {
                            "id": "61897731",
                            "name": "UnpaidTO",
                            "type": {
                                "key": "UNPAID_TIME_OFF",
                                "description": "UNPAID_TIME_OFF",
                                "value": "UNPAID_TIME_OFF"
                            }
                        }
                    }
                },
                {
                    "node": {
                        "id": "624712162",
                        "active": true,
                        "employerCompensation": {
                            "id": "61897732",
                            "name": "HolidayPay",
                            "type": {
                                "key": "HOLIDAY_PAY",
                                "description": "HOLIDAY_PAY",
                                "value": "HOLIDAY_PAY"
                            }
                        }
                    }
                },
                {
                    "node": {
                        "id": "624712163",
                        "active": true,
                        "employerCompensation": {
                            "id": "61897733",
                            "name": "Bonus",
                            "type": {
                                "key": "BONUS",
                                "description": "BONUS",
                                "value": "BONUS"
                            }
                        }
                    }
                },
                {
                    "node": {
                        "id": "624712161",
                        "active": true,
                        "employerCompensation": {
                            "id": "61897734",
                            "name": "Commission",
                            "type": {
                                "key": "COMMISSION",
                                "description": "COMMISSION",
                                "value": "COMMISSION"
                            }
                        }
                    }
                },
                {
                    "node": {
                        "id": "624712172",
                        "active": true,
                        "employerCompensation": {
                            "id": "61897735",
                            "name": "Salary",
                            "type": {
                                "key": "SALARY",
                                "description": "SALARY",
                                "value": "SALARY"
                            }
                        }
                    }
                }
            ]
        }
    }
}

```



