# Principles and Concepts

Healthcare Agents is a model-agnostic prompt and skills library for healthcare
administration work. Its purpose is to turn a healthcare operations problem into
structured, source-aware, human-reviewable work without presenting AI-generated
output as a clinical, legal, coding, billing, audit, or compliance decision.

## Core Principles

### Role specialization

The library uses narrow specialist roles instead of one general-purpose
healthcare assistant. Each role has an explicit domain, responsibilities,
required inputs, deliverables, escalation boundaries, source awareness,
operational workflows, and handoffs to adjacent specialists.

A specialist should own the primary analysis for a task. Supporting specialists
should be invoked only when the task crosses a clear role boundary.

### Task-based routing

Users describe an administrative problem first and the system routes it toward
a primary agent, supporting agents, an output mode, and any relevant workflow.
Routing is deterministic for the repository's workflow catalog and should prefer
the smallest useful specialist set.

The [agent selection guide](usage/agent-selection-guide.md) provides role-based
routing, while the [workflow gallery](workflows/README.md) provides canonical
operational workups.

### Model and platform portability

Agent prompts are Markdown assets with YAML frontmatter. The same source pack can
be adapted to subagent, skill, instruction, rule, prompt, or repository-context
formats supported by the target coding or agent runtime.

Portability is a design goal, not a promise of identical behavior across models.
Agents should remain usable with different models and tools, while runtime-specific
state, context limits, and tool availability remain explicit.

### Human authority

AI assistance can organize evidence, draft artifacts, identify risks, and prepare
questions. It cannot replace accountable clinical, legal, coding, billing,
compliance, contracting, pharmacy, audit, actuarial, or executive authority.

Outputs must distinguish:

- facts supplied by the user;
- facts derived from named sources;
- synthetic examples;
- assumptions and unknowns;
- recommendations for human review; and
- matters that require escalation.

### PHI and environment boundaries

The repository does not make an AI runtime safe for protected health information.
PHI is permitted only in an organization-approved environment with appropriate
contracts, access controls, retention, logging, business-associate arrangements,
and de-identification or minimum-necessary practices.

Public evidence packs and fixtures must not contain PHI, private payer contracts,
portal exports, or downloaded source payloads. Demo values must be synthetic and
provenance-labeled.

## Operational Concepts

### Agents

An agent is a focused, installable role prompt. It defines what the role may do,
what it may not decide, the inputs needed to work safely, the expected output,
handoff boundaries, and the source families relevant to the role.

The 51-agent catalog spans revenue cycle, clinical administration, quality,
compliance, operations, payer relationships, health IT, population health,
pharmacy, strategy, and emergency preparedness.

### Workflows

A workflow is a canonical healthcare administration workup with a primary
specialist, supporting handoffs, required evidence, missing questions, safety
boundaries, and a platform-ready prompt. Workflows connect natural-language
requests to repeatable repository artifacts.

Operator OS extends workflows with release-gated evidence metadata, citation
cards, fixtures, provenance rules, validation, coverage status, and review
ownership.

### Evidence packs

An evidence pack is committed, offline-first metadata about source families,
lookup paths, required fields, review owners, verification status, freshness,
and red flags. It does not claim that a live source was verified unless the
packet explicitly records that verification.

Citation cards use conservative statuses:

| Status | Meaning |
|---|---|
| `verified-pinpoint` | Exact local source text or citation was verified |
| `source-family-not-pinpoint` | The correct source family is identified, but exact text is not |
| `local-policy-required` | Final interpretation depends on local policy or a local accountable owner |
| `expired-review` | The evidence is stale and cannot be active in a release pack |

### Data modes and provenance

Runtime enrichment is explicit and fail-closed. Normal workups use the prompt and
committed metadata without live research or private-case ingestion.

| Data mode | Default behavior |
|---|---|
| `prompt_only` | Uses the prompt and user-supplied problem only |
| `public_evidence` | Attaches offline evidence-pack metadata |
| `synthetic_only` | Attaches deterministic synthetic fixture data |
| `hybrid_synthetic_public` | Combines synthetic fixtures with public evidence metadata |
| `public_search` | Reserved for future explicit refresh; disabled today |
| `internal_private` | Reserved for future governed private ingestion; disabled today |

Generated fields carry `synthetic`, `user_supplied`, or `source_derived`
provenance. Facts taken from a CLI problem must not be mislabeled as synthetic.

## Delivery and Validation

The repository treats prompts as maintained software artifacts. Changes should
preserve narrow role identity and pass the relevant release checks, including:

- agent registry and frontmatter validation;
- source-freshness validation;
- package and public-version synchronization;
- workflow, evidence-pack, citation-card, and fixture validation;
- install and uninstall behavior for supported runtime targets; and
- the frozen rubric and append-only evaluation results.

Evaluation scores are maintenance signals, not certification. They do not replace
source verification, domain review, or human approval.

## Review Model

A complete high-risk workflow separates work into three responsibilities:

1. **The specialist** performs scoped analysis and produces a structured artifact.
2. **The evidence and safety controls** expose provenance, source limitations,
   escalation triggers, and approved operating boundaries.
3. **The accountable human** verifies local facts, current primary sources,
   professional requirements, and final authorization.

This separation is also reflected in the versioned strategic-review protocols:
the system may analyze candidate assessments and preserve frozen evidence hashes,
but it does not make the professional disposition or own release authority.

## Related Documentation

- [Trust and Safety](trust-and-safety.md)
- [Agent Selection Guide](usage/agent-selection-guide.md)
- [Handoff Map](usage/handoff-map.md)
- [Operator OS Catalog](operator-os/catalog.md)
- [Evidence Pack Authoring](operator-os/evidence-pack-authoring.md)
- [Platform Guides](platforms/github-copilot.md)
