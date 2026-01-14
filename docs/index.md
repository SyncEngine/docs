# Welcome to SyncEngine

SyncEngine is a modular automation and data synchronization engine designed for building reliable, transparent, and
high-performance integrations between systems.

It combines the flexibility of custom integrations with a structured execution model that provides previews,
execution history, and deep observability out of the box.

SyncEngine is built for people who care not just that an integration runs, but why, how, and with what data it ran.

Whether you’re building advanced automation flows, connecting third-party APIs, or orchestrating entire business
processes, SyncEngine provides the performance, flexibility, and transparency you need.

## What is SyncEngine?

SyncEngine is a modular automation framework that lets you define, execute, and inspect data flows between systems, 
running as a self-hosted service or managed platform.

At its core, it provides:

- Explicit automation flows composed of reusable routines and tasks
- Preview and live execution modes
- Structured traces that record execution history
- An extensible architecture for both prebuilt modules and custom logic

You can use SyncEngine to:

- Synchronize data between platforms (e.g. webshops, PIMs, ERPs, CRMs)
- Orchestrate complex, multi-step business processes
- Build robust integrations without reinventing logging, retries, and execution tracking
- Replace fragile custom scripts with a maintainable integration framework

## Key Capabilities

- **Flows & Routines**  
Build automations from reusable building blocks with predictable execution.
- **Preview & Live Execution Modes**  
Inspect data and behavior before running an automation live.
- **Execution History (Traces)**  
Every run is recorded with inputs, outputs, decisions, and metadata.
Trace retention is configurable and automatically managed.
- **Built-in Observability**  
Understand what happened, when it happened, and why.
- **Extensible by Design**  
Add custom tasks or install modules without modifying Core.

---

## Quick Start

If you’re new to SyncEngine, we recommend starting here:

1. [Installation](user/installation.md) – Set up SyncEngine using Docker, the Installer, or Composer.
2. [Setup](user/setup.md) – Complete the initial configuration and create your admin account.
3. [Dashboard](user/dashboard.md) – Learn how to use the dashboard and preview features.
4. [Tasks](user/tasks.md) – Understand how SyncEngine processes data using tasks.
5. [Connections](user/connections.md) – Connect to your favorite apps and services.

---

## Documentation Overview

The documentation is divided into two main sections:

- **User Docs** – Installation, setup, dashboard, flows, routines, storages, and tasks.
- **Developer Docs** – Modules, API, and architecture for extending SyncEngine.

---

## Contributing

Bug reports 🐛, improvements ✨ and ideas 💡 are very welcome!  
If you’d like to improve the docs, report issues, or contribute modules, visit our GitHub repositories:

- [SyncEngine Core](https://github.com/SyncEngine/SyncEngine)
- [SyncEngine Docker](https://github.com/SyncEngine/docker)
- [SyncEngine Installer](https://github.com/SyncEngine/installer)

### Join the Team

Help us shape the future of Secure, Scalable and Automated Data Synchronization!
Please contact us at [info@syncengine.io](mailto:info@syncengine.io)

---

## Why SyncEngine?

Many automation tools focus on getting something running as quickly as possible.
They often trade long-term clarity, debuggability, and performance for convenience.

SyncEngine takes a different approach:

- It treats execution history as a first-class concept, not just logs.
- It makes previewing and live execution explicit choices.
- It favors predictable, inspectable behavior over hidden automation.
- It is designed to handle recurring and bulk synchronizations reliably.

---

## License Comparison

The Core License is designed for developers, agencies, and companies
who use SyncEngine as part of their own systems or client projects,
without offering SyncEngine itself as a hosted or multi-tenant service.

If you want to offer SyncEngine as a public SaaS, managed hosting
platform, or large-scale multi-tenant solution, a Commercial or
Enterprise License is required.

This licensing model ensures long-term sustainability of the project
while remaining friendly to integrators, consultants, and embedded
use cases.

The table below highlights the practical differences between the
SyncEngine Core License and the Commercial / Enterprise License.

| Feature / Use Case                         | Core License                         | Commercial / Enterprise License |
|-------------------------------------------|--------------------------------------|---------------------------------|
| Personal / learning use                   | Allowed                              | Allowed                          |
| Agency / client-specific deployments      | Allowed (per client)                 | Allowed                          |
| Internal business applications            | Allowed                              | Allowed                          |
| Embedded use inside a SaaS application    | Allowed (not exposed as standalone)  | Allowed                          |
| Public SaaS offering of SyncEngine        | ❌ Not allowed                       | ✅ Allowed                       |
| Multi-tenant deployments                  | ❌ Not allowed                       | ✅ Allowed                       |
| Managed hosting / PaaS offerings          | ❌ Not allowed                       | ✅ Allowed                       |
| Competing SaaS platform                   | ❌ Not allowed                       | Restricted / separate agreement |
| Automated provisioning / deployment       | ❌ Not allowed                       | ✅ Allowed                       |
| Enterprise-only features or modules       | Limited                              | ✅ Allowed                       |
| Modules Marketplace participation         | Optional                             | Optional                         |
| Commercial support / SLA                  | Not included                         | Optional / contractual           |

---

## Learn More

- [Installation Guide](user/installation.md)
- [Setup Guide](user/setup.md)
- [Dashboard Overview](user/dashboard.md)
- [Task Reference](user/tasks.md)