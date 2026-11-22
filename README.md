# Portable Engineering Harness

**Blueprint and build authority for an AI-first, portable engineering system.**

This repository defines the architecture, contracts, lifecycle, execution
policies, and implementation plan for a Harness that can be specialized for
new or legacy software projects while keeping the upstream blueprint clean.

> This repository contains the specification package. It is not an executable
> Harness source repository.

## What the Harness is

The Portable Engineering Harness is an operating model and engineering toolbox
for AI-assisted software delivery. It combines:

- Four engineering pillars: architecture, software engineering, backlog
  management, and development.
- AI-first project discovery instead of opaque pre-install scanning.
- Real specialist-agent delegation and evidence-backed orchestration.
- Spec-driven implementation, pipelines, reviews, and resumable runs.
- Canonical project memory, handoffs, run logs, checkpoints, and evidence.
- Provider-neutral model access and replaceable integration adapters.
- Security, portability, observability, evaluation, and controlled learning.
- Explicit authority boundaries for architecture, homologation, GMUD, and
  production deployment.

## Separation of concerns

The repository contains the **upstream blueprint**. A project receives a
**local copy/instance** that takes the shape of that project:

- Blueprint repository: generic rules and reusable contracts.
- Managed target: project-specific instructions, memory, adapters, and evidence.
- Harness source: executable implementation maintained separately.
- Verification artifacts: synthetic or real evidence for the target project.

Results and decisions collected in a project must not silently become upstream
rules. Generalization requires a separate review of the evidence, removal of
project-specific data, creation of synthetic fixtures, and explicit approval.

## Repository structure

```text
engineering-harness-blueprint/
  01_PRODUCT_VISION_AND_REQUIREMENTS.md
  02_ARCHITECTURE.md
  03_E2E_PIPELINE.md
  ...
  24_PLANO_MESTRE_DE_CONSTRUCAO_DO_HARNESS.md
  25_VALIDACAO_AUTONOMA_DE_PROJETOS.md
  AGENTS.md
  AGENT_START_HERE.md
  BLUEPRINT_STATUS.yaml
  MEMORY.md
  LOG.md
  PACKAGE_MANIFEST.json
  diagrams/
  examples/
  references/
```

## Start here

For implementation or maintenance work:

1. Read [`AGENTS.md`](engineering-harness-blueprint/AGENTS.md).
2. Read [`AGENT_START_HERE.md`](engineering-harness-blueprint/AGENT_START_HERE.md).
3. Use [`BLUEPRINT_STATUS.yaml`](engineering-harness-blueprint/BLUEPRINT_STATUS.yaml)
   to identify the current state.
4. Follow [`24_PLANO_MESTRE_DE_CONSTRUCAO_DO_HARNESS.md`](engineering-harness-blueprint/24_PLANO_MESTRE_DE_CONSTRUCAO_DO_HARNESS.md)
   as the ordered build backlog.
5. Consult [`25_VALIDACAO_AUTONOMA_DE_PROJETOS.md`](engineering-harness-blueprint/25_VALIDACAO_AUTONOMA_DE_PROJETOS.md)
   for the stack-neutral autonomous validation procedure.

## Core invariants

- AI remains the orchestrator; deterministic helpers provide bounded evidence,
  not hidden architectural decisions.
- The Master Agent coordinates real specialist agents; it does not replace them.
- An approved architecture baseline cannot be reopened without evidence and
  the owner's authorization.
- Project memory is canonical for project decisions. External memory remains
  retrieval support.
- Production deployment is outside normal build and implementation boundaries.
- Upstream releases, local instances, provider states, platform support, and
  test evidence must remain explicit and versioned.

## Documentation model

The specification starts with requirements and architecture, then covers:

- AI-first onboarding and workspace identity.
- Execution adapters and real delegation.
- Specialist pillars and dynamic pipelines.
- Durable state, memory, evidence, handoffs, and recovery.
- Security and portability.
- Integration adapters and provider capability states.
- Observability, evaluation, learning, and release readiness.
- Autonomous target validation and evidence collection.

## Integrity

The blueprint package is integrity-checked with:

- [PACKAGE_MANIFEST.json](engineering-harness-blueprint/PACKAGE_MANIFEST.json)
- Relative file sizes.
- SHA-256 hashes for every recorded package file.

## Status

The repository is a specification package used to build and verify the
Portable Engineering Harness. It does not claim that every referenced provider
is installed or that every executor/platform integration has live smoke
evidence.
