---
layout: default
title: GraphQL best practices
nav_order: 8
parent: GraphQL Concepts
---

# Adopt Intuit API best practices

GraphQL is a powerful and efficient API language. Here are a few ways you can structure and craft your code to build world-class apps.

For more tips, [visit GraphQL.org](https://graphql.org/learn/best-practices/). 

## GraphQL best practices

### Only query for the data your app needs

In GraphQL, less is more. Create queries that only request the specific fields and data required for your QuickBooks integration to be fully functional. You can change queries later and add more fields as needed. 


### Use aliases to query the same field with different arguments

[Aliases](https://graphql.org/learn/queries/#aliases) let you rename the results of identical object fields so you can query the same field for different arguments. You can pass multiple queries at once and get responses for both fields without conflict. 
 
### Always include operation names to avoid confusion

Consciously [use operation names](https://graphql.org/learn/queries/#operationname) to make your code crystal clear, concrete, and unambiguous. 
 
### Create fragments to speed up code writing

Instead of querying frequently used fields individually, [create and reuse fragments](../fragments/) whenever possible. This makes code modular and easier to scale. 
 
### Take advantage GraphQL's unique features to get more done

[Pagination](../pagination/) lets you quickly navigate large datasets. If you expect a query to return lots of data, use pagination to fetch specific subsets. 


## Advantages of GraphQL

### Single endpoints

Let's say you want to build an invoicing app. It needs to constantly request data for a specific set of invoices. The invoices contain particular items for a specific date range. And, the app needs current business addresses for each customer. 

With REST API, you’d need to make several queries to different endpoints: one for the invoice, one for each customer, and one for each item.

GraphQL only requires a single query, [sent to a single endpoint](../../getting-started/endpoints/). 

### Get only the data you need

GraphQL queries are highly customizable. You specify the exact fields and values you want the server to return. Apps only get the data you query for - there’s no over-fetching or under-fetching.

### Develop with any language
GraphQL is language-independent. You can use it with any backend framework or programming language implementation.

### Validate queries as you work

Since GraphQL is a type system, everything about it is part of its schema. You can validate queries as you create them with [testing tools like GraphiQL](../../getting-started/graphql-ide/) (which has a built-in parser). You can also let the server validate requests against the current schema version.

### Everything is documented

Accurate and up-to-date documentation is always available along with the schema. 

### Simple data hierarchies

GraphQL requests and responses are structured the same way. It naturally follows relationships between attributes.
