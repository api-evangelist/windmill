# Windmill

Windmill is an open-source developer platform and workflow engine for turning scripts into webhooks, workflows, and internal apps. It supports TypeScript, Python, Go, PHP, Bash, C#, SQL, and Rust, and serves as an open-source alternative to Retool, Airflow, and Temporal for building comprehensive internal tools including endpoints, workflows, and UIs.

**URL:** [https://raw.githubusercontent.com/api-evangelist/windmill/refs/heads/main/apis.yml](https://raw.githubusercontent.com/api-evangelist/windmill/refs/heads/main/apis.yml)

## Scope

- **Type:** Index
- **Position:** Consumer
- **Access:** 3rd-Party

## Tags

- Automation
- Internal Tools
- Open Source
- ProCode API Composition
- Scripts
- Webhooks
- Workflow Engine
- Workflows

## Timestamps

- **Created:** 2026-03-03
- **Modified:** 2026-05-03

## APIs

### Windmill API
The Windmill API provides programmatic access to the Windmill developer platform, enabling management of scripts, flows, apps, resources, variables, schedules, jobs, users, workspaces, and webhooks. It follows the OpenAPI 3.0.3 specification (version 1.694.0) with 692 endpoints across 57 functional tags.

**Human URL:** [https://www.windmill.dev/docs/intro](https://www.windmill.dev/docs/intro)

#### Tags

- Automation
- Internal Tools
- ProCode API Composition
- Scripts
- Webhooks
- Workflows

#### Properties

| Type | URL |
|------|-----|
| Documentation | https://www.windmill.dev/docs/intro |
| OpenAPI | https://raw.githubusercontent.com/api-evangelist/windmill/refs/heads/main/openapi/windmill-api-openapi.yml |
| API Reference Documentation | https://app.windmill.dev/openapi.html |
| Getting Started | https://www.windmill.dev/docs/getting_started/how_to_use_windmill |
| Authentication | https://www.windmill.dev/docs/core_concepts/authentification |
| HTTP Routes | https://www.windmill.dev/docs/core_concepts/http_routing |
| CLI | https://www.windmill.dev/docs/advanced/cli |
| Self Hosting | https://www.windmill.dev/docs/advanced/self_host |
| TypeScript SDK | https://www.windmill.dev/docs/advanced/clients/ts_client |
| Python SDK | https://www.windmill.dev/docs/advanced/clients/python_client |
| Spectral Rules | https://raw.githubusercontent.com/api-evangelist/windmill/refs/heads/main/rules/windmill-api-rules.yml |

## Common Properties

| Type | URL |
|------|-----|
| Portal | https://www.windmill.dev |
| Documentation | https://www.windmill.dev/docs/intro |
| Getting Started | https://www.windmill.dev/docs/getting_started/how_to_use_windmill |
| Pricing | https://www.windmill.dev/pricing |
| Plans | https://www.windmill.dev/docs/misc/plans_details |
| Blog | https://www.windmill.dev/blog |
| Changelog | https://www.windmill.dev/changelog |
| Roadmap | https://www.windmill.dev/roadmap |
| Login | https://app.windmill.dev/user/login |
| GitHub Org | https://github.com/windmill-labs |
| GitHub Repo | https://github.com/windmill-labs/windmill |
| Integration Hub | https://hub.windmill.dev |
| Status Page | https://windmill.betteruptime.com/ |
| Discord | https://discord.com/invite/V7PM2YHsPB |
| Community Forum | https://questions.windmill.dev/ |
| Terms Of Service | https://www.windmill.dev/terms_of_service |
| Privacy Policy | https://www.windmill.dev/privacy_policy |
| License Terms | https://www.windmill.dev/terms |
| Trust Center | https://trust.windmill.dev |
| Careers | https://www.windmill.dev/careers |
| Partners | https://www.windmill.dev/partners |
| Case Studies | https://www.windmill.dev/case-studies |
| Brand | https://www.windmill.dev/brand |

## Artifacts

### OpenAPI Specification
- [windmill-api-openapi.yml](openapi/windmill-api-openapi.yml) — 692 endpoints, OpenAPI 3.0.3, v1.694.0

### Spectral Rules
Custom Spectral ruleset enforcing Windmill API conventions.
- [windmill-api-rules.yml](rules/windmill-api-rules.yml)

### Naftiko Capabilities

#### Shared Definitions
- [capabilities/shared/windmill-api.yaml](capabilities/shared/windmill-api.yaml) — Full Windmill API consumer definition

#### Workflow Capabilities
- [capabilities/workflow-automation.yaml](capabilities/workflow-automation.yaml) — End-to-end workflow automation (scripts, flows, jobs, schedules, resources, variables, workspaces)

### JSON Schema
Generated JSON Schema files for key Windmill data types:
- [windmill-script-schema.json](json-schema/windmill-script-schema.json)
- [windmill-flow-schema.json](json-schema/windmill-flow-schema.json)
- [windmill-job-schema.json](json-schema/windmill-job-schema.json)
- [windmill-completedjob-schema.json](json-schema/windmill-completedjob-schema.json)
- [windmill-queuedjob-schema.json](json-schema/windmill-queuedjob-schema.json)
- [windmill-resource-schema.json](json-schema/windmill-resource-schema.json)
- [windmill-user-schema.json](json-schema/windmill-user-schema.json)
- [windmill-workspace-schema.json](json-schema/windmill-workspace-schema.json)
- [windmill-schedule-schema.json](json-schema/windmill-schedule-schema.json)
- [windmill-createvariable-schema.json](json-schema/windmill-createvariable-schema.json)

### JSON Structure
Structure definitions for key Windmill entities:
- [windmill-script-structure.json](json-structure/windmill-script-structure.json)
- [windmill-flow-structure.json](json-structure/windmill-flow-structure.json)
- [windmill-job-structure.json](json-structure/windmill-job-structure.json)
- [windmill-resource-structure.json](json-structure/windmill-resource-structure.json)
- [windmill-user-structure.json](json-structure/windmill-user-structure.json)
- [windmill-workspace-structure.json](json-structure/windmill-workspace-structure.json)
- [windmill-schedule-structure.json](json-structure/windmill-schedule-structure.json)

### JSON-LD Context
- [windmill-context.jsonld](json-ld/windmill-context.jsonld)

### Examples
Representative request/response examples:
- [windmill-run-script-example.json](examples/windmill-run-script-example.json)
- [windmill-list-completed-jobs-example.json](examples/windmill-list-completed-jobs-example.json)
- [windmill-create-script-example.json](examples/windmill-create-script-example.json)
- [windmill-get-workspace-example.json](examples/windmill-get-workspace-example.json)

### Vocabulary
- [windmill-vocabulary.yml](vocabulary/windmill-vocabulary.yml)

## Maintainers

**Kin Lane** — kin@apievangelist.com
