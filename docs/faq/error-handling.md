---
layout: default
title: Error Handling
nav_order: 3
parent: FAQ
---

# Fix Intuit Ecosystem API errors

If you ever encounter an error, the Intuit Ecosystem API server returns an error message. These messages give you clues about the issue's source and cause. 

## Step 1: Capture the intuit_tid value
Server responses for gateway and service errors have an `intuit_tid` field in the header. 

Capture this field's value. It will help our support team quickly find and address reported issues.


## Step 2: Identify the error type 

When requests from apps go to our servers, they first pass through a gateway layer. Then they pass through the GraphQL Service layer. Thus, there are two general types: **gateway** errors and **service** errors. 

Review the HTTP Status Code (xxx). This tells you the error type:

* Service errors: **200** or **400** [List of possible GraphQL Error Code]
* Gateway errors: **All other ###**

## Step 3: Fix errors

### Fix gateway errors
Gateway errors are generated when requests hit our server's gateway layer. They apply to the whole request and follow the gateway error resposne format. 

```
HTTP Status Code 401
{
  "code": "AuthenticationFailed",
  "type": "INPUT",
  "message": null,
  "detail": "Malformed bearer token: too short or too long",
  "moreInfo": null
}
```
In the server response, review the following fields:

* HTTP Status Code - Gives you the error code
* `code` - Tells you what went wrong
* `detail` - Tells you what to fix

Many gateway errors are self-explanatory. Some require a bit more digging. For 401 errors, you may need to [update your app's access or refresh tokens](https://developer.intuit.com/app/developer/qbo/docs/develop/authentication-and-authorization/oauth-2.0). 

#### See all gateway errors
```
Status code - Error details
302 - Resource redirect or resource has moved
401 - Unauthenticated access: application authentication failed due to invalid or expired tokens
403 - Forbidden access: authorization specific, application authorization failed due to insufficient user access role
403 - Resource not found: routing error, access or configuration on the  Gateway, or incorrect endpoint requested
405 - Method not allowed: attempt to request other than GET/POST requests
429 - Too many requests: request is throttled as it exceeded the throttle policy.
500 - Internal Server Error: missing POST body or other exceptions within application, or a service outage
502 - Bad Gateway: Infrastructure misconfiguration, propagates response from downstream, or a service outage
503 - Service unavailable: Outage
504 - Service timeout: Outage

```

### Fix service errors
Service errors are generated when requests hit our server's service layer.

Service errors only apply to a subset of requests, not the entire request. Responses follow the standard GraphQL service format.

#### See all service errors
```
200 - Authorization errors due to incorrect scope, or partial semantic errors
400 - GraphQL validation errors such as invalid or malformed requests and missing variables, or system errors
```

Most service errors are in response to [malformed queries](../../graphql-concepts/query/), parsing problems, validation issues, or [incorrect scopes](../../getting-started/scopes/). As a result, the server won't authorize the request.

Here's an example of an HTTP status code 400:
```
{
  "errors": [
    {
      "message": "cannot query field 'offset' on type 'Common_PageInfo!'",
      "extensions": {
        "type": "Common_PageInfo!",
        "field": "offset",
        "code": "INVALID_FIELD"
      }
    }
  ]
}

```


Here's an example validation error:
```

```
Review the server response and use it as a guide. Each field gives clues for what failed, where, and how to fix it: 

* **Message** — Description of the error.
* **Extensions** — Additional info and error codes:
  * **code** - Specific error code.
  * **field** — Field that has the error. 
  * **type** — Entity that has the error. 

#### Error by error types

### EmployeeCompensation :
 
| Error Type                 | Error Code                 | Error Message                                                                                   | Error Source                                                  |
|----------------------------|----------------------------|-------------------------------------------------------------------------------------------------|---------------------------------------------------------------|
| SYSTEM_ERROR               | SYSTEM_ERROR               | Cannot return null for non-nullable field Payroll_EmployerCompensation.xx                       | When Pay types data is not available in the DB                |
| SERVICE_UNAVAILABLE        | SERVICE_UNAVAILABLE        | Service Unavailable                                                                             | When querying paytypes and downstream service not available   |
| AUTHENTICATION_ERROR       | AUTHENTICATION_ERROR       | Unauthorized                                                                                    | When Querying paytypes and dont have valid active credentials |
| VALIDATION_ON_FILTER_ERROR | EMPTY_VALUES_PASSED_ERROR  | Invalid filter input. Multiple filter operations specified, only a single operation is allowed. | When Querying paytypes with multiple filter options           |
| AUTHORIZATION_ERROR        | AUTHORIZATION_ERROR        | Forbidden                                                                                       | When Querying paytypes and dont have sufficient permissions   |
| PERMISSION_DENIED          | AUTHORIZATION_ERROR        | Forbidden                                                                                       | When Querying Compensations by not existing EmployeeId        |
| INVALID_REQUEST            | INVALID_REQUEST            | Could not find any employee with RealmId <RealmId> ExternalEmployeeId <Id>                      | Query PayTypes by not existing EmployeeId                     |
| INVALID_REQUEST            | INVALID_REQUEST            | Unable to find companyId for realmId=<someRealmId>                                              | Query PayTypes for a terminated company                       |
| VALIDATION_ERROR           | NOT_A_VALID_NUMBER_ERROR   | Invalid filter input. Please provide a valid number for employeeId                              | Query PayTypes by EmployeeId with invalid format              |
| VALIDATION_ERROR           | VALIDATION_ON_FILTER_ERROR | Invalid filter input. No values passed for the list                                             | Query PayTypes by Compensation Ids with empty list            |
| VALIDATION_ERROR           | NOT_A_VALID_NUMBER_ERROR   | Invalid filter input. Please provide a valid number for id                                      | Query PayTypes by Compensation Ids with Invalid format        |

### CustomField :

| Error Type       | Error Code                                    | Error Message                                                                                   | Error Source                                                                                                                                                                                                          |
|------------------|-----------------------------------------------|-------------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| VALIDATION_ERROR | V4_EXECUTION_FAILED                           | Failed to execute V4 request.                                                                   | There was an internal server error.  Please retry after a few mins.                                                                                                                                                   |
| SYSTEM_ERROR     | V4_MAPPING_ERROR                              | Failed to map V4 response object.                                                               | There was an internal server error.  Please retry after a few mins.                                                                                                                                                   |
| INVALID_REQUEST  | V4_ENTITY_CHECK_FAILED                        | Check Entity Response Failure                                                                   | There was an internal server error.  Please retry after a few mins.                                                                                                                                                   |
| INVALID_REQUEST  | EMPTY_VALUES_PASSED_ERROR                     | Invalid filter input. Multiple filter operations specified, only a single operation is allowed. | There was an internal server error.  Please retry after a few mins.                                                                                                                                                   |
| INVALID_REQUEST  | NA                                            | V4 Interaction Request is null.                                                                 | There was an internal server error.  Please retry after a few mins.                                                                                                                                                   |
| SYSTEM_ERROR     | V4_MISSING_RESPONSE_NODE                      | Property node is missing in edges.                                                              | Error while mapping the v4 response to CustomFieldDefinitionsConnection.                                                                                                                                              |
| SYSTEM_ERROR     | V4_MISSING_CFD_RESPONSE                       | customFieldDefinitions field is missing in response                                             | Error in getting the property company while mapping the v4 response to CustomFieldDefinitionsConnection                                                                                                               |
| SYSTEM_ERROR     | V4_MISSING_COMPANY_RESPONSE                   | Company field is missing in response                                                            | Error in getting the property customFieldDefinitions while mapping the v4 response to CustomFieldDefinitionsConnection                                                                                                |
 | SYSTEM_ERROR     | V4_MISSING_RESPONSE_DATA                      | Data field is missing in response                                                               | Error in getting the property data while mapping the v4 response to CustomFieldDefinitionsConnection(Method extractEdge not used)                                                                                     | 
 | SYSTEM_ERROR     | V4_MISSING_RESPONSE_KEY                       | %s Key field is missing in response                                                             | Error in getting any property while mapping the v4 response to CustomFieldDefinitionsConnection(Method extractEdge not used)                                                                                          | 
 | SYSTEM_ERROR     | V4_MISSING_RESOURCE                           | Resource is missing                                                                             | If any resource is missing in the write payload while creating a graphql entity for monolith call                                                                                                                     | 
 | VALIDATION_ERROR | V4_UNSUPPORTED_TYPE_REQUEST                   | request type is unsupported                                                                     | Whenever request type is not Write or Read, This is for internal execution flow                                                                                                                                       | 
 | SYSTEM_ERROR     | UNEXPECTED_ERROR                              | Some unknown error occurred                                                                     | This is thrown when error code is not passed, we can get rid of any such instances                                                                                                                                    | 
 | SYSTEM_ERROR     | INTERNAL_SERVER_ERROR                         | An internal server error has occurred.                                                          | There is a general exception.                                                                                                                                                                                         |
 | INVALID_REQUEST  | FORBIDDEN_ACCESS                              | Forbidden Request                                                                               | Not used( thrown in case of read one and readFromDB flag is not on)                                                                                                                                                   | 
 | INVALID_REQUEST  | PRINCIPAL_FETCH_FAILED                        | Failed to fetch principal                                                                       | This error would never show up for 3P customers, we can remove it                                                                                                                                                     | 
| VALIDATION_ERROR | CUSTOM_FIELD_LIMIT_EXCEEDED                   | Custom field limit exceeded                                                                     | validates count of custom fields based on sku for advanced effective count of cf's is no of cf's in db, for lower sku's effective count is calculated using the total number of parent entities like SALE or PURCHASE | 
 | VALIDATION_ERROR | CUSTOM_FIELD_ASSOCIATED_ENTITY_LIMIT_EXCEEDED | Content Message proposed by PM.                                                                 | Validates the maximum number of sub associated entity for custom fields                                                                                                                                               | 
 | VALIDATION_ERROR | ACTIVE_SUB_ASSOCIATION_REQUIRED               | Custom field missing entity condition                                                           | No the entity condition here refers to associations. If there is no active sub entity.                                                                                                                                | 
 | VALIDATION_ERROR | INVALID_SUB_ASSOCIATION                       | Custom field unsupported entity condition                                                       | No the entity condition here refers to associations. If there is no active sub entity which is not supported, for example any transaction that is not supported is sent in associations.                              | 
 | VALIDATION_ERROR | INVALID_DATA_TYPE                             | Invalid custom field definition data type                                                       | Data type is invalid, there is a set of enum for data type and using anything apart from this would result into this error.                                                                                           | 
| VALIDATION_ERROR | INVALID_ASSOCIATION                           | Invalid custom field definition associated entity                                               | validates mandatory fields in custom field definition associations and throws an exception when any entity is invalid. If associatedEntity not of type project, contact or transaction                                |                                                                        |                                                                                                                                                                                                                      |
| VALIDATION_ERROR | ITEM_REQUIRED_IN_SUB_ASSOCIATIONS             | Invalid custom field definition sub association                                                 | When sub association list is empty for any association                                                                                                                                                                | 
 | VALIDATION_ERROR | DUPLICATE_ASSOCIATION                         | Custom field definition duplicate association                                                   | If there are duplicate associations, eg contact type is passed two times                                                                                                                                              | 
 | VALIDATION_ERROR | DUPLICATE_SUB_ASSOCIATION                     | Custom field definition duplicate sub association                                               | If there are duplicate sub associations, eg sale_invoice is passed two times                                                                                                                                          | 
 | VALIDATION_ERROR | ACTIVE_ITEM_REQUIRED_IN_DROPDOWN              | Custom field definition dropdown type invalid active allowed values count                       | validates allowedValues list and throws an exception when allowed values count is invalid. Count is less than 2                                                                                                       | 
 | VALIDATION_ERROR | PRINT_OPERATION_ALLOWED_ONLY                  | Custom field definition invalid allowed operation                                               | allowedOperation is anything other than PRINT                                                                                                                                                                         | 
 | VALIDATION_ERROR | MISSING_ALLOWED_VALUES                        | Custom field definition value missing in dropdown allowed value                                 | No this is the case where dropdown list is added but there is no value passed or null value passed into the value field of dropdown list                                                                              | 
 | VALIDATION_ERROR | DUPLICATE_ALLOWED_VALUE                       | Custom field definition duplicate value in dropdown allowed value                               | Value field has duplicate value in allowed definition                                                                                                                                                                 | 
 | VALIDATION_ERROR | LABEL_REQUIRED                                | Custom field definition null or empty label                                                     | If label is null or empty                                                                                                                                                                                             | 
 | VALIDATION_ERROR | LABEL_ALREADY_EXISTS                          | Custom field definition duplicate label                                                         |                                                                                                                                                                                                                       | 
 | VALIDATION_ERROR | LABEL_LENGTH_EXCEEDED                         | Custom field definition label length exceeded                                                   |                                                                                                                                                                                                                       | 
 | VALIDATION_ERROR | INVALID_LABEL_CHARACTERS                      | Custom field definition label regex mismatch                                                    | This is an error when the label has some special characters or it doesn't matches the validations for naming a custom field. We can send back the regex in error saying that this should match the label              | 
 | VALIDATION_ERROR | DUPLICATE_ALLOWED_VALUE_ID                    | Custom field definition duplicate id in dropdown allowed value                                  | dropdown list is added but id field has duplicate values of dropdown list                                                                                                                                             | 
 | VALIDATION_ERROR | UNSUPPORTED_DATA_TYPE                         | Unsupported custom field definition data type for company sku                                   | Different SKUs has different data types supported for example, simple start only supports the text type of Custom field                                                                                               | 
 | VALIDATION_ERROR | ITEM_REQUIRED_IN_DROPDOWN                     | Custom field definition value no dropdown allowed values present                                | Yes, The error message also denotes the same.                                                                                                                                                                         | 
 | VALIDATION_ERROR | PRINT_NOT_SUPPORTED_IN_ALLOWED_OPERATION      | Custom field definition print limit exceeded for sub entity                                     | This is the case that PRINT is passed for sub associations(transactions) on which print is not supported                                                                                                              | 
 | VALIDATION_ERROR | PRINT_LIMIT_EXCEEDED_FOR_ENTITY               | Custom field definition print limit exceeded                                                    | No for any CF created, the user can specify "PRINT" only for a maximum of three transactions, i.e. "SALE", "SALE_INVOICE", "SALE_ESTIMATE".                                                                           | 
 | VALIDATION_ERROR | ID_REQUIRED                                   | Missing id in custom field definition                                                           | In case of update, id is not passed                                                                                                                                                                                   | 
 | VALIDATION_ERROR | MISSING_SCHEMA                                | Schema is null/missing                                                                          | Should not show up for 3P as we map the request fields to schema.                                                                                                                                                     | 
 | SYSTEM_ERROR     | CUSTOM_FIELD_SAVE_FAILED                      | Error saving Custom field definition                                                            | We can add error message to retry in these cases                                                                                                                                                                      | 
 |                  | VALIDATIONS_NOT_FOUND                         |                                                                                                 | If any error occurs in fetching the threshold limits from variability engine                                                                                                                                          | 
 | SYSTEM_ERROR     | ID_NOT_FOUND                                  | Custom field to be updated not found                                                            | Id for update entity doesn't exist                                                                                                                                                                                    | 
 | SYSTEM_ERROR     | UNEXPECTED_UPDATE_FAILED                      | Custom field update not supported                                                               | When legacyID v4 is not passed correctly for 3P, others when legacyId doesn't matches the existing CFDs                                                                                                               | 
 | SYSTEM_ERROR     | READ_ERROR                                    | Error reading Custom field definitions                                                          | We can add error message to retry in these cases                                                                                                                                                                      | 
 | INVALID_REQUEST  | PERSONA_ID_FETCH_FAILED                       | Failed to fetch personaId from principal                                                        | This error would never show up for 3P customers, we can remove it                                                                                                                                                     | 
 | VALIDATION_ERROR | FIELD_MISSING                                 | Field %s cannot be null or empty                                                                | This error is thrown when a field is null or empty                                                                                                                                                                    | 
 | VALIDATION_ERROR | ACTIVE_ITEM_REQUIRED_IN_SUB_ASSOCIATIONS      | Custom field missing active entity                                                              | If there are no active sub associations                                                                                                                                                                               | 
 | INVALID_REQUEST  | AUTHORIZATION_DENIED                          | Forbidden Access                                                                                | If user is not having allowed permission to make a request or insufficient privilege                                                                                                                                  | 
 | SYSTEM_ERROR     | AUTHORIZATION_FETCH_FAILED                    | Failed to fetch authz decision                                                                  | If there is connection timeout, or gateway timeout etc (system error)                                                                                                                                                 | 
 | SYSTEM_ERROR     | UNDETERMINED_AUTHORIZATION_DECISION           | AuthZ could not get a expected decision                                                         | If authZ response is indeterminate  or not applicable                                                                                                                                                                 | 
 | VALIDATION_ERROR | EXPIRED_QBO_SUBSCRIPTION                      |                                                                                                 |                                                                                                                                                                                                                       | 
 | SYSTEM_ERROR     | ENTITLEMENT_ERROR                             |                                                                                                 |                                                                                                                                                                                                                       | 


### Project error code :

| Error Code                                        | Error Message                                                                                                                                                               | 
|---------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DUPLICATE_PROJECT_NAME_FOR_CLIENT                 | Project name already exists                                                                                                                                                 | 
| INVALID_EMAIL_ADDRESS                             | Invalid Email , It should be valid and should follow the standard email format                                                                                              | 
| WORKFLOW_QUERY_JOB_NOT_FOUND                      | Job with id 300 not found                                                                                                                                                   | 
| WORKFLOW_INVALID_JOB_ID                           | Job id is invalid.                                                                                                                                                          | 
| WORKFLOW_QUERY_DATE_RANGE_ERROR                   | The date range Tue Oct 07 00:00:00 UTC 2025 to Fri Dec 06 00:00:00 UTC 2024 is invalid                                                                                      | 
| INVALID_STATUS                                    | Status %s is invalid                                                                                                                                                        | 
| ValidationError                                   | "Variable 'status' has an invalid value: Invalid input for Enum 'ProjectManagement_Status'. No value found for name 'OPENN                                                  | 
| INVALID_ASSIGNEE_ID                               | Assignee Id must be exactly 15 characters.                                                                                                                                  | 
| NULL_CLIENT_OII_ID                                | Client oii id must not be null.                                                                                                                                             | 
| INVALID_CLIENT_OII_ID                             | Client oii id must be exactly 15 characters.                                                                                                                                | 
| NULL_CLIENT_LOCAL_ID                              | Client local id must not be null.                                                                                                                                           | 
| INVALID_CLIENT_LOCAL_ID                           | Invalid client local id %s                                                                                                                                                  | 
| CLIENT_LOCAL_ID_NULL_FOR_SMB                      | For SMB, client local id must not be null                                                                                                                                   | 
| CLIENT_OII_ID_NULL_FOR_INSERVICETOTYPE_CONTACT    | For inService type Contact, Client oii id must not be null                                                                                                                  | 
| CLIENT_OII_ID_NOT_NULL_FOR_INSERVICETOTYPE_SELF   | For inService type Self, Client oii id must be null                                                                                                                         | 
| CLIENT_LOCAL_ID_NULL_FOR_INSERVICETOTYPE_CONTACT  | For inService type Contact, Client local id must not be null                                                                                                                | 
| CLIENT_LOCAL_ID_NOT_NULL_FOR_INSERVICETOTYPE_SELF | For inService type Self, Client local id must be null                                                                                                                       | 
| INSERVICETOTYPE_DEFAULT_ERROR                     | InService type Contact : Client oii id & local id cannot be null; for type SELF: Client oii id & local id must be null                                                      | 
| NULL_NAME                                         | Name must not be null.                                                                                                                                                      | 
| INVALID_PROJECT_NAME_WITH_COLON                   | Project name cannot include a colon.                                                                                                                                        | 
| INVALID_PROJECT_NAME_ONLY_WHITE_SPACES            | Project names cannot only contain white spaces.                                                                                                                             | 
| NULL_TEAM                                         | Team must not be null.                                                                                                                                                      | 
| INVALID_TEAM                                      | Team must be exactly 15 characters.                                                                                                                                         | 
| INVALID_PROJECT_COMPLETION_RATE_AMOUNT            | Project completion rate must be between 00.00 and 100.00                                                                                                                    | 
| ADDRESSLINE1_EXCEEDS_CHARACTER_LIMIT              | Exception while fetching data (/projectManagementCreateProject) : No enum constant com.intuit.qba.workflow.service.error.WorkflowError.ADDRESSLINE1_EXCEEDS_CHARACTER_LIMIT | 
| INVALID_DESCRIPTION_SIZE                          | Description must be between 0-1500 characters.                                                                                                                              | 
| INVALID_QUERY_ENTITY_ID_LIMIT                     | Project IDs exceeded the limit of %s                                                                                                                                        | 
| INVALID_NAME                                      | Name must be between 1-80 characters                                                                                                                                        | 
| INVALID_PRIORITY                                  | Project priority must be null or in range [0,9]                                                                                                                             | 
| AUTHZ_ERROR                                       | Something went wrong in AuthZ: %s                                                                                                                                           | 
| AUTHZ_VERIFY_ACCESS_ERROR                         | VerifyAccess DENY from AuthZ: %                                                                                                                                             | 
| AUTHZ_UNDETERMINDED_DECISION_ERROR                | AuthZ could not get a expected decision: %s                                                                                                                                 | 
| AUTHZ_DENIED_ERROR                                | AuthZ denied decision                                                                                                                                                       | 
| AUTHORIZATION_INVALID_TARGET_REALM                | Authorization failed: target realm id is invalid                                                                                                                            | 
| AUTHORIZATION_INVALID_CSRF                        | Authorization failed: CSRF is invalid for the realm                                                                                                                         | 
| DEFAULT_ERROR                                     | An error has occurred.  %s                                                                                                                                                  | 
| DataFetchingException                             | After cursor value shoud be null or base64 string value                                                                                                                     | 
| GRAPHQL_VALIDATION_FAILED                         | String cannot represent a non string value                                                                                                                                  | 
