---
layout: default
title: Mutation
nav_order: 2
parent: GraphQL Concepts
---

# Use mutations to modify data

Mutations are what apps use to perform operations to change, update, or delete data on the server.

In GraphQL, you can use mutations to modify multiple fields with a single request. If you're familiar with REST API, mutations are similar to the **POST**, **PUT**, and **DELETE** operations. 

While it's possible to create and update data with queries in GraphQL, it's best practice to use mutations to change or update data. And unlike queries, mutations run in series instead of in parallel. 

## How to use mutations 

For Intuit API, you'll use mutations often for things like timesheets, projects and custom fields. 

Here's a simple mutation to create a project:

```
mutation AppFoundationsCreateCustomFieldDefinition(
  $input: AppFoundations_CustomFieldDefinitionCreateInput!
) {
  appFoundationsCreateCustomFieldDefinition(input: $input) {
    label
    active
    associations {
      associatedEntity
      active
      associationCondition
      subAssociations {
        associatedEntity
        active
        allowedOperations
      }
    }
    dataType
    dropDownOptions {
      value
      active
      order
    }
  }
}

```
Mutations typically take input data and define fields based on the response the server returns. 

In this example, the mutation took the `deduction` argument and let the server define the `id` and `name` fields based on the response. 

Learn more about [mutations from GraphQL.org](https://graphql.org/learn/queries/#mutations).