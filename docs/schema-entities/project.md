---
layout: default
title: Project
nav_order: 1
parent: Schema Entities
---

## Project

The APIs related to the Project entity allow you to manage and track projects. The Project API provides support for create, read, update, delete, and recover operations. You can also add transactions to your project by configuring the ID for the project while creating the transaction.

### Operations supported for Project                
                                              
- [Read](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#read-project) - Query (POST)                         
- [Create](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#create-project) - Mutation (POST)                    
- [Update](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#update-project) - Mutation (POST)                    
- [Delete](https://intuitdeveloper.github.io/intuit-api/docs/schema-entities/project/#delete-project) - Mutation (POST)                    

We have also added support to the below transactions so  that you can configure a project ID. This will allow you to read back information about the project to which this transaction belongs to (if any). These include a few transactions like -
To do this you can make use of the "ProjectRef" field in the V3 entity which is available starting with minorVersion=69)

- Project Cost Estimate (enhanced version of V3 estimate) 
    - New fields available - `Line.CostAmount`, `Line.HomeCostAmount`, `Line.SalesItemLine.SalesItemLineDetail.UnitCostPrice`
- Invoice
- Receive Payment 
- Bill
- TimeEntry
- Expenses 
- Purchase Order
- Credit Memo
- Refund Receipt
- Sales Receipt
                                                                                                                           
### ProjectRef                                                                                                                       
Developer docs for reference: https://developer.intuit.com/app/developer/qbo/docs/api/accounting/most-commonly-used/invoice 
         
![](/intuit-api/assets/images/ProjectRef.png)       

### CRUD Endpoint

-   For production apps:  https://qb.api.intuit.com/graphql

### Project Fields

| Field Name        | Type                                       | Required      | 	Description                                                                                                                            |
|-------------------|--------------------------------------------|---------------|-----------------------------------------------------------------------------------------------------------------------------------------|
| name              | String!                                    | Yes	          | The name of the project.                                                                                                                |
| description       | String                                     | No            | The description of the project.                                                                                                         |
| type              | String                                     | No  (Optional) | The type of the project that user can define to differentiate between different types of projects and filter on them.                   |
| status	         | ProjectManagement_Status                   | No            | The status of the project indicating the current state of the project. Default status is "OTHER"                                        |
| priority          | Int                                        | No (Optional) | The priority of the project in the range of 0-9 where 0 is the lowest and 9 is the highest priority and filter on them.                 |
| customer          | ProjectManagement_CustomerInput            | Yes	          | The customer for whom the project is created.                                                                                           |
| account           | ProjectManagement_CompanyInput             | Yes	          | The company for whom the project is created.                                                                                            |
| dueDate           | DateTime                                   | No            | The due date of the project.                                                                                                            |
| startDate         | DateTime                                   | No            | The start date of the project.                                                                                                          |
| completedDate	 | DateTime                                   | No            | The completed date of the project.                                                                                                      |
| completedBy       | ProjectManagement_UserInput                | No            | The user completed the project.                                                                                                         |
| completionRate    | Decimal                                    | No            | The rate of completion of project.                                                                                                      |
| pinned            | Boolean                                    | No            | Pinned is used to tell if a project should be shown at the top of the projects list above those that are not pinned.                    |
| emailAddress      | [Qb_EmailAddressInput]                     | No            | The email address of the project.                                                                                                       |
| addresses         | [Qb_PostalAddressInput]                    | No            | The addresses of the project.                                                                                                           |

### ProjectManagement_Status
```
""" The list of allowed status values of the project indicating the current state of the project """
""" If the status is not set default to "OTHER" """
enum ProjectManagement_Status {
    OPEN
    IN_PROGRESS
    BLOCKED
    CANCELED
    COMPLETE
    OTHER
}
```
 
### Input Variables: 

### ProjectManagement_CustomerInput 

```
input ProjectManagement_CustomerInput {
    """
    The customer Id for whom the project is created.
    id: ID!
}
```

### ProjectManagement_CompanyInput
```
input ProjectManagement_CompanyInput {
    """
    The companyId(realmId) for whom the project is created. 
    id: ID!
}
```
### ProjectManagement_UserInput

```
input ProjectManagement_UserInput {
    """
    The user completed the project.
    id: ID!
}
```

### Qb_EmailAddressInput

````
input Qb_EmailAddressInput  {
    """
    full email address
    """
    email: String! #@pattern(regex: ".+@.+\\..+")
    """
    the name before @
    """
    name: String
    """
    domain address, part after @
    """
    domain: String
    """
    How this email address is used
    """
    variation: Common_ContactVariationInput
    """
    email verification
    """
    verification: Common_VerificationInput
}

````


### Qb_PostalAddressInput

```

input Qb_PostalAddressInput { 
    """
    Contains primary address information - number, street
    """
    streetAddressLine1: String!
    """
    Contains secondary address information - apartment, care of, attention, ...
    """
    streetAddressLine2: String
    """
    Contains spill over space for line 2.
    """
    streetAddressLine3: String
    """
    Postal codes or zip code used for mail sorting.
    """
    postalCode: String
    """
    Any human settlement including cities, towns, villages, hamlets, localities, etc.
    """
    city: String
    """
    State, province, region abbreviation.  Reference https://pe.usps.com/text/pub28/28apb.htm
    """
    state: String
    """
    Sovereign nations and their dependent territories, anything with
    an ISO-3166 3 letter code.  Reference https://www.iso.org/iso-3166-country-codes.html
    """
    country: Common_CountryCode
    """
    Latitude/longitude associated to this address
    """
    geoLocation: Common_GeoLocationInput
    """
    Address verification information. This field is read only.
    """
    verification: Common_VerificationInput
    """
    Capture the intention of this postal address
    """
    variation: Common_ContactVariationInput
}

```

### Qb_PostalAddressInput

```
input Qb_PostalAddressInput { 
    """
    Contains primary address information - number, street
    """
    streetAddressLine1: String!
    """
    Contains secondary address information - apartment, care of, attention, ...
    """
    streetAddressLine2: String
    """
    Contains spill over space for line 2.
    """
    streetAddressLine3: String
    """
    Postal codes or zip code used for mail sorting.
    """
    postalCode: String
    """
    Any human settlement including cities, towns, villages, hamlets, localities, etc.
    """
    city: String
    """
    State, province, region abbreviation.  Reference https://pe.usps.com/text/pub28/28apb.htm
    """
    state: String
    """
    Sovereign nations and their dependent territories, anything with
    an ISO-3166 3 letter code.  Reference https://www.iso.org/iso-3166-country-codes.html
    """
    country: Common_CountryCode
    """
    Latitude/longitude associated to this address
    """
    geoLocation: Common_GeoLocationInput
    """
    Address verification information. This field is read only.
    """
    verification: Common_VerificationInput
    """
    Capture the intention of this postal address
    """
    variation: Common_ContactVariationInput
}

```

### Common_ContactVariationInput

```
input Common_ContactVariationInput  {
  """
  usage of this contact info - HOME, PUBLIC, BUSINESSALTERNATE
  """
  usage: Common_ContactUsage!

  """
  purpose of this contact info - BILLING, SHIPPING
  """
  purpose: Common_ContactPurpose!

  """
  Primary, Secondary...
  """
  Common_Ordinal: Common_Ordinal!
}
```
### Common_ContactUsage
```

"""
Contact usage
"""
enum Common_ContactUsage @tag(name: "common") @tag(name: "qb-external") @tag(name: "qb") {
  UNKNOWN
  OTHER
  HOME
  # Commenting while t is being resolved
  #   BUSINESSALTERNATE
  BUSINESS
  ALTERNATE
  PUBLIC
}
```

### Common_ContactPurpose
```
"""
Purpose for contact info: home, business, ...
"""
enum Common_ContactPurpose @tag(name: "common") @tag(name: "qb-external") @tag(name: "qb") {
  UNKNOWN
  OTHER
  GENERAL
  LEGAL
  SUPPORT
  ORDER
  BILLING
  SHIPPING
}
```

### Common_Ordinal
```
"""
Classification/Importance of the contact type
"""
enum Common_Ordinal @tag(name: "common") @tag(name: "qb-external") @tag(name: "qb") {
  UNKNOWN
  OTHER
  PRIMARY
  SECONDARY
  TERTIARY
}

```

### Common_VerificationInput
```
input Common_VerificationInput  {
  """
  Verification status: success, pending, failure ...
  """
  status: Common_VerificationStatus
  """
  Verification date/time
  """
  time: DateTime
  """
  Verification method: direct user contact, 3rd party ...
  """
  method: Common_VerificationMethod
  """
  Verified type: email, sms, inapp ...
  """
  type: Common_VerificationType
}

```
### Common_VerificationStatus
```
"""
Verification status: success, pending, failure ...
"""
enum Common_VerificationStatus  {
  UNKNOWN
  # commenting whie it is being resolved with
  OTHER
  SUCCESS
  PENDING
  FAILURE
}
```

### Common_VerificationMethod
```
"""
Verification methods: direct, third party ...
"""
enum Common_VerificationMethod  {
  UNKNOWN
  # commenting whie it is being resolved with
  #	OTHER,
  #	DIRECT,
  OTHER
  DIRECT
  THIRD_PARTY
}
```

### Common_VerificationType
```
"""
Verified type: email, sms, inapp ...
"""
enum Common_VerificationType {
  UNKNOWN
  UNVERIFIED
  OTHER
  EMAIL
  INAPP
  SMS
}
```


### Common_CountryCode

```
"""
Country alpha-3 code from international standards: https://en.wikipedia.org/wiki/ISO_3166-1
"""
enum Common_CountryCode {
  UNKNOWN
  ABW
  AFG
  AGO
  AIA
  ALA
  ALB
  AND
  ARE
  ARG
  ARM
  ASM
  ATA
  ATF
  ATG
  AUS
  AUT
  AZE
  BDI
  BEL
  BEN
  BES
  BFA
  BGD
  BGR
  BHR
  BHS
  BIH
  BLM
  BLR
  BLZ
  BMU
  BOL
  BRA
  BRB
  BRN
  BTN
  BVT
  BWA
  CAF
  CAN
  CCK
  CHE
  CHL
  CHN
  CIV
  CMR
  COD
  COG
  COK
  COL
  COM
  CPV
  CRI
  CUB
  CUW
  CXR
  CYM
  CYP
  CZE
  DEU
  DJI
  DMA
  DNK
  DOM
  DZA
  ECU
  EGY
  ERI
  ESH
  ESP
  EST
  ETH
  FIN
  FJI
  FLK
  FRA
  FRO
  FSM
  GAB
  GBR
  GEO
  GGY
  GHA
  GIB
  GIN
  GLP
  GMB
  GNB
  GNQ
  GRC
  GRD
  GRL
  GTM
  GUF
  GUM
  GUY
  HKG
  HMD
  HND
  HRV
  HTI
  HUN
  IDN
  IMN
  IND
  IOT
  IRL
  IRN
  IRQ
  ISL
  ISR
  ITA
  JAM
  JEY
  JOR
  JPN
  KAZ
  KEN
  KGZ
  KHM
  KIR
  KNA
  KOR
  KWT
  LAO
  LBN
  LBR
  LBY
  LCA
  LIE
  LKA
  LSO
  LTU
  LUX
  LVA
  MAC
  MAF
  MAR
  MCO
  MDA
  MDG
  MDV
  MEX
  MHL
  MKD
  MLI
  MLT
  MMR
  MNE
  MNG
  MNP
  MOZ
  MRT
  MSR
  MTQ
  MUS
  MWI
  MYS
  MYT
  NAM
  NCL
  NER
  NFK
  NGA
  NIC
  NIU
  NLD
  NOR
  NPL
  NRU
  NZL
  OMN
  PAK
  PAN
  PCN
  PER
  PHL
  PLW
  PNG
  POL
  PRI
  PRK
  PRT
  PRY
  PSE
  PYF
  QAT
  REU
  ROU
  RUS
  RWA
  SAU
  SDN
  SEN
  SGP
  SGS
  SHN
  SJM
  SLB
  SLE
  SLV
  SMR
  SOM
  SPM
  SRB
  SSD
  STP
  SUR
  SVK
  SVN
  SWE
  SWZ
  SXM
  SYC
  SYR
  TCA
  TCD
  TGO
  THA
  TJK
  TKL
  TKM
  TLS
  TON
  TTO
  TUN
  TUR
  TUV
  TWN
  TZA
  UGA
  UKR
  UMI
  URY
  USA
  UZB
  VAT
  VCT
  VEN
  VGB
  VIR
  VNM
  VUT
  WLF
  WSM
  XKX
  YEM
  ZAF
  ZMB
  ZWE
}

```

### Common_GeoLocationInput

```
input Common_GeoLocationInput  {
  """
  Angular distance north (positive) or south (negative) from the equator
  expressed as whole number and decimal part (e.g. '37.428328')
  """
  latitude: String! #@length(max: 100)
  """
  Angular distance east or west of the prime meridian, expressed as
  whole number and decimal part (e.g. '-122.095892')
  """
  longitude: String! #@length(max: 100)
}
```

### Read Project

Query:
```
 query projectManagementProjects(
   $first: PositiveInt!,
   $after: String,
   $filter: ProjectManagement_ProjectFilter!,
   $orderBy: [ProjectManagement_OrderBy!]
 ) {
   projectManagementProjects(
     first: $first,
     after: $after
     filter: $filter,
     orderBy: $orderBy
   ) {
     edges {
       node {
         id,
         name,
         description,
         type,
         status,
         dueDate,
         startDate,
         completedDate,
         dueDate,
         assignee{
             id
         },
         priority,
         customer{
             id
         },
         account{
             id
         }
     addresses {
             streetAddressLine1,
             streetAddressLine2,
             streetAddressLine3
             state,
             postalCode
         }   
       }
     }
     pageInfo {
       hasNextPage
       hasPreviousPage
       startCursor
       endCursor
     }
   }
 }
 
```
Variables:

```
 {
   "first": 2,
   "after": null,
   "filter": {
     "startDate": {
       "between": {
         "minDate": "2024-03-30T18:47:25.123456789-07:00",
         "maxDate": "2024-05-30T18:47:25.123456789-07:00"
       }
     }
   },
   "orderBy": ["DUE_DATE_DESC"]
 }
```

Response:
```
{
    "data": {
        "projectManagementProjects": {
            "edges": [
                {
                    "node": {
                        "id": "53115889",
                        "name": "Test project 3 updated",
                        "description": null,
                        "type": null,
                        "status": "IN_PROGRESS",
                        "dueDate": "2024-08-22T00:00:00.000Z",
                        "startDate": "2024-05-01T00:00:00.000Z",
                        "completedDate": null,
                        "assignee": null,
                        "priority": null,
                        "customer": {
                            "id": "3"
                        },
                        "account": {
                            "id": "9341452164365778"
                        },
                        "addresses": []
                    }
                },
                {
                    "node": {
                        "id": "53150787",
                        "name": "TestJamProject1",
                        "description": null,
                        "type": null,
                        "status": "OPEN",
                        "dueDate": "2024-08-15T00:00:00.000Z",
                        "startDate": null,
                        "completedDate": null,
                        "assignee": null,
                        "priority": null,
                        "customer": {
                            "id": "8"
                        },
                        "account": {
                            "id": "9341452164365778"
                        },
                        "addresses": []
                    }
                },
                {
                    "node": {
                        "id": "53150815",
                        "name": "TestJamProject2",
                        "description": null,
                        "type": null,
                        "status": "OPEN",
                        "dueDate": "2024-08-15T00:00:00.000Z",
                        "startDate": null,
                        "completedDate": null,
                        "assignee": null,
                        "priority": null,
                        "customer": {
                            "id": "9"
                        },
                        "account": {
                            "id": "9341452164365778"
                        },
                        "addresses": []
                    }
                },
                {
                    "node": {
                        "id": "53115872",
                        "name": "Test project 2",
                        "description": null,
                        "type": null,
                        "status": "IN_PROGRESS",
                        "dueDate": "2024-07-31T00:00:00.000Z",
                        "startDate": "2024-05-14T00:00:00.000Z",
                        "completedDate": null,
                        "assignee": null,
                        "priority": null,
                        "customer": {
                            "id": "3"
                        },
                        "account": {
                            "id": "9341452164365778"
                        },
                        "addresses": []
                    }
                },
                {
                    "node": {
                        "id": "53115846",
                        "name": "Test project",
                        "description": null,
                        "type": null,
                        "status": "IN_PROGRESS",
                        "dueDate": "2024-06-30T00:00:00.000Z",
                        "startDate": "2024-04-01T00:00:00.000Z",
                        "completedDate": null,
                        "assignee": null,
                        "priority": null,
                        "customer": {
                            "id": "1"
                        },
                        "account": {
                            "id": "9341452164365778"
                        },
                        "addresses": []
                    }
                }
            ],
            "pageInfo": {
                "hasNextPage": false,
                "hasPreviousPage": false,
                "startCursor": "c2ltcGxlLWN1cnNvcjA=",
                "endCursor": "c2ltcGxlLWN1cnNvcjQ="
            }
        }
    }
}

```

## Filter support:

### ProjectManagement_ProjectFilter

```
""" Filtering for projects """
input ProjectManagement_ProjectFilter  {
    """ Filter the projects with given if of project """
    id: ProjectManagement_IdExpression
    """ Filter the projects with given status.Allowed values of status (OPEN,IN_PROGRESS,BLOCKED,CANCELED,COMPLETE,OTHER).
    Only one status can be applied at a time and returns projects of all status if not passed"""
    status: ProjectManagement_StatusExpression
    """ Filter the projects with the given customerId """
    customer: ProjectManagement_CustomerExpression
    """ Filter the projects with the given type """
    type: ProjectManagement_StringExpression
    """ Filter the projects with the given priority """
    priority: ProjectManagement_IntExpression
    """ Filter the projects with the given whose dueDate is prior to the given date """
    dueDate: ProjectManagement_DateExpression
    """ Filter the projects with the given whose startDate is after to the given date """
    startDate: ProjectManagement_DateExpression
    """ Filter the projects that are recurring projects. Default is false and will not return recurring projects """
    includeRecurringProjects: Boolean
    """ Filter the projects that deleted is passed false(default behaviour). If passed true will include all projects """
    deleted: Boolean
}
```

### ProjectManagement_CustomerExpression
```
input ProjectManagement_CustomerExpression  {
    equals: ProjectManagement_CustomerInput
    in: [ProjectManagement_CustomerInput!]
}
```

### ProjectManagement_StatusExpression
```
input ProjectManagement_StatusExpression  {
    equals: ProjectManagement_Status
    in: [ProjectManagement_Status!]
    nin: [ProjectManagement_Status!]
}
```

### ProjectManagement_StringExpression
```
input ProjectManagement_StringExpression  {
    equals: String
    in: [String!]
}
```

### ProjectManagement_IdExpression
```
input ProjectManagement_IdExpression  {
    equals: ID
    in: [ID!]
}
```

### ProjectManagement_IntExpression
```
input ProjectManagement_IntExpression {
    equals: Int
    in: [Int!]
}
```
### ProjectManagement_DateExpression
```
input ProjectManagement_DateExpression  {
    between: ProjectManagement_DateRange!
}
```
### ProjectManagement_DateRange
```
input ProjectManagement_DateRange  {
    minDate: DateTime!
    maxDate: DateTime!
}
```

### Sample filter

```
 "dueDate": {
      "between": {
        "minDate": "2024-04-01T18:47:25.123456789-07:00",
        "maxDate": "2024-12-05T18:47:25.123456789-07:00"
      }
    }
```

### Create Project
Mutation:
```
mutation ProjectManagementCreateProject($name: String!, 
    $description: String,
    $startDate: DateTime,
    $dueDate: DateTime!, 
    $status: ProjectManagement_Status,
    $customer: ProjectManagement_CustomerInput,
    $account: ProjectManagement_CompanyInput!,
    $priority: Int,
    $pinned: Boolean,
    $completionRate: Decimal,
    $emailAddress: [Qb_EmailAddressInput],
    $addresses: [Qb_PostalAddressInput]
) {
  projectManagementCreateProject(input:{
    name: $name,
    description: $description,
    startDate : $startDate,
    dueDate : $dueDate,
    status: $status
    customer: $customer, 
    account: $account,
    priority: $priority,
    pinned: $pinned,
    completionRate: $completionRate,
    emailAddress: $emailAddress,
    addresses: $addresses,
  }
    )
    {
           ... on ProjectManagement_Project {
        id,
        name,
        description,
        startDate,
        dueDate,
        status,
        priority,
        customer{
            id
        },
        account{
            id
        },
        priority,
        pinned,
        completionRate,
        emailAddress {
            email,
            name
        }
        addresses {
            streetAddressLine1,
            streetAddressLine2,
            streetAddressLine3
            state,
            postalCode
        }   
      }
        }
    }
}
```

Sample Variables: 

``` 
{
  "name": "Demo Project",
  "description": "Test Demo Project",
  "startDate:": "2024-07-05T00:00:00.000Z",
  "dueDate": "2024-07-31T00:00:00.000Z",
   "status": "OPEN",
   "customer": {
        "id":1
    },
  "account": {
    "id": "9341451497924167"
    },
   "priority": 1,
   "pinned": false,
   "completionRate": 99.00,
   "emailAddress": [
        {
        "email":"testproject@gmail.com",
        "name": "Demo Client",
        "variation": {
            "purpose": "BILLING",
            "usage": "HOME",
            "Common_Ordinal": "PRIMARY"
        }
        }
    ],
   "addresses": [
       {
        "streetAddressLine1": "3000 17th street",
        "postalCode": "94114",
        "city": "San Francisco",
        "state": "California",
        "country": "USA",
        "variation": {
            "purpose": "BILLING",
            "usage": "HOME",
            "Common_Ordinal": "PRIMARY"
        }
       }
   ]
  }
  
```
Required Fields:

- `name` - The name of the project
- `customer` - The customer for whom the project is created
- `account` - The company for whom the project is created

Sample response:

```
{
    "data": {
        "projectManagementCreateProject": {
            "id": "409291028",
            "name": "Demo Project",
            "description": "TestDemoProject",
             "startDate:": "2024-07-23T00:00:00.000Z",
            "dueDate": "2024-07-31T00:00:00.000Z",
            "status": "OPEN",
            "priority": 1,
            "customer": {
                "id": "1"
            },
            "account": {
                "id": "9341452075221168"
            },
            "pinned": false,
            "completionRate": 99.0,
            "emailAddress": [
                {
                    "email": "testproject@gmail.com",
                    "name": null
                }
            ],
            "addresses": [
                {
                    "streetAddressLine1": "3000 17th street",
                    "streetAddressLine2": null,
                    "streetAddressLine3": null,
                    "state": "California",
                    "postalCode": "94114"
                }
            ]
        }
    }
}

```

### Update Project

Mutation:
### projectManagementUpdateProject                                    
``` 
mutation projectManagementUpdateProject($id: ID!,
    $name: String,
    $description: String,
    $status: ProjectManagement_Status,
    $startDate: DateTime,
    $dueDate: DateTime,
    $customer: ProjectManagement_CustomerInput,
    $account: ProjectManagement_CompanyInput,
    $priority: Int,
    $completionRate: Decimal,
    $pinned: Boolean,
)
{
    projectManagementUpdateProject(input:{
        id: $id,
        name: $name,
        description : $description,
        status: $status,
        startDate:$startDate,
        dueDate: $dueDate,
        customer:$customer
        account: $account,
        priority: $priority,
        completionRate: $completionRate,
        pinned: $pinned
    })
    {
    ... on ProjectManagement_Project {
            id,
            name,
            description,
             status,
            startDate,
            dueDate,
            customer{
                id
            },
            account{
                id
            },
            priority,
            completionRate,
            pinned
        }
    }
}
```

Variables:
You can pass the required fields that needs to be updated. Sample below has almost all fields.

```
{
"id": 409291370,
  "name": "Demo Project New Update",
  "description": "Test DemoProject New Update",
  "startDate:": "2024-07-17T00:00:00.000Z",
  "dueDate": "2024-07-31T00:00:00.000Z",
   "status": "OPEN",
  "account": {
    "id": "9341452075221168"
    },
    "customer": {
        "id":1
    },

   "priority": 1,
   "completionRate": 99.00,
   "pinned": false
  }
```

Response:
```
{
    "data": {
        "projectManagementUpdateProject": {
            "id": "409291370",
            "name": "Demo Project New Update",
            "description": "Test DemoProject New Update",
            "status": "OPEN",
            "startDate": null,
            "dueDate": "2024-07-31T00:00:00.000Z",
            "customer": {
                "id": "1"
            },
            "account": {
                "id": "9341452075221168"
            },
            "priority": 1,
            "completionRate": 99.0,
            "pinned": false
        }
    }
}

```

You can also use separate updateMutation that exists to update individual fields as below or use the generalized update mutation as above to update more than one field as needed.

### projectManagementUpdateName
```
 mutation projectManagementUpdateName($id: ID!, 
     $name: String!)
 {
     projectManagementUpdateName(input:{
         id: $id,
         name: $name,
     })
     {
     ... on ProjectManagement_Project {
             name, 
             id,
             description
         }
     }
 }
```

### projectManagementUpdateStatus
```
mutation projectManagementUpdateStatus($id: ID!, 
    $status: ProjectManagement_Status!)
{
    projectManagementUpdateStatus(input:{
        id: $id,
        status: $status,
    })
    {
    ... on ProjectManagement_Project {
            name, 
            id,
            status
        }
    }
}
```

### projectManagementUpdateDescription
```
mutation projectManagementUpdateDescription($id: ID!, 
    $description: String!)
{
    projectManagementUpdateDescription(input:{
        id: $id,
        description: $description,
    })
    {
    ... on ProjectManagement_Project {
            name, 
            id,
            description
        }
    }
}
```

###  projectManagementUpdateDueDate
 
```
mutation projectManagementUpdateDueDate($id: ID!, 
    $dueDate: DateTime!)
{
    projectManagementUpdateDueDate(input:{
        id: $id,
        dueDate: $dueDate,
    })
    {
    ... on ProjectManagement_Project {
            id,
            dueDate
        }
    }
}
```

###  projectManagementUpdateStartDate
```
mutation projectManagementUpdateStartDate($id: ID!, 
    $startDate: DateTime!)
{
    projectManagementUpdateStartDate(input:{
        id: $id,
        startDate: $startDate,
    })
    {
    ... on ProjectManagement_Project {
            id,
            startDate
        }
    }
}
```

###  projectManagementUpdateCustomer 
```
mutation projectManagementUpdateCustomer($id: ID!, 
    $customer: ProjectManagement_CustomerInput!)
{
    projectManagementUpdateCustomer(input:{
        id: $id,
        customer: $customer,
    })
    {
    ... on ProjectManagement_Project {
            id
        }
    }
}
``` 

### projectManagementUpdateCompletionRate

```
mutation projectManagementUpdateCompletionRate($id: ID!, 
    $completionRate: Decimal!)
{
    projectManagementUpdateCompletionRate(input:{
        id: $id,
        completionRate: $completionRate,
    })
    {
    ... on ProjectManagement_Project {
            id,
            completionRate
        }
    }
}
```

### projectManagementUpdateEmailAddress
 
```
mutation projectManagementUpdateEmailAddress($id: ID!, 
    $emailAddress: [Qb_EmailAddressInput]!
    )
{
    projectManagementUpdateEmailAddress(input:{
        id: $id,
        emailAddress: $emailAddress,
    })
    {
    ... on ProjectManagement_Project {
            id,
            completionRate
        }
    }
}
```

### projectManagementUpdateAddresses
``` 
mutation projectManagementUpdateAddresses($id: ID!, 
    $addresses: [Qb_PostalAddressInput]!
    )
{
    projectManagementUpdateAddresses(input:{
        id: $id,
        addresses: $addresses,
    })
    {
    ... on ProjectManagement_Project {
            id
        }
    }
}
```

                                                                                                                                              
### Delete Project
Mutation:                                                                                                                           
                                                                                                                                              
```                                                                                                                                           
mutation projectManagementDeleteProject($id: ID!)                                                                                             
{                                                                                                                                             
    projectManagementDeleteProject(input:{                                                                                                    
        id: $id,                                                                                                                              
    })
    {                                                                                                                                         
    ... on ProjectManagement_Project {                                                                                                        
            id,                                                                                                                               
            deleted                                                                                                                           
        }                                                                                                                                     
    }                                                                                                                                         
                                                                                                                                              
```                                                                                                                                           
                                                                                                                                              
Variables:                                                                                                                                    
```                                                                                                                                           
{                                                                                                                                             
  "id": 53115889                                                                                                                              
}                                                                                                                                             
```                                                                                                                                           
                                                                                                                                              
Response:                                                                                                                                     
```
{
    "data": {
        "projectManagementDeleteProject": {
            "id": "53115872",
            "deleted": true
        }
    }
}

```                                                                                                                                     
                                                                                                                                              
                                                                                                                                              