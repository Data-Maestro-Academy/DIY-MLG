# Data Access

## Purpose
The `dataAccess` element defines how consumers can retrieve and interact with a data product.
It specifies access methods, formats, authentication, and technical endpoints.
This ensures consumers understand exactly how to consume the data product and what integration methods are available.

## Structure
The element follows the ODPS 4.x schema:

```yaml
dataAccess:
  default:
    name:
      - en: REST API Access
    description:
      - en: RESTful API access with JSON response format
    outputPortType: API
    format: JSON
    authenticationMethod: OAuth
    specification: OAS
    accessURL: https://api.example.com/v1/flights
    specsURL: https://api.example.com/openapi.json
    documentationURL: https://docs.example.com/api

  agent:
    name:
      - en: AI Agent Access via MCP
    description:
      - en: Model Context Protocol access for AI agents
    outputPortType: AI
    format: MCP
    authenticationMethod: Token
    specification: MCP
    accessURL: https://mcp.example.com/server
    specsURL: https://mcp.example.com/llms.txt
    documentationURL: https://docs.example.com/mcp
```

## Key Components

### Access Method Name
A **required** access method named `default` must always be present. Additional methods (like `agent`, `file`, `sql`) are optional.

### Output Port Types
Describes how data is delivered:

* **API** — REST, GraphQL, or other API
* **file** — Downloadable files (CSV, JSON, Excel, ZIP)
* **SQL** — Direct database query access
* **AI** — Model Context Protocol (MCP) for AI agents
* **gRPC** — High-performance RPC
* **sFTP** — Secure file transfer

### Data Formats
Supported formats include:

* **JSON** — Structured API responses
* **CSV** — Tabular data files
* **XML** — Legacy or enterprise systems
* **Excel** — Spreadsheet format
* **zip** — Compressed file packages
* **plain text** — Simple text files
* **GraphQL** — Query-based API
* **MCP** — AI agent protocol
* **TOON** — Specialized format

### Authentication Methods

* **OAuth** — Standard OAuth 2.0 flow
* **Token** — Bearer token or API token
* **API key** — Simple key-based auth
* **HTTP Basic** — Username/password
* **none** — Public/open access

### Specifications

* **OAS** — OpenAPI Specification (Swagger)
* **RAML** — RESTful API Modeling Language
* **Slate** — API documentation framework
* **MCP** — Model Context Protocol for AI agents

## Profile-Based Approach

In this organization, access methods can be defined in two ways:

### 1. Shared Access Profiles (Recommended for APIs and Agents)

For common access patterns that use the same gateway/server, use profiles from `Access/` folder:

```yaml
dataAccess:
  default:
    $ref: ./Access/profiles.yaml#/api
  agent:
    $ref: ./Access/profiles.yaml#/agent
```

### 2. Inline Definitions (Required for Product-Specific URLs)

For file-based access or product-specific endpoints, define inline:

```yaml
dataAccess:
  default:
    name:
      - en: CSV File Download
    description:
      - en: Flight schedule data in CSV format
    outputPortType: file
    format: CSV
    authenticationMethod: none
    accessURL: https://data.example.com/flight-schedules.csv
```

### 3. Mixed Approach (Common Pattern)

Combine inline file access with shared API/Agent profiles:

```yaml
dataAccess:
  default:
    name:
      - en: CSV File Download
    outputPortType: file
    format: CSV
    accessURL: https://data.example.com/product-name.csv

  API:
    $ref: ./Access/profiles.yaml#/api

  agent:
    $ref: ./Access/profiles.yaml#/agent
```

## Available Profiles

**API Profile** (`Access/profiles.yaml#/api`)
- REST API with OAuth
- JSON format
- OpenAPI specification

**Agent Profile** (`Access/profiles.yaml#/agent`)
- Model Context Protocol (MCP)
- Token authentication
- AI agent specification

**File Profile** (`Access/profiles.yaml#/file`)
- ZIP download
- No authentication
- Generic file access

## Governance Guidelines

### When Access Methods are Required

* Include `dataAccess` at the **product level** if no pricing plans exist
* Include `dataAccess` **within each pricing plan** if pricing plans are defined
* Never include both—access definitions belong in one place only

### The `default` Requirement

* ODPS 4.1 **requires** an access method named `default`
* This is the primary/recommended way to access the data
* Additional methods are optional but encouraged for flexibility

### Choosing Between Profiles and Inline

* **Use profiles** when multiple products share the same API gateway or MCP server
* **Use inline** when each product has unique URLs (files, product-specific endpoints)
* **Mix both** when offering multiple access methods with different requirements

### URL Specificity

* File URLs must be product-specific (e.g., `flight-schedules.csv`)
* API URLs can be shared if using common gateway (use profiles)
* Agent/MCP URLs can be shared if using common MCP server (use profiles)

## Implementation

Access definitions are:

* Stored inline in product files OR referenced from `2. Products/Access/profiles.yaml`
* Validated against ODPS 4.1 schema locally
* Version-controlled in Git for traceability

Claude ensures correct structure, profile references, and schema compliance.

## Relationship to Pricing Plans

Different pricing tiers often provide different access methods:

* **Freemium tiers** → Basic API access only
* **Standard tiers** → Full API access
* **Premium tiers** → API + file downloads
* **Agent tiers** → AI Agent (MCP) access for autonomous systems

This allows monetization based on access sophistication.

## AI Agent Access (MCP)

The Model Context Protocol (MCP) enables AI agents to access data products autonomously:

```yaml
agent:
  name:
    - en: AI Agent Access via MCP
  description:
    - en: Autonomous AI access using Model Context Protocol
  outputPortType: AI
  format: MCP
  authenticationMethod: Token
  specification: MCP
  accessURL: https://mcp.example.com/server
  specsURL: https://mcp.example.com/llms.txt
  documentationURL: https://docs.example.com/mcp-guide
```

MCP access is ideal for:
- Autonomous AI applications
- LLM-powered analytics
- Intelligent automation
- Real-time agent decision-making

## Example Use

When creating a new data product:

1. Determine which access methods consumers need
2. Choose between profile references (shared) or inline definitions (product-specific)
3. Ensure `default` access method is present
4. If using pricing plans, assign access methods per tier
5. Validate that all URLs and `$ref` paths are correct
6. Provide complete documentation URLs for each access method

## Multi-Access Strategy

Offering multiple access methods increases product value:

```yaml
dataAccess:
  default:
    # File download for simple use cases
    outputPortType: file
    format: CSV
    accessURL: https://data.example.com/product.csv

  API:
    # API for application integration
    $ref: ./Access/profiles.yaml#/api

  agent:
    # AI/MCP for autonomous agents
    $ref: ./Access/profiles.yaml#/agent
```

This serves diverse consumer needs:
- **Analysts** → CSV files
- **Developers** → REST API
- **AI Systems** → MCP agents

## Related MLG Elements

* **SLA** — Access methods subject to SLA guarantees (uptime, response time)
* **dataQuality** — Quality applies regardless of access method
* **pricingPlans** — Access methods bundled with pricing tiers
* **dataContract** — Contract specifies allowed access patterns

## Compliance Notes

* Each data product must provide at least one access method named `default`
* Access URLs must be valid and reachable
* Authentication methods must align with organizational security policies
* Documentation URLs should provide complete integration guides
* Specification files (OpenAPI, MCP schema) must be kept up-to-date
* When using `$ref`, ensure referenced profiles exist and are valid
* File-based access requires product-specific URLs—profiles cannot be used
* Consider access logs and usage tracking for compliance and billing
