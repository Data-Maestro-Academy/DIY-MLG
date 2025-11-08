# Data Contracts in the Context of the Open Data Product Specification (ODPS) 4.1  

## What is a Data Contract?  
A *data contract* is a formal agreement between a data producer and a data consumer. It establishes expectations about data structure, semantics, usage rights, quality commitments and change-management. 

In the ODPS framework the contract becomes a first-class metadata object, embedded in the data-product specification.

Simple bare minimum example with schema:

```yaml 
apiVersion: v3.0.2
kind: DataContract

id: <unique-contract-id>
version: 1.0.0
status: draft

name: <contract name>
domain: <logical domain>
dataProduct: <data product identifier>
description:
  purpose: <brief purpose>
  limitations: <brief limitations>
  usage: <brief usage guidance>
schema:
  - name: flight_schedules
    logicalType: object
    physicalType: table
    description: Scheduled flights at Abu Dhabi Airport (AD-X).
    properties:
      - name: flight_id
        description: Unique flight identifier.
        logicalType: string
        physicalType: text
      - name: scheduled_departure
        description: Scheduled departure date/time.
        logicalType: dateTime
        physicalType: timestamp
      - name: scheduled_arrival
        description: Scheduled arrival date/time.
        logicalType: dateTime
        physicalType: timestamp
      - name: origin_airport
        description: Origin airport ICAO/IATA code.
        logicalType: string
        physicalType: text
      - name: destination_airport
        description: Destination airport ICAO/IATA code.
        logicalType: string
        physicalType: text
```

## How ODPS supports Data Contracts  
ODPS 4.1 allows the `contract` object within a data product spec to be defined in three ways:  

* Via `contractURL` pointing to an external contract document.  
* Inline via a `spec` YAML block with full contract details.   
* By using a `$ref` pointer to a separate file containing the contract. 

Key attributes include:  
* `id`: Unique identifier of the contract. 
* `type`: Defines the contract standard (e.g., ODCS, DCS). 
* `contractVersion`: Version of the standard. 
* `contractURL`, `spec` or `$ref`: How the contract content is linked or embedded. 

## Setting the Scene  
A *data contract* is typically an internal agreement between a data producer and a data consumer. It defines responsibilities such as data structure, quality, access, and usage within the organisation.  
A *data agreement* (or data-sharing agreement) is legally binding, often used when data is exchanged **outside** company borders.  

In the latter external scenario, you still need licensing terms, service levels, usage rights and obligations — and this is where the ODPS metadata model becomes useful.

## How ODPS Supports Legal-Agreement Elements  
Even though ODPS is a metadata specification (not a full legal contract), it captures many of the key elements you would expect in a data agreement.  
Here are some examples:

| ODPS Component      | What it captures                                           | How it maps to a legal agreement clause |
|---------------------|------------------------------------------------------------|----------------------------------------|
| **licensing**       | Usage rights, restrictions, intellectual property, geography, modification rights.  | “The data can only be used for internal analytics, may not be resold, is governed by [jurisdiction], termination rights apply…” |
| **SLA (Service Level Agreement)** | Update frequency, uptime, support conditions, response times.  | “The data provider guarantees 99.5% availability, daily refresh, supports issues in business hours, credits for downtime…” |
| **dataQuality**     | Commitments on accuracy, completeness, timeliness, metrics.  | “Provider commits to completeness ≥ 98% each month, will notify consumer of schema changes 10 business days ahead…” |
| **dataAccess**      | Roles, permissions, access channels/methods, usage context. 
| “Consumer may access via API endpoint only, usage limited to approved internal projects, must log access…” |
| **pricingPlans / paymentGateway** | If external, includes pricing, payment terms, tiers. | “Subscription $X/month, payment due net-30, late fee applies, plan renewal automatically…” |

## What ODPS *Does Not* Fully Cover (And Why the Legal Agreement Still Matters)  

* ODPS does **not** enforce sign-off, signatures, jurisdiction, indemnities, liability limits, arbitration clauses or full legal boiler-plate.  
* When sharing data externally you still need a signed agreement that binds both parties under law (governance, compliance, privacy, liability).  
* Use ODPS to structure the metadata and technical/operational terms — then embed or link from your legal data-sharing agreement to that machine-readable definition.

## Practical policy  
1. In your data product spec (using ODPS) include the `licensing`, `SLA`, `dataQuality`, `dataAccess`, `pricingPlans` objects.  
2. Draft a separate data-sharing agreement for external partners that references the ODPS spec (e.g., “The parties will abide by the terms set out in the attached metadata spec, version X, section ‘dataAccess’, ‘SLA’, ‘licensing’ etc.”)  
3. Maintain versioning: when you update the ODPS metadata you may need to trigger a contract review.  
4. Internally you may rely on just the ODPS metadata and a lightweight data contract. Externally use both.

