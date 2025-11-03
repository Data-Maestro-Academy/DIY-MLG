# Product Strategy

## Purpose
The `productStrategy` element defines the **strategic intent** of a data product.  
It connects business-level outcomes to product-level outputs and provides measurable traceability between them.  
This ensures every data product delivers value aligned with organizational goals.

## Structure
The element follows the ODPS 4.x schema:

```yaml
productStrategy:
    objectives:
      - en: "Enable travel apps and services"
    contributesToKPI:
      id: "KPI-AUH-001"
      name: "Passenger satisfaction score"
      unit: "percent"
      target: 85
      direction: increase
      timeframe: "2025-Q4"
    productKPIs:
      - id: "KPI-AUH-001-A"
        name: "Travel app integrations"
        unit: "applications"
        target: 15
        calculation: "COUNT(DISTINCT app_id) FROM active_integrations"
```

## Guidelines

* **Outcome** = business-level objective (strategic impact). `contributesToKPI` links each KPI to its contributing outcome

* **Output** = data product KPIs (delivery and performance). Use `productKPIs` to define and align with business leadership on product level metrics.

* Use plain, verifiable metrics

* Keep descriptions business-readable but schema-compliant

## Governance Notes

* Each data product must have at least one outcome and one output KPI.

* The link between outcome and output is critical for value tracking.

* Updates to KPIs must remain traceable across releases.

* Validation is enforced locally against ODPS schema.

## Implementation

This element is maintained directly in YAML and validated using Claude in VS Code with the local ODPS schema.

Claude ensures correct structure, naming, and schema alignment.

## Example Use

When publishing new data products, include a validated productStrategy section to enable:

* Consistent goal tracking
* Automated impact reporting
* AI-assisted governance audits

## Related MLG Elements

* dataQuality
* SLA
* pricingPlans