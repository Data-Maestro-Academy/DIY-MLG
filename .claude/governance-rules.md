# Project Rules for SME-MLG Data Products

## 1. Schema Usage
- Use local schema only: "../ODPS4.1/schema/odps.json"
- No remote URLs
- Reject any field not defined in the schema
- Verify results you generate agains the given Schema

## 2. File Format
- YAML only
- One data product per file

## 4. Naming and IDs
- productID must be unique and match the file name theme
- No spaces in IDs

## 5. Validation
- Always reference the current ODPS version in the file
- Do not add fields unless confirmed in schema

## 6. Behavior Rules for Claude
- Do not infer business attributes
- Ask for clarification when information is missing
- If validation cannot be ensured, stop and request schema context

## 7. Change Control
- Any new field requires schema update before use

## 8. Minimal Data Product Definition
- When "minimal" or "minimum" is requested, include only basic product details:
  - Required schema fields only
  - NO pricing plan
  - NO SLA or data quality
  - No access component
  - No license terms
  - No Core support contact information
- Avoid extensive or elaborate content in descriptions
- Keep all text fields concise and to the point
- create only english version

Example:

schema: https://opendataproducts.org/v4.1/schema/odps.yaml
version: 4.1
product:
  en:
    name: Pets of the year
    productID: 123456are
    visibility: private
    status: draft
    type: derived data

## 9. Product Strategy Definition
- productStrategy is always initially minimal when created the first time
  - one business KPI
  - one product KPI
- Product KPIs (productKPIs) are NEVER about data quality
- Product KPIs measure outputs that support the outcome defined in contributesToKPI
- Data quality metrics belong in the separate dataQuality section
- Product KPIs should focus when possible on:
  - Adoption and usage metrics
  - Integration success
  - Business value delivery
- Each product KPI must clearly link to how it supports the business KPI in contributesToKPI

## 10. Data Contracts
- By default unless otherwise ordered, always add data contract to the data product with $ref syntax and refer to existing data product in the data-contracts folder. 

## 11. Using Data Quality Profiles

Objective:
* Ensure all data products apply standardized and reusable data quality definitions.

Instructions:

* When adding a dataQuality element to a data product, always look in the DQ/ folder for available profiles.

* Profiles (e.g., default, premium) are defined in YAML files inside DQ/.

* In the data product file define the dataQuality element and under it include a $ref pointing to the selected profile.

* By default, use the default profile unless otherwise specified.

* In this company, inline data quality definitions are not allowed. Every data product must use $ref to reference an existing profile from DQ/.

Example:

dataQuality:
  $ref: "../DQ/file.yaml#/xxxx"

Compliance Check:

The $ref must resolve without validation errors against the ODPS 4.1 schema.

## 12. Using SLA Profiles

Objective:
* Ensure all data products apply standardized and reusable SLA definitions.

Instructions:

* When adding a SLA element to a data product, always look in the SLA/ folder for available profiles.

* Profiles (e.g., default, premium, etc) are defined in YAML files inside SLA/.

* In the data product file define the SLA element and under it include a $ref pointing to the selected profile.

* By default, use the default profile unless otherwise specified.

* In this company, inline SLA definitions are not allowed. Every data product must use $ref to reference an existing profile from SLA/.

Example:

SLA:
  $ref: "./SLA/profiles.yaml#/default"

Compliance Check:

The $ref must resolve without validation errors against the ODPS 4.1 schema.

## 13. Using Data Access Profiles

Objective:
* Ensure all data products apply standardized and reusable data access definitions where appropriate, while allowing product-specific URLs for file-based access.

Instructions:

* The `default` access method is REQUIRED by ODPS 4.1 and must always be present in dataAccess.

* When adding dataAccess to a data product, consider which access methods can be shared vs. product-specific:
  - **File-based access** (CSV, ZIP, etc.): Requires product-specific URLs - define inline
  - **API access**: Can be shared if using common API gateway - use $ref to profiles
  - **MCP/Agent access**: Can be shared if using common MCP server - use $ref to profiles

* **Approach 1 - Single access method (file-based only)**:
  - Use inline definition with product-specific URL

* **Approach 2 - Single access method (API or Agent only that includes default)**:
  - Use $ref to reference shared profile from Access/

* **Approach 3 - Mixed access (file + API/Agent)**:
  - Define `default` inline with product-specific file URL
  - Reference shared API/Agent profiles using $ref for additional access methods
  - Mixing inline definitions with $ref is valid and recommended for this scenario

* **Approach 4 - Multiple shared access methods (API + Agent)**:
  - Name the primary access method as `default` and reference a shared profile
  - Reference additional shared profiles (like agent) with their own names
  - This approach satisfies the ODPS requirement for `default` while using shared profiles

Example - Inline only (file access):

dataAccess:
  default:
    name:
      - en: CSV File Download
    description:
      - en: Product data in CSV format
    outputPortType: file
    format: CSV
    authenticationMethod: none
    accessURL: https://example.com/files/product-name.csv

Example - Reference only (shared API):

dataAccess:
  $ref: "./Access/profiles.yaml#/api"

Example - Mixed (inline file + shared API):

dataAccess:
  default:
    name:
      - en: CSV File Download
    description:
      - en: Product data in CSV format
    outputPortType: file
    format: CSV
    authenticationMethod: none
    accessURL: https://example.com/files/product-name.csv
  API:
    $ref: "./Access/profiles.yaml#/api"
  agent:
    $ref: "./Access/profiles.yaml#/agent"

Example - Multiple shared profiles (API as default + Agent):

dataAccess:
  default:
    $ref: "./Access/profiles.yaml#/api"
  agent:
    $ref: "./Access/profiles.yaml#/agent"

Compliance Check:

The $ref must resolve without validation errors against the ODPS 4.1 schema.