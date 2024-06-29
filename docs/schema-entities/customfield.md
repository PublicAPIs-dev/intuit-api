---
layout: default
title: CustomField
nav_order: 1
parent: Schema Entities
---

## Custom Field

The APIs related to the Custom Fields allow you to manage and sync custom fields into QuickBooks Online. The Custom Fields API provides support for create, read, update, and disable operations. You can also add custom fields to transactions and other entities by configuring the custom field definition ID while creating the transaction.


### API Reference

<table>
  <tr>
   <td><strong>Field</strong>
   </td>
   <td><strong>DataType</strong>
   </td>
   <td><strong>Required</strong>
   </td>
   <td><strong>Description</strong>
   </td>
  </tr>
  <tr>
   <td>id
   </td>
   <td>String
   </td>
    <td>Required
   </td>
   <td>Id of the Custom Field definition.
   </td>
  </tr>
   <tr>
   <td>legacyIDV2
   </td>
   <td>String
   </td>
    <td>Required
   </td>
   <td>Unique numeric identifier of the Custom Field Definition used in QBO 3P REST APIs
   </td>
  </tr>
   <tr>
   <td>associations
   </td>
   <td>
   </td>
    <td>Required
   </td>
   <td>Custom Extension Associations: All the associations and their properties
   </td>
  </tr>
  <tr>
   <td>associations.associatedEntity
   </td>
   <td>String
   </td>
    <td>Required
   </td>
   <td>entity associated.For example: transaction (SALE,
	SALE_INVOICE,
	SALE_ESTIMATE,
	SALE_CREDIT,
	SALE_REFUND, PURCHASE_ORDER,
	PURCHASE,
	PURCHASE_BILL,
PURCHASE_CHECK,
PURCHASE_CREDIT,
PURCHASE_CREDIT_CARD_CREDIT)
   </td>
  </tr>
    <tr>
   <td>dataType
   </td>
   <td>String
   </td>
    <td>Required
   </td>
   <td>Data type of the Custom Extension Definition. For example: String, Number
   </td>
  </tr>
   <tr>
   <td>dropDownOptions.id
	</td>
   <td>String
   </td>
    <td>Required
   </td>
   <td>Id of the option
   </td>
  </tr>
  <tr>
   <td>dropDownOptions.valu
   </td>
   <td>String
   </td>
    <td>Required
   </td>
   <td>Value of the option
   </td>
  </tr>
</table>


### Read Custom Fields

Sample query (Query Time Entries):
```
query {
  appFoundationsCustomFieldDefinitions {
	edges {
  	node {
    	id
    	legacyIDV2
    	label
    	associations {
      	associatedEntity
      	active
      	validationOptions {
        	required
      	}
      	allowedOperations
      	associationCondition
      	subAssociations {
        	associatedEntity
        	active
        	allowedOperations
      	}
    	}
    	dataType
    	dropDownOptions {
      	id
      	value
      	active
      	order
    	}
    	active
    	customFieldDefinitionMetaModel {
      	suggested
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
    	"appFoundationsCustomFieldDefinitions": {
        	"edges": [
            	{
                	"node": {
                    	"id": "udcf_5",
                    	"legacyIDV2": "1149622",
                    	"label": "cf-03",
                    	"associations": [
                        	{
                            	"associatedEntity": "/transactions/Transaction",
                            	"active": true,
                            	"validationOptions": {
                                	"required": false
                            	},
                            	"allowedOperations": [],
                            	"associationCondition": "INCLUDED",
                            	"subAssociations": [
                                	{
                                    	"associatedEntity": "SALE_INVOICE",
                                    	"active": true,
                                    	"allowedOperations": []
                                	}
                            	]
                        	}
                    	],
                    	"dataType": "STRING",
                    	"dropDownOptions": [],
                    	"active": true,
                    	"customFieldDefinitionMetaModel": {
                        	"suggested": null
                    	}
                	}
            	}
        	]
    	}
	}
}


```

### Create Custom Field
Mutation:
 
```
mutation AppFoundationsCreateCustomFieldDefinition($input: AppFoundations_CustomFieldDefinitionCreateInput!) {
  appFoundationsCreateCustomFieldDefinition(input: $input) {
	label
	active
	associations {
  	associatedEntity
  	active
  	validationOptions {
    	required
  	}
  	allowedOperations
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
 
Input Variables: 
``` 
{
  "input": {
	"label": "CF-customerType2",
	"associations": [
  	{
    	"validationOptions": {
    	"required": false
    	},
    	"associatedEntity": "/transactions/Transaction" ,
    	"active": true,
    	"allowedOperations": [],
    	"associationCondition": "INCLUDED",
    	"subAssociations": [
      	{
        	"associatedEntity": "SALE_INVOICE",
        	"active": true,
        	"allowedOperations": []
      	}
    	]
  	},
  	{
    	"associatedEntity": "/network/Contact",
    	"active": true,
    	"validationOptions": {
        	"required": false
    	},
    	"allowedOperations": [],
    	"associationCondition": "INCLUDED",
    	"subAssociations": [
        	{
            	"associatedEntity": "CUSTOMER",
            	"active": true,
            	"allowedOperations": []
        	}
    	]
	}  
	],
	"dataType": "STRING",
	"active": true
  }
}

```    

Sample response:
```
{
	"data": {
    	"appFoundationsCreateCustomFieldDefinition": {
        	"label": "CF-customerType2",
        	"active": true,
        	"associations": [
            	{
                	"associatedEntity": "/transactions/Transaction",
                	"active": true,
                	"validationOptions": {
                    	"required": false
                	},
                	"allowedOperations": [],
                	"associationCondition": "INCLUDED",
                	"subAssociations": [
                    	{
                        	"associatedEntity": "SALE_INVOICE",
                        	"active": true,
                        	"allowedOperations": []
                    	}
                	]
            	},
            	{
                	"associatedEntity": "/network/Contact",
                	"active": true,
                	"validationOptions": {
                    	"required": false
                	},
                	"allowedOperations": [],
                	"associationCondition": "INCLUDED",
                	"subAssociations": [
                    	{
                        	"associatedEntity": "CUSTOMER",
                        	"active": true,
                        	"allowedOperations": []
                    	}
                	]
            	}
        	],
        	"dataType": "STRING",
        	"dropDownOptions": []
    	}
	}
}

```

### Update Custom Field

Mutation:

``` 
mutation AppFoundationsUpdateCustomFieldDefinition($input: AppFoundations_CustomFieldDefinitionUpdateInput!) {
  appFoundationsUpdateCustomFieldDefinition(input: $input) {
	id
	legacyIDV2
	label
	associations {
  	associatedEntity
  	active
  	validationOptions {
    	required
  	}
  	allowedOperations
  	associationCondition
  	subAssociations {
    	associatedEntity
    	active
    	allowedOperations
  	}
	}
	dataType
	dropDownOptions {
  	id
  	value
  	active
  	order
	}
	active
  }
}

```


Variables:
```
{
  "input": {
    	"id": "udcf_1",
    	"legacyIDV2": "1149549",
    	"label": "cf-01",
    	"active": false,
    	"associations": [
  	{
    	"validationOptions": {
    	"required": true
    	},
    	"associatedEntity": "/transactions/Transaction" ,
    	"active": true,
    	"allowedOperations": [],
    	"associationCondition": "INCLUDED",
    	"subAssociations": [
      	{
        	"associatedEntity": "SALE_INVOICE",
        	"active": true,
        	"allowedOperations": []
      	},
      	{
        	"associatedEntity": "SALE_ESTIMATE",
        	"active": true,
        	"allowedOperations": []
      	}
    	]
  	}
	]
	}
}

```

Response:
```
{
	"data": {
    	"appFoundationsUpdateCustomFieldDefinition": {
        	"id": "udcf_1",
        	"legacyIDV2": "1149549",
        	"label": "cf-01",
        	"associations": [
            	{
                	"associatedEntity": "/transactions/Transaction",
                	"active": true,
                	"validationOptions": {
                    	"required": false
                	},
                	"allowedOperations": [],
                	"associationCondition": "INCLUDED",
                	"subAssociations": [
                    	{
                        	"associatedEntity": "SALE_INVOICE",
                        	"active": true,
                        	"allowedOperations": []
                    	},
                    	{
                        	"associatedEntity": "SALE_ESTIMATE",
                        	"active": true,
                        	"allowedOperations": []
                    	}
                	]
            	}
        	],
        	"dataType": "STRING",
        	"dropDownOptions": [],
        	"active": false
    	}
	}
}

```
### Disable Custom Field

Mutation:

``` 
mutation AppFoundationsUpdateCustomFieldDefinition($input: AppFoundations_CustomFieldDefinitionUpdateInput!) {
  appFoundationsUpdateCustomFieldDefinition(input: $input) {
	id
	legacyIDV2
	label
	associations {
  	associatedEntity
  	active
  	validationOptions {
    	required
  	}
  	allowedOperations
  	associationCondition
  	subAssociations {
    	associatedEntity
    	active
    	allowedOperations
  	}
	}
	dataType
	createdSource
	dropDownOptions {
  	id
  	value
  	active
  	order
	}
	active
  }
}


```

Variables:
``` 
{
  "input": {
    	"id": "udcf_1",
    	"legacyIDV2": "1149549",
    	"label": "cf-01",
    	"active": false,
    	"associations": [
  	{
    	"validationOptions": {
    	"required": true
    	},
    	"associatedEntity": "/transactions/Transaction" ,
    	"active": false,
    	"allowedOperations": [],
    	"associationCondition": "INCLUDED",
    	"subAssociations": [
      	{
        	"associatedEntity": "SALE_INVOICE",
        	"active": true,
        	"allowedOperations": []
      	},
      	{
        	"associatedEntity": "SALE_ESTIMATE",
        	"active": true,
        	"allowedOperations": []
      	}
    	]
  	}
	]
	}
}

```

Response:

```
{
	"data": {
    	"appFoundationsUpdateCustomFieldDefinition": {
        	"id": "udcf_1",
        	"legacyIDV2": "1149549",
        	"label": "cf-01",
        	"associations": [
            	{
                	"associatedEntity": "/transactions/Transaction",
                	"active": false,
                	"validationOptions": {
                    	"required": false
                	},
                	"allowedOperations": [],
                	"associationCondition": "INCLUDED",
                	"subAssociations": [
                    	{
                        	"associatedEntity": "SALE_INVOICE",
                        	"active": true,
                        	"allowedOperations": []
                    	},
                    	{
                        	"associatedEntity": "SALE_ESTIMATE",
                        	"active": true,
                        	"allowedOperations": []
                    	}
                	]
            	}
        	],
        	"dataType": "STRING",
        	"createdSource": null,
        	"dropDownOptions": [],
        	"active": false
    	}
	}
}


```