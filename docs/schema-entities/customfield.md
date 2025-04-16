---
layout: default
title: CustomField
nav_order: 1
parent: Schema Entities
---

## Custom Field

The APIs related to the Custom Fields allow you to manage and sync custom fields into QuickBooks Online. 
The Custom Fields API provides support for create, read, update, and disable operations. 
When creating a Custom Field, you can create associations with entities. 
You can also add custom fields to transactions and other entities by configuring the custom field definition ID while creating the transaction.

### Operations for Custom Fields entity

- [Read](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/customfield/#read-custom-fields) - Query (POST)
- [Create](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/customfield/#create-custom-field) - Mutation (POST)
- [Update](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/customfield/#update-custom-field) - Mutation (POST)
- [Disable](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/customfield/#disable-custom-field) -Mutation (POST)


### API Reference

| Field                          | Type                                                 | Required | Description                                                                                                               |
|--------------------------------|------------------------------------------------------|----------|---------------------------------------------------------------------------------------------------------------------------|
| id                             | ID!                                                  | yes      | Id of the Custom Field Definition.                                                                                        |
| legacyIDV2                     | ID!                                                  | yes      | Unique numeric identifier of the Custom Field Definition used in QBO 3P REST APIs.                                        |
| label                          | String                                               | no       | Label of the Custom Field Definition.                                                                                     |
| associations                   | [AppFoundations_CustomExtensionAssociations!]!       | yes      | All the associations and their properties.                                                                                |
| dataType                       | AppFoundations_CustomExtensionDataType!              | yes      | Data type of the Custom Extension Definition. For example: String, Number.                                                |
| dropDownOptions                | [AppFoundations_CustomFieldDefinitionDropDownOption] | no       | drop down options, applicable when dataType is a list.                                                                    |
| active                         | Boolean                                              | no       | indicates whether this CF is still active.                                                                                |
| customFieldDefinitionMetaModel | AppFoundations_CustomFieldDefinitionMetaModel        | no       | used during downgrade flow to indicate if a custom field needs to be inactivated due to missing support in the target sku |

### Type AppFoundations_CustomExtensionAssociations

| Field                | Type                                               | Required | Description                                                                                                                                                  |
|----------------------|----------------------------------------------------|----------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| associatedEntity     | String!                                            | yes      | entity associated.For example: transaction.                                                                                                                  |
| active               | Boolean                                            | yes      | indicates whether this entity association is active.                                                                                                         |
| validationOptions    | AppFoundations_CustomExtensionValidationOptions    | no       | indicates the validation options on this entity.                                                                                                             |
| allowedOperations    | [AppFoundations_CustomExtensionAssociations!]!     | yes      | indicates what possible operations can be done. For example: Search, Print.                                                                                  |
| associationCondition | AppFoundations_CustomExtensionAssociationCondition | yes      | indicates whether this entity is part of an inclusion list or exclusion list. For exmaple: dimension will have EXCLUDED list, and CF will have INCLUDED list |
| subAssociations      | [AppFoundations_CustomExtensionSubAssociation!]    | yes      | all sub associations for this entity.                                                                                                                        |

### Type AppFoundations_CustomExtensionSubAssociation

| Field             | Type                                             | Required | Description                                                                 |
|-------------------|--------------------------------------------------|----------|-----------------------------------------------------------------------------|
| associatedEntity  | String!                                          | yes      | sub entity associated. For example: invoice.                                |
| active            | Boolean                                          | no       | indicates whether this sub entity is active.                                |
| validationOptions | AppFoundations_CustomExtensionValidationOptions  | no       | indicates the validation options on this entity.                            |
| allowedOperations | [AppFoundations_CustomExtensionAllowedOperation] | no       | indicates what possible operations can be done. For example: Search, Print. |

### AppFoundations_CustomExtensionDataType

```
enum AppFoundations_CustomExtensionDataType {
    UNKNOWN
    STRING
    NUMBER
    DATE
    OBJECT_LIST
    STRING_LIST
}

```

### AppFoundations_CustomFieldDefinitionDropDownOption

| Field  | Type    | Required | Description                                    |
|--------|---------|----------|------------------------------------------------|
| id     | ID!     | yes      | id of the option.                              |
| value  | String! | yes      | value of the option.                           |
| active | Boolean | no       | indicates whether this option is still active. |
| order  | Int     | yes      | indicates the creation order of this option.   |


### AppFoundations_CustomFieldDefinitionMetaModel

| Field     | Type    | Required | Description                                                                                           |
|-----------|---------|----------|-------------------------------------------------------------------------------------------------------|
| suggested | Boolean | no       | this field being true indicates that the CF is unsupported in target sku and needs to be inactivated. |


### AppFoundations_CustomExtensionValidationOptions

| Field    | Type    | Required | Description                                |
|----------|---------|----------|--------------------------------------------|
| required | Boolean | no       | indicates whether this entity is required. |


### AppFoundations_CustomExtensionAssociationCondition

```
enum AppFoundations_CustomExtensionAssociationCondition {
    UNKNOWN
    INCLUDED
    EXCLUDED
}
```

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