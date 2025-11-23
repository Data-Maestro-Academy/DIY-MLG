# Minimum Lovable Governance (MLG) Design Guide

Welcome to the MLG Design Guide — your comprehensive resource for building, maintaining, and governing data products using a lightweight, standards-driven approach.

## What is MLG?

**Minimum Lovable Governance (MLG)** is a governance model that balances control with agility. It replaces heavy, centralized bureaucracy with simple, automatable rules that both humans and AI can follow.

### Core Principles

1. **Minimum, not minimal** — Just enough governance to ensure quality, trust, and compliance
2. **Lovable** — Practical to use, respected by teams, not a burden
3. **As Code** — YAML, schemas, and workflows replace manual processes
4. **Reusable Components** — Shared profiles for quality, SLAs, and access methods
5. **AI-Ready** — Structured for AI tools to author, validate, and explain compliance

### Why MLG Matters

Traditional governance creates bottlenecks. MLG enables fast delivery while maintaining accountability through:

* **Standardization** — Open Data Product Specification (ODPS) 4.1
* **Automation** — Schema validation, CI/CD, AI assistance
* **Transparency** — Everything in Git for traceability
* **Scalability** — Governance grows through automation, not headcount

## Design Guide Contents

This guide explains how to implement MLG for data products. Each chapter covers a key component:

### Foundation

**[1. Introduction](1-introduction.md)**
- What is Minimum Lovable Governance?
- Core principles and benefits
- Why traditional governance fails
- How MLG fits agile and DevOps culture

### Core Data Product Elements

**[2. Product Strategy](2-product-strategy.md)**
- Connecting data products to business outcomes
- Defining KPIs that matter (business vs product level)
- Measuring value and impact
- Aligning products with organizational goals

**[3. Data Contracts](3-data-contracts.md)**
- What are data contracts and why they matter
- Internal contracts vs external data-sharing agreements
- How ODPS supports contracts
- Schema definitions and versioning
- Relationship to legal agreements

### Quality and Service Commitments

**[4. Data Quality](4-data-quality.md)**
- Defining measurable quality commitments
- Eight standard quality dimensions
- Profile-based quality management (default vs premium)
- Declarative vs executable quality monitoring
- Quality in pricing tiers

**[5. Service Level Agreements (SLA)](5-sla.md)**
- Service-level commitments and accountability
- Key SLA dimensions (uptime, response time, freshness)
- Profile-based SLA management
- Support commitments and response times
- Handling SLA violations

**[6. Data Access](6-data-access.md)**
- How consumers retrieve and use data products
- Access methods: API, files, SQL, AI agents (MCP)
- Authentication and security
- Profile-based vs product-specific access
- Multi-access strategies for diverse consumers

## How to Use This Guide

### For Product Owners

Start with **[Introduction](1-introduction.md)** and **[Product Strategy](2-product-strategy.md)** to understand how data products align with business goals. Then review **[Data Quality](4-data-quality.md)** and **[SLA](5-sla.md)** to understand commitments you'll make to consumers.

### For Data Engineers

Focus on **[Data Contracts](3-data-contracts.md)**, **[Data Quality](4-data-quality.md)**, and **[Data Access](6-data-access.md)** to understand technical implementation. Use these as reference when building products.

### For Governance Teams

Read the entire guide to understand the governance framework. Pay special attention to profile-based approaches in Quality, SLA, and Access chapters — this is where governance scales without creating bottlenecks.

### For AI/Claude Users

This guide is designed to be AI-readable. Claude Code uses these guidelines along with technical rules in `.claude/governance-rules.md` to help you build compliant data products automatically.

## The Profile-Based Approach

A key MLG innovation is **reusable profiles** for quality, SLAs, and access:

```
2. Products/
├── DQ/
│   └── profiles.yaml          # default, premium quality profiles
├── SLA/
│   └── profiles.yaml          # default, premium SLA profiles
├── Access/
│   └── profiles.yaml          # api, agent, file access profiles
└── [product-name].yaml        # References profiles via $ref
```

### Benefits

✅ **Consistency** — All products using "premium" get the same quality level
✅ **Maintainability** — Update once, apply everywhere
✅ **Clarity** — Clear tiers (default, premium) make choices simple
✅ **Scalability** — New products reference existing profiles
✅ **Pricing Alignment** — Profiles map cleanly to pricing tiers

## Governance in Action

When you create a data product:

1. **Define strategy** — What business outcome does it support?
2. **Establish contract** — What data schema and semantics?
3. **Set quality** — Reference appropriate DQ profile
4. **Commit to SLA** — Reference appropriate SLA profile
5. **Enable access** — Define how consumers get the data
6. **Validate** — Claude checks against ODPS 4.1 schema
7. **Version control** — Git tracks all changes
8. **Monitor** — Optional executable specs enable automation

## Relationship to Pricing Plans

A powerful MLG pattern: **embed quality, SLA, and access within pricing plans**

```yaml
pricingPlans:
  declarative:
    en:
      - name: Freemium Plan
        price: "0"
        dataQuality:
          $ref: ./DQ/profiles.yaml#/default
        SLA:
          $ref: ./SLA/profiles.yaml#/default
        access:
          $ref: ./Access/profiles.yaml#/api

      - name: Premium Plan
        price: "149"
        dataQuality:
          $ref: ./DQ/profiles.yaml#/premium
        SLA:
          $ref: ./SLA/profiles.yaml#/premium
        access:
          $ref: ./Access/profiles.yaml#/api

      - name: Agent Plan
        price: "399"
        dataQuality:
          $ref: ./DQ/profiles.yaml#/premium
        SLA:
          $ref: ./SLA/profiles.yaml#/premium
        access:
          $ref: ./Access/profiles.yaml#/agent
```

This creates clear value tiers where higher prices deliver better service.

## Standards and Tools

### Standards

* **ODPS 4.1** — Open Data Product Specification
* **ODCS/DCS** — Open Data Contract Specification
* **MCP** — Model Context Protocol (for AI agents)
* **OAS** — OpenAPI Specification (for APIs)

### Tools

* **Git** — Version control for all governance artifacts
* **YAML** — Human and machine-readable format
* **JSON Schema** — Validation of ODPS compliance
* **Claude Code** — AI-assisted authoring and validation
* **CI/CD pipelines** — Automated validation and deployment

## Getting Started

1. Read **[1. Introduction](1-introduction.md)** to understand the philosophy
2. Review relevant chapters for your role
3. Look at example data products in `2. Products/` folder
4. Reference `.claude/governance-rules.md` for technical rules
5. Use Claude Code to build compliant products faster

## Contributing

This guide evolves with our governance practices:

* Update chapters when policies change
* Add examples as we learn
* Keep aligned with `.claude/governance-rules.md`
* Share lessons learned

## Support

Questions about MLG or data product governance?

* Check this guide first
* Review example products
* Consult the governance team
* Use Claude Code for guidance

---

**Remember:** Governance should enable, not block. MLG makes doing the right thing easy.

*Last Updated: 2025*
