# Skill Registry — seedspec

**Project:** seedspec (Associated ERP — spec-only)
**Generated:** 2026-04-02
**Mode:** engram

---

## User Skills (from ~/.claude/skills/)

| Skill | Trigger Context | Path |
|-------|----------------|------|
| sdd-init | Initialize SDD context | ~/.claude/skills/sdd-init/SKILL.md |
| sdd-explore | Explore/investigate ideas before committing | ~/.claude/skills/sdd-explore/SKILL.md |
| sdd-propose | Create change proposal | ~/.claude/skills/sdd-propose/SKILL.md |
| sdd-spec | Write specifications with requirements/scenarios | ~/.claude/skills/sdd-spec/SKILL.md |
| sdd-design | Create technical design document | ~/.claude/skills/sdd-design/SKILL.md |
| sdd-tasks | Break down change into task checklist | ~/.claude/skills/sdd-tasks/SKILL.md |
| sdd-apply | Implement tasks from change | ~/.claude/skills/sdd-apply/SKILL.md |
| sdd-verify | Validate implementation against specs | ~/.claude/skills/sdd-verify/SKILL.md |
| sdd-archive | Sync delta specs and archive completed change | ~/.claude/skills/sdd-archive/SKILL.md |
| branch-pr | PR creation workflow | ~/.claude/skills/branch-pr/SKILL.md |
| issue-creation | GitHub issue creation workflow | ~/.claude/skills/issue-creation/SKILL.md |
| judgment-day | Parallel adversarial review protocol | ~/.claude/skills/judgment-day/SKILL.md |
| skill-creator | Create new AI agent skills | ~/.claude/skills/skill-creator/SKILL.md |
| skill-registry | Create/update skill registry | ~/.claude/skills/skill-registry/SKILL.md |
| find-skills | Discover and install agent skills | ~/.claude/skills/find-skills/SKILL.md |
| go-testing | Go testing patterns (NOT applicable here) | ~/.claude/skills/go-testing/SKILL.md |

---

## Project-Level Skills

None. No `.claude/skills/` directory exists in the project.

---

## Project Conventions (from AGENTS.md + spec/)

### Document Naming
- Spec files follow pattern: `NNN_kebab-case.md` under `spec/`
- Each document declares `**Inputs:** KB-xxx` header fields

### Traceability IDs (MANDATORY in all spec edits)
- RF: `NxRFyy` (e.g. N3RF01) — from 003_requisitos-funcionales.md
- RNF: `RNF-XXX` — from 004_rnf-base.md
- RNFT: `RNFT-XXX` — from 008_rnf-tecnicos.md
- BC: `BC-[Nombre]` — from 005_modelo-dominio.md
- ADR: `ADR-XXX` — from 006_adrs.md
- US: `US-XXX` — from 009_user-stories.md
- UC: `UC-XXX` — from 010_casos-uso.md
- ENT: `ENT-XXX` — from 012_modelo-de-datos.md
- EP: `EP-XXX` — from 013_inventario-de-endpoints.md

### Document Structure Template
```markdown
# [Título]
**Proyecto:** Associated
**Versión:** X.Y
**Fecha:** [date]
**Inputs:** [KB-xxx list]
**Estado:** Borrador | Revisión | Aprobado

---
[Contenido]

---
## Trazabilidad
[Cross-reference matrix]

## Changelog
- vX.Y: [changes]
```

### Language Rule
- All spec content MUST be in Spanish
- Code identifiers may use English (per glosario-traducciones.md)

### Generation Order (strict — do not skip)
003 RF → 004 RNF → 005 BC → 006 ADR → 007 Stack → 008 RNFT → 009 US → 010 UC → 012 ENT → 013 EP

---

## Compact Rules (inject into sub-agents)

### SPEC-001: Traceability IDs are MANDATORY
Every spec change MUST maintain or add traceability references using the established ID system (NxRFyy, RNF-XXX, ADR-XXX, BC-Name, US-XXX, UC-XXX, ENT-XXX, EP-XXX). Never remove existing cross-references.

### SPEC-002: Document header fields are MANDATORY
Every spec document MUST start with: Proyecto, Versión, Fecha, Inputs, Estado fields. Inputs must list KB-xxx identifiers.

### SPEC-003: Language
All spec content is in Spanish. Code identifiers (class names, field names) may use English following glosario-traducciones.md conventions.

### SPEC-004: Generation order
Changes to upstream documents (e.g. ADRs) may require downstream propagation (Stack → RNFT → UC → ENT → EP). Always check and propagate impact.

### SPEC-005: No source code
This is a SPEC-ONLY project. Do NOT create source code files, package.json, or any implementation artifacts. The spec documents are the deliverable.

### SPEC-006: Cross-reference integrity
When adding a new requirement or entity, update ALL related documents downstream: a new RF may need a new US, UC, ENT, and EP.
