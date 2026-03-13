# Working Proposal: Domain Connect Enhancements for Machine-Native DNS Operations

**Status:** Working proposal  
**Audience:** Domain Connect WG members, DNS providers, service providers, implementers, sponsors  
**Author:** SeamlessDNS  
**Discussion status:** Seeking feedback

## Summary

The current version of Domain Connect solves a valuable and important problem: how a service asks a DNS provider to apply known DNS changes.

That model is useful for human-approved, template-based SaaS DNS provisioning. But it is weaker for machine-native execution, richer operation types, dynamic and policy-aware requests, Infrastructure-as-Code workflows, shared multi-provider orchestration, and the broader operational ecosystem required to make Domain Connect the default open implementation layer.

This proposal outlines a set of enhancements and adjacent workstreams intended to modernize Domain Connect for the next era of DNS automation, while preserving compatibility with the core protocol and avoiding duplication of work already underway in the base standard.

This is not a replacement for the current Domain Connect standardization effort. It is a gap analysis and working proposal for what may come next.

## Problem Statement

Domain Connect has demonstrated that DNS setup can be easier, safer, and more interoperable than manual record entry.

However, several ecosystem realities limit its impact today:

- adoption is still too limited
- interoperability does not scale smoothly across providers
- the model is centered on static templates and human-driven flows
- richer operational use cases remain underserved
- conformance, transparency, and neutral common-good infrastructure are still underdeveloped

At the same time, the web is becoming more automated and more agentic. More software is acting on behalf of users and organizations. Those systems will need ways to securely bootstrap, authorize, execute, verify, and operate DNS changes across many providers in machine-native workflows.

The goal of this proposal is to define the next layer of Domain Connect evolution: not to replace its successful core, but to extend it where necessary and build the operational ecosystem around it.

## Non-Goals

This proposal does **not** aim to:

- replace the current Domain Connect base protocol
- duplicate work already covered by the active IETF draft
- collapse protocol work, hosted service operations, governance, and business model into a single standards-track document
- require that all future enhancements be part of the core standard
- make agent bootstrap records part of this document; that work is better treated as a separate, adjacent standard

## Current Limitations

### 1. Adoption is too limited to fulfill the original promise

Domain Connect is valuable in principle, but it has not achieved the level of adoption needed to become the default way domains are connected to services. Too much of the ecosystem still relies on manual DNS setup, custom integrations, or expensive proprietary intermediaries.

### 2. Interoperability does not scale well across providers

The current model does not yet provide a robust public-utility layer that makes cross-provider DNS automation work smoothly at scale. Differences between DNS providers still create friction for service providers, users, and implementers.

### 3. The templating model is too rigid

Current Domain Connect is centered on prepublished templates. That works for many fixed SaaS flows, but it is limiting for more dynamic or runtime-specific operations. There is a need for dynamic intents and signed runtime payloads, not just static template definitions.

### 4. Operation types are too limited

Current Domain Connect is primarily good at a narrow class of “apply these records” operations. It does not fully support a safer, richer CRUD-style model with guardrails for controlled list, read, update, and delete actions.

### 5. The workflow model works for only one use case

The current protocol is mostly designed around a service asking a DNS provider to write records. That is useful, but incomplete. Some important domains of automation, especially DNSSEC, need workflows that move between different providers and systems, rather than a single one-way model.

### 6. It is still too human-driven

Current Domain Connect assumes a user-driven browser flow or consent step. That is workable for classic SaaS onboarding, but not ideal for a future where software agents and automated systems need to execute DNS operations directly within machine-driven environments.

### 7. It is not designed for the agentic web

The web is becoming more agentic: more software acting on behalf of people and organizations. These systems will need delegated DNS operations and related automated workflows. Current Domain Connect does not explicitly solve for that environment.

### 8. There is no standard discovery and negotiation layer

Providers need a better way to advertise what they support: profiles, constraints, capabilities, policy rules, operation types, and limits. Current Domain Connect does not provide enough capability discovery and negotiation to support a more heterogeneous ecosystem.

### 9. There is no standard policy evaluation model for runtime requests

If requests become more dynamic, providers need a safe way to evaluate them at runtime. Current Domain Connect does not provide a standard policy engine model or ruleset framework for allowing or denying dynamic requests safely.

### 10. Infrastructure-as-Code is not first-class

Current Domain Connect is mostly framed around interactive service onboarding. It is not yet well shaped for enterprise operations models like Terraform, Pulumi, GitOps, or IaC-first environments, where discovery, distribution, and automation need to be native.

### 11. Conformance and transparency are too weak

There is not yet a strong, open conformance suite with public scorecards that would make adoption easier, reveal interoperability gaps, and create ecosystem pressure toward better implementations.

### 12. The ecosystem lacks a neutral hosted service layer

There is a protocol, but not yet a true “public utility layer” for Domain Connect: a neutral, common-good hosted service that anyone can use, that reduces integration cost and becomes shared infrastructure for the ecosystem.

### 13. The ecosystem is vulnerable to pay-to-play dynamics

Today, DNS automation can become gated by expensive commercial intermediaries, listing control, approval bottlenecks, or vendor-controlled pathways. That limits openness and makes adoption harder, especially for smaller providers and services.

### 14. The ecosystem lacks strong operational support

Even with a base standard, there is still a need for real implementation resources: hosted services, conformance tooling, reference implementations, provider connectors, policy engines, signed artifacts, mirrorable registries, and full-time staff.

### 15. It is not yet built to be non-capturable

A public utility layer needs to be multi-operator, mirrorable, signed, forkable, and independent of any single host or funder. Current Domain Connect does not yet have that broader institutional and operational model attached to it.

## Design Goals

Any next layer of Domain Connect evolution should aim to be:

- **compatible** with the current protocol where possible
- **machine-native**, not only human-driven
- **safe**, with explicit guardrails and policy control
- **interoperable** across many DNS providers and service types
- **discoverable**, with capability and profile negotiation
- **auditable**, with better status, error, and conformance visibility
- **IaC-friendly**, so modern infrastructure workflows are first-class
- **open and non-capturable**, with mirrorable artifacts and multi-operator viability
- **incremental**, allowing extension profiles rather than requiring a single monolithic redesign

## Proposed Enhancement Areas

The following areas represent the main enhancement themes.

### A. Dynamic intents and signed runtime payloads

Extend beyond prepublished static templates by allowing well-defined runtime payloads that can be signed, validated, and evaluated safely.

Potential goals:
- support dynamic but constrained requests
- reduce the need to pre-register every possible variation
- preserve safety and auditability

Open questions:
- how should runtime payloads be signed
- how should providers validate authenticity and intent
- what compatibility model exists with existing templates

### B. Policy engine model for request evaluation

Define a standard model for how providers can evaluate dynamic requests using configurable rulesets.

Potential goals:
- allow providers to accept or reject dynamic requests safely
- give providers local control without breaking interoperability
- make policy decisions transparent and machine-readable

Potential outputs:
- policy model
- error taxonomy
- decision/result schema
- reference policy engine

### C. Capability discovery and negotiation

Allow providers to advertise supported profiles, constraints, operation types, policy requirements, limits, and optional features.

Potential goals:
- reduce guesswork for services and clients
- enable profile negotiation before execution
- support heterogeneous provider capabilities cleanly

Potential outputs:
- discovery document format
- profile/version negotiation rules
- capability registry or mirrorable artifacts

### D. Safe CRUD profile

Define a constrained and auditable profile for controlled list, read, update, and delete operations with explicit safety boundaries.

Potential goals:
- move beyond one-time record creation
- support managed lifecycle operations
- prevent unsafe or overly broad mutations

Potential outputs:
- operation taxonomy
- allowed object scope
- resource identity rules
- conflict and rollback behavior

### E. Bidirectional workflow support

Support cases where the current one-way “service asks provider to write records” model is insufficient.

A key example is DNSSEC-related flows, where information and state may need to move between multiple systems or providers.

Potential goals:
- support multi-party workflows
- define status handoffs and job coordination
- preserve clear responsibility boundaries

### F. IaC-first profiles and distribution

Treat Infrastructure-as-Code as a first-class operating model.

Potential goals:
- support Terraform, Pulumi, GitOps, and related workflows
- make discovery, profile selection, and execution automatable
- enable artifacts and modules to be distributed in standard ways

Potential outputs:
- provider profile metadata suitable for IaC
- standard module formats or mappings
- examples and reference modules

### G. Jobs, status, and error taxonomy

Make execution and verification easier to reason about with better asynchronous job handling and machine-readable status models.

Potential goals:
- improve traceability and retries
- support asynchronous and multi-step flows
- standardize errors across providers

Potential outputs:
- job lifecycle model
- event schema
- standardized result and error codes

### H. Conformance suite and public scorecards

Create open conformance tests and public reporting to improve ecosystem quality and transparency.

Potential goals:
- make adoption easier
- reveal interoperability gaps
- provide neutral incentives toward better implementation quality

Potential outputs:
- conformance test suite
- implementation badges or profiles
- public scorecard methodology

### I. Hosted common-good service layer

Build and operate a neutral hosted Domain Connect service as common infrastructure for the ecosystem.

This is not necessarily a standards-track item, but it is central to making the protocol practical and broadly accessible.

Potential goals:
- reduce integration cost for providers and services
- provide a shared entry point for APIs, SDKs, and tooling
- demonstrate best practices through a reference implementation

Potential outputs:
- hosted service
- common API / SDK / CLI
- mirrorable signed artifacts
- multi-operator compatibility model

## Classification of Work

Not all of the above belongs in the same venue. A key part of this proposal is to separate concerns clearly.

### Candidate standards-track extension work

These may merit future IETF standardization if they prove useful and sufficiently interoperable:

- dynamic intents and signed runtime payloads
- capability discovery and negotiation
- safe CRUD profile
- jobs, status, and error taxonomy
- bidirectional workflow extensions
- policy evaluation model, if standardized behavior is needed across providers

### Candidate informational or architecture work

These may be better published as informational drafts, design notes, or implementation guidance:

- architectural patterns for multi-provider orchestration
- machine-native execution flows
- IaC-first packaging and distribution guidance
- deployment and operator guidance

### Candidate project and ecosystem work

These are likely better handled as open project deliverables rather than protocol documents:

- hosted common-good service
- conformance suite and scorecards
- provider onboarding kits and connectors
- reference implementations
- signed registries and mirrorability tooling
- governance, nonprofit structure, and operational model

## Proposed Deliverables

### Near-term documents

- this working proposal
- gap analysis against the base Domain Connect standard
- enhancement matrix mapping each proposal to standards-track, informational, or project scope
- open issues list for design questions

### Near-term technical artifacts

- conformance suite v0
- capability discovery draft format
- intent and jobs schema drafts
- reference policy engine prototype
- at least one provider implementation / connector

### Near-term ecosystem artifacts

- public repo for discussion and issue tracking
- provider and service implementation guides
- transparency and scorecard framework
- draft principles for non-capturable common-good infrastructure

## Open Questions

- Which of these enhancements are best handled inside the Domain Connect WG?
- Which should remain implementation-layer profiles rather than core protocol changes?
- Is a single extension framework sufficient, or should these be split into multiple focused documents?
- What is the minimal viable set of enhancements needed to unlock materially better adoption?
- Which provider use cases should be treated as the design center for vNext work?
- How should safety and interoperability be balanced when moving from static templates to dynamic intents?
- What should be the first conformance target?

## Initial Recommendations

1. Treat this document as a working proposal, not a finished specification.
2. Socialize it with WG members, providers, and implementers before drafting any large “v2” standard.
3. Split protocol extensions from hosted-service and ecosystem work early.
4. Prefer small, composable extension documents over a single monolithic replacement.
5. Prioritize concrete interoperability wins first:
   - capability discovery
   - jobs/status/error taxonomy
   - conformance tooling
   - one or two narrowly scoped dynamic-intent use cases

## Call for Feedback

Feedback is especially welcome on:

- which pain points are most important in practice
- which enhancements belong in the protocol versus in implementation
- what should be the first concrete technical deliverable
- which provider and service use cases should guide design
- where current Domain Connect already solves the problem well enough

## Appendix A: Working shorthand

A concise way to describe this proposal is:

> Domain Connect v1 solves how a service asks a DNS provider to apply known DNS changes.
>
> The next layer should solve how services and automated systems securely authorize, execute, verify, and operate DNS changes across many providers in a machine-native way.

## Appendix B: Why now

The ecosystem now has a chance to do for DNS automation what other public-interest infrastructure projects have done in adjacent areas: combine open specifications, reference implementations, conformance tooling, and neutral common-good operations into something that can scale broadly without becoming closed or pay-to-play.

The goal is not only a better protocol, but a more usable and durable ecosystem.
