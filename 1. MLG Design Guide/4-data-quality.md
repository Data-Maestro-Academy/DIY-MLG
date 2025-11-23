# Data Quality

## Purpose
The `dataQuality` element defines measurable quality commitments for a data product.
It establishes expectations about data accuracy, completeness, timeliness, and other quality dimensions that consumers can rely on.
This ensures trust and enables quality-driven decision-making across the organization.

## Structure
The element follows the ODPS 4.x schema with two parts:

```yaml
dataQuality:
  declarative:
    - dimension: accuracy
      displaytitle:
        - en: Data Accuracy
      description:
        - en: Percentage of records that correctly represent real-world entities
      objective: 95
      unit: percentage

    - dimension: completeness
      displaytitle:
        - en: Data Completeness
      description:
        - en: Percentage of required fields that contain valid values
      objective: 98
      unit: percentage

  executable:
    - type: SodaCL
      version: 3.0
      spec:
        checks for flight_schedules:
          - missing_count(flight_id) = 0
          - invalid_percent(scheduled_departure) < 1%
```

## Key Dimensions

ODPS 4.1 defines eight standard quality dimensions:

* **accuracy** — Data correctly represents reality
* **completeness** — All required data is present
* **conformity** — Data follows defined formats and rules
* **consistency** — Data is coherent across systems
* **coverage** — Data covers the intended scope
* **timeliness** — Data is available when needed
* **validity** — Data conforms to business rules
* **uniqueness** — No unwanted duplicates exist

## Profile-Based Approach

In this organization, **inline data quality definitions are not allowed**. Every data product must reference a standardized profile from the `DQ/` folder.

### Available Profiles

**Default Profile** (`DQ/profiles.yaml#/default`)
- Accuracy: 95%
- Completeness: 98%
- Timeliness: 95%

**Premium Profile** (`DQ/profiles.yaml#/premium`)
- Accuracy: 99%
- Completeness: 99%
- Validity: 99%

### Usage in Data Products

```yaml
# At product level (without pricing plans)
dataQuality:
  $ref: ./DQ/profiles.yaml#/default

# Within pricing plans
pricingPlans:
  declarative:
    en:
      - name: Freemium Plan
        dataQuality:
          $ref: ./DQ/profiles.yaml#/default

      - name: Premium Plan
        dataQuality:
          $ref: ./DQ/profiles.yaml#/premium
```

## Governance Guidelines

### When Data Quality is Required

* Include `dataQuality` at the **product level** if no pricing plans exist
* Include `dataQuality` **within each pricing plan** if pricing plans are defined
* Never include both—quality definitions belong in one place only

### Profile References

* Always use `$ref` to reference profiles from `DQ/profiles.yaml`
* Never define quality metrics inline in product files
* Use `default` profile unless business requirements justify `premium`

### Quality Monitoring

* **Declarative** section defines target quality levels (human-readable commitments)
* **Executable** section (optional) contains automated monitoring code for tools like SodaCL, Monte Carlo, or DQOps
* Executable specs enable continuous quality validation in data pipelines

## Implementation

Data quality profiles are:

* Centrally maintained in `2. Products/DQ/profiles.yaml`
* Referenced by all data products using `$ref` syntax
* Validated against ODPS 4.1 schema locally
* Version-controlled in Git for change tracking

Claude ensures correct structure, profile references, and schema compliance.

## Relationship to Pricing Plans

Different pricing tiers typically offer different quality guarantees:

* **Freemium/Free tiers** → Default quality profile (95-98%)
* **Premium tiers** → Premium quality profile (99%)
* **Enterprise tiers** → Premium quality profile with executable monitoring

This allows consumers to choose the quality level that matches their needs and budget.

## Example Use

When creating a new data product:

1. Determine required quality level based on use cases
2. Reference appropriate profile from `DQ/profiles.yaml`
3. If using pricing plans, assign quality profiles per tier
4. Validate that `$ref` paths resolve correctly
5. Ensure no standalone `dataQuality` section exists if pricing plans are defined

## Related MLG Elements

* **SLA** — Service-level commitments (uptime, response time)
* **dataAccess** — How quality data is delivered
* **pricingPlans** — Quality guarantees bundled with pricing tiers
* **productStrategy** — Business KPIs that quality supports

## Compliance Notes

* Each data product must declare quality commitments
* Quality levels must be measurable and verifiable
* Changes to quality profiles affect all products using that profile
* Premium quality typically requires both better targets and executable monitoring
* Quality metrics should align with consumer expectations and regulatory requirements
