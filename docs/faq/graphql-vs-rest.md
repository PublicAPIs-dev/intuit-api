---
layout: default
title: GraphQL vs REST
nav_order: 1
parent: FAQ
---

# GraphQL vs Rest


## REST vs GraphQL

If you’re familiar with our QuickBooks Online Accounting API (referred to as Accounting API for the remainder of this section), you may be used to the REST framework. In this section, we’ll go over some general differences between REST and GraphQL, and some differences between the Accounting API and the Ecosystem API.

### HTTP request type

In REST, you would do a GET request to retrieve data from the server and a POST, PATCH PUT or DELETE to modify data on the server.

  

In GraphQL, all requests are POSTs and the body of the request tells the server what needs to be done. All of the HTTP calls are made the same way and to the same endpoint but with different request bodies. To retrieve data, you would use a query operation, and to modify data you would use a mutation operation. More on those operations [here](../../graphql-concepts).

### Single endpoint vs multiple endpoints

Let’s say you wanted to read an invoice and find out how that invoice was paid. In REST, these would typically be separate resources or entities called Invoice and Payment, which is what the Accounting API does. You would have to make one call to the Invoice endpoint to get the invoice information. You would then find the related Payment ID and then make another call to Payment endpoint to find out more information about the payment transaction.

  

If you were to do this in GraphQL, you would be dealing with a single endpoint. Depending on the schema, you would make a call to one endpoint and ask for the Invoice fields you need and the fields on the related Payment.

### Fetching data

The Accounting API would return all the information it has about an entity that is queried. A simple read or query operation would return all the fields associated with the entity for the record that has been requested.

  

The Ecosystem API gives you more control over what data you want from the server. Whether you are doing a query or a mutation, you can specify exactly which fields (and nested fields) you want from the server.

  

### Schema updates

In REST, schema updates may be notified to end-users via webhooks or release notes. GraphQL does this with the help of the [introspection query](../../graphql-concepts/introspection).

### Identifiers

In the Accounting API, when you query for records, each record comes with an ID, which is unique only within that entity. So you may have an Invoice with ID 123 and a Payment with ID 123.

The ID's returned by the Intuit API are interoperable with the Accounting Rest API.

### Error response

The Accounting API returns HTTP status codes like 4** or 5** to indicate different types of errors.

  

The GraphQL API handles errors differently. Errors related to the API gateway later will return status codes 4** or 5**, but API errors will return HTTP status code 200 and provide all of the necessary information within the response body. Refer to [this section](../../graphql-concepts/error-handling-best-practices) for more details.

### Operations

  

Let’s take a deeper dive into some common operations like create, read, update, delete, and query. We’ll take the example of the Accounting API’s Invoice entity for this section. An Invoice represents a sales form where the customer pays for a product or service later. It is associated with Items, Customers, and Payments. Although Invoice is not supported in the Ecosystem API, we’ll do a comparison between how we work with this entity in the Accounting API vs how we would hypothetically do this in the Ecosystem API.

  

In all of the snippets below you will see that in the Accounting API (REST), the response will be the object with all of the fields. In the GraphQL API, however, you can specify which fields you want and only those fields are a part of the response.

  

Note that these are illustrative examples only. For exact field names and operations, please refer to the Accounting API Reference and GraphQL introspection.

### Create an invoice
### REST
Request:
POST `<baseUrl>/v3/company/<realmID>/invoice`
```
{
  "Line": [
    {
      "DetailType": "SalesItemLineDetail", 
      "Amount": 100.0, 
      "SalesItemLineDetail": {
        "ItemRef": {
          "name": "Services", 
          "value": "1"
        }
      }
    }
  ], 
  "CustomerRef": {
    "value": "1"
  }
}
```

Response:
```
{
  "Invoice": {
    "Id": "238", 
    "DocNumber": "1069",
    "Balance": 100.0, 
    "TxnDate": "2020-07-24", 
    "TotalAmt": 100.0, 
    "CustomerRef": {
      "name": "Amy's Bird Sanctuary", 
      "value": "1"
    }, 
    "ShipAddr": {
      "City": "Bayshore", 
      "Line1": "4581 Finch St.", 
      "PostalCode": "94326", 
      "CountrySubDivisionCode": "CA", 
      "Id": "109"
    }, 
    "DueDate": "2020-08-23", 
    "Line": [
      {
        "Amount": 100.0, 
        "SalesItemLineDetail": {
          "TaxCodeRef": {
            "value": "NON"
          }, 
          "ItemRef": {
            "name": "Services", 
            "value": "1"
          }
        }, 
        "DetailType": "SalesItemLineDetail"
      }
    ], 
	... other fields ... 
}
```


### GraphQL
Request:
POST `<graphBaseURL>/v1`
```
mutation Create{
  createInvoice(item: {type: "inventory", amount: 100, itemref: "111"}, customerRef: "222", paymentRef: "333") {
    id
    docNumber
  }
}
```

Response:
```
{
	"id": 1,
	"docNumber": "101"
}
```


The above are only illustrative examples. To get a full list of operations and resources available in the Grapqh API, use the API reference docs [here](). You can also refer to our collections for Insomnia and Postman [here](). 
