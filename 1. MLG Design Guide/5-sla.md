# Service Level Agreements (SLA)

## Purpose
The `SLA` element defines measurable service-level commitments for a data product.
It establishes clear expectations about availability, performance, support responsiveness, and data freshness.
This creates accountability and trust between data producers and consumers.

## Structure
The element follows the ODPS 4.x schema with two parts:

```yaml
SLA:
  declarative:
    - dimension: uptime
      displayTitle:
        en: System Uptime
      description:
        en: Percentage of time the system is operational
      objective: 99.5
      unit: percent

    - dimension: responseTime
      displayTitle:
        en: API Response Time
      description:
        en: Maximum time to process API requests
      objective: 500
      unit: milliseconds

    - dimension: updateFrequency
      displayTitle:
        en: Data Update Frequency
      description:
        en: How often data is refreshed
      objective: 1
      unit: days

  executable:
    - type: Prometheus
      spec:
        - metric: http_request_duration_seconds
          threshold: 0.5
```

## Key Dimensions

ODPS 4.1 defines standard SLA dimensions:

* **uptime** — System availability percentage
* **responseTime** — API/query response latency
* **updateFrequency** — Data refresh cadence
* **errorRate** — Maximum tolerated error percentage
* **timeToDetect** — How quickly issues are identified
* **timeToNotify** — How quickly customers are informed
* **timeToRepair** — How quickly issues are resolved
* **emailResponseTime** — Support team response speed
* **endOfSupport** — When support ends for a version
* **endOfLife** — When a version is retired

## Profile-Based Approach

In this organization, **inline SLA definitions are not allowed**. Every data product must reference a standardized profile from the `SLA/` folder.

### Available Profiles

**Default Profile** (`SLA/profiles.yaml#/default`)
- Uptime: 99%
- Response Time: 500ms
- Update Frequency: Daily (1 day)

**Premium Profile** (`SLA/profiles.yaml#/premium`)
- Uptime: 99.9%
- Response Time: 200ms
- Update Frequency: Hourly (60 minutes)
- Error Rate: < 0.1%

### Usage in Data Products

```yaml
# At product level (without pricing plans)
SLA:
  $ref: ./SLA/profiles.yaml#/default

# Within pricing plans
pricingPlans:
  declarative:
    en:
      - name: Freemium Plan
        SLA:
          $ref: ./SLA/profiles.yaml#/default

      - name: Premium Plan
        SLA:
          $ref: ./SLA/profiles.yaml#/premium
```

## Governance Guidelines

### When SLA is Required

* Include `SLA` at the **product level** if no pricing plans exist
* Include `SLA` **within each pricing plan** if pricing plans are defined
* Never include both—SLA definitions belong in one place only

### Profile References

* Always use `$ref` to reference profiles from `SLA/profiles.yaml`
* Never define SLA metrics inline in product files
* Use `default` profile unless business requirements justify `premium`

### Support Commitments

SLA profiles can include support terms:

```yaml
support:
  phoneNumber: "+971-2-123-4567"
  phoneServiceHours: "24/7 for critical issues"
  email: "support@example.com"
  emailServiceHours: "Business hours: Sun-Thu 8AM-5PM GST"
  documentationURL: "https://docs.example.com/support"
```

### SLA Monitoring

* **Declarative** section defines target service levels (commitments to customers)
* **Executable** section (optional) contains monitoring code for SLA verification
* Use tools like Prometheus, Datadog, or custom monitoring
* Automated monitoring enables proactive issue detection

## Implementation

SLA profiles are:

* Centrally maintained in `2. Products/SLA/profiles.yaml`
* Referenced by all data products using `$ref` syntax
* Validated against ODPS 4.1 schema locally
* Version-controlled in Git for accountability

Claude ensures correct structure, profile references, and schema compliance.

## Relationship to Pricing Plans

Different pricing tiers typically offer different service levels:

* **Freemium/Free tiers** → Default SLA (99% uptime, daily updates)
* **Standard tiers** → Default SLA with email support
* **Premium/Enterprise tiers** → Premium SLA (99.9% uptime, hourly updates, priority support)

This allows consumers to pay for the service level they need.

## SLA vs Data Quality

While related, these are distinct commitments:

| Aspect | SLA | Data Quality |
|--------|-----|--------------|
| **Focus** | Service delivery (how) | Data content (what) |
| **Examples** | Uptime, response time, support | Accuracy, completeness, validity |
| **Impact** | Service availability and performance | Data reliability and correctness |

Both are critical for data product trust and should be defined together.

## Example Use

When creating a new data product:

1. Determine required service level based on consumer needs
2. Reference appropriate profile from `SLA/profiles.yaml`
3. If using pricing plans, assign SLA profiles per tier
4. Validate that `$ref` paths resolve correctly
5. Ensure no standalone `SLA` section exists if pricing plans are defined
6. Consider adding support contact information if needed

## Handling SLA Violations

When service levels aren't met:

* **Transparency** — Communicate issues promptly
* **Root Cause Analysis** — Understand what went wrong
* **Remediation** — Fix the underlying issue
* **Compensation** — Consider credits or refunds for paid tiers
* **Improvement** — Update processes to prevent recurrence

SLAs build trust only when honored consistently.

## Related MLG Elements

* **dataQuality** — Quality commitments complement service commitments
* **dataAccess** — Access methods subject to SLA guarantees
* **pricingPlans** — SLA bundled with pricing tiers
* **productStrategy** — SLA supports business KPIs (e.g., customer satisfaction)

## Compliance Notes

* Each data product must declare SLA commitments
* Service levels must be measurable and realistic
* Changes to SLA profiles affect all products using that profile
* Premium SLAs typically require both better targets and proactive monitoring
* Support response times must align with available support resources
* Consider time zones when defining service hours for global products
