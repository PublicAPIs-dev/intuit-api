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


### Read EmployeeCompensationId 

Query Employee Compensation:

```
query getEmployeeCompensationById {
  payrollEmployerInfo {
	employerCompensations (filter: {employeeId: "4"}){
  	edges {
    	node {
      	id
      	alternateIds {
        	id
      	}
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

Response:
``` 
{
    "data": {
        "payrollEmployerInfo": {
            "employerCompensations": {
                "edges": [
                    {
                        "node": {
                            "id": "1000029752",
                            "alternateIds": [
                                {
                                    "id": "djQuMTo5MzQxNDUyMTk5MDE3Mzc1OjI4ZDA3MTdlOTY:1000029752"
                                }
                            ],
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
                            "id": "1000029753",
                            "alternateIds": [
                                {
                                    "id": "djQuMTo5MzQxNDUyMTk5MDE3Mzc1OjI4ZDA3MTdlOTY:1000029753"
                                }
                            ],
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
                            "id": "1000029758",
                            "alternateIds": [
                                {
                                    "id": "djQuMTo5MzQxNDUyMTk5MDE3Mzc1OjI4ZDA3MTdlOTY:1000029758"
                                }
                            ],
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
                            "id": "1000029761",
                            "alternateIds": [
                                {
                                    "id": "djQuMTo5MzQxNDUyMTk5MDE3Mzc1OjI4ZDA3MTdlOTY:1000029761"
                                }
                            ],
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

