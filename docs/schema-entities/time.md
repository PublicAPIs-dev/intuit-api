---
layout: default
title: Bill
nav_order: 1
parent: Schema Entities
---

## Time

The APIs related to the Time entity allow you to manage Time so that you can track your Time entry for the project and employee.
The Time API provides support for create, read, update and delete operations.

### Operations for Bill entity

- Read - Query (POST)
- Create - Mutation (POST)
- Update - Mutation (POST)
- Delete - Mutation (POST)

### Endpoints

-   For production apps:  https://qb-e2e.api.intuit.com/graphql
-   For sandbox environments and testing: 

### Sample query header

-   Content-type: **application/json**
-   Use the bill scope **[com.intuit.quickbooks.accounting]** for the authorization header 

### Sample query body

Do an [introspection query](../../graphql-concepts/introspection) to see the current schema for the Bill entity.
Here's an example query using every possible field. Remember, with GraphQL you only need to query for the data you need:

Sample query (Query Time Entries):
```


```

Variables:

```
}
```

Response:
``` 

```

## Filter support:

You can choose to **query by dateRange** (as shown above) or query for all bills by removing the filter.

### Create mutation
 
Mutation:
 
```


```
 
Sample Variables: 
``` 

```    

Sample response:
```

```

### Update mutation

Mutation:

``` 

```
Required fields:
- id: ID of an existing bill
- metadata: you need to provide the entity version returned from a previous create/update/read operation. 

Variables:
```

```

Response:
```

```
### Delete Mutation

Mutation:

``` 

```

Required fields:
- id: ID of an existing bill
- metadata: you need to provide the entity version returned from a previous create/update/read operation. 

Variables:
``` 

```

Response:
```

```