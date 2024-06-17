---
layout: default
title: CustomField
nav_order: 1
parent: Schema Entities
---

## Time

The APIs related to the Time entity allow you to manage Time so that you can track your Time entry for the project and employee.
The Time API provides support for create, read, update and delete operations.

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
    	legacyID
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
        	"__typename": "AppFoundations_CustomFieldDefinitionsConnection",
        	"edges": [
            	{
                	"__typename": "AppFoundations_CustomFieldDefinitionEdge",
                	"node": {
                    	"__typename": "AppFoundations_CustomFieldDefinition",
                    	"id": "udcf_1000000001",
                    	"legacyID": "djQ6OTM0MTQ1MjE3MjU1MzIyOTovY29tbW9uL0N1c3RvbUZpZWxkRGVmaW5pdGlvbjo:286432",
                    	"label": "supergraph-create-0509",
                    	"associations": [
                        	{
                            	"__typename": "AppFoundations_CustomExtensionAssociations",
                            	"associatedEntity": "/transactions/Transaction",
                            	"active": true,
                            	"validationOptions": {
                                	"__typename":     "AppFoundations_CustomExtensionValidationOptions",
                                	"required": false
                            	},
                            	"allowedOperations": [],
                            	"associationCondition": "INCLUDED",
                            	"subAssociations": [
                                	{
                                    	"__typename": "AppFoundations_CustomExtensionSubAssociation",
                                    	"associatedEntity": "SALE_INVOICE",
                                    	"active": true,
                                    	"allowedOperations": []
                                	}
                            	]
                        	}
                    	],
                    	"dataType": "STRING",
                    	"createdSource": null,
                    	"dropDownOptions": [],
                    	"active": true,
                    	"customFieldDefinitionMetaModel": {
                        	"__typename": "AppFoundations_CustomFieldDefinitionMetaModel",
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
	id
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
 
Input Variables: 
``` 
{
  "input": {
	"label": "sample-custom-lablel-0509",
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
        	"id": "udcf_1000000018",
        	"label": "sample-custom-lablel-0509",
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
        	"createdSource": null,
        	"dropDownOptions": [],
        	"active": true
    		}
	}
}

```

### Update Custome Field

Mutation:

``` 
mutation AppFoundationsUpdateCustomFieldDefinition($input: AppFoundations_CustomFieldDefinitionUpdateInput!) {
  appFoundationsUpdateCustomFieldDefinition(input: $input) {
	id
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
    	"id": "djQ6OTM0MTQ1MjE3MjU1MzIyOTovY29tbW9uL0N1c3RvbUZpZWxkRGVmaW5pdGlvbjo:287780",
    	"label": "customfield-0613",
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
        	"id": "udcf_12",
        	"label": "customfield-0613",
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