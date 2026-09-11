---
name: diagrams
description: >
  Generate Mermaid diagrams from OpenSpec artifacts. Creates requirements-level
  visualisations from specs and technical diagrams from design documents.
  Use when the user asks for diagrams, architecture visuals, or flow charts
  for an OpenSpec change.
---

# Diagrams Skill

Generate Mermaid diagrams for an OpenSpec change using two independent modes.

## Applicability

Activate when the user requests diagram generation or updates for an OpenSpec
change. This skill reads planning artifacts and produces `.diagrams.md` files
alongside them.

## Workflow Modes

### Requirements Mode (from specs)

Reads: `specs/<capability>/spec.md` files in the active change.
Writes: `spec-diagrams.md` in the change root.

Generates non-technical, user-focused visualisations:
- **User Journey Map**: Priority-ordered flowchart of user stories with dependencies
- **Actor-Goal Overview**: System interactions showing which actors pursue which goals
- **Entity Relationship Map**: Conceptual entity connections (only if specs define key entities)
- **Status Lifecycle**: State progressions and triggers (only if specs describe lifecycle states)

### Technical Mode (from design)

Reads: `design.md` in the active change.
Writes: `design-diagrams.md` in the change root.

Produces implementation-level component diagrams:
- **Component Dependencies**: Layered flowchart of all architectural elements
- **Sequence Diagram**: End-to-end flows including happy path, error scenarios, and HITL paths
- **Workflow State Machines**: One subsection per stateful component showing state transitions

## Steps

1. **Identify the active change**

   Run `openspec list --json` if no change is specified by the user.
   Confirm which change to generate diagrams for.

2. **Check which artifacts exist**

   Run `openspec status --change "<name>" --json` to find:
   - `specs` artifact status and paths
   - `design` artifact status and path

3. **Select mode(s)**

   - If specs exist -> run Requirements Mode
   - If design exists -> run Technical Mode
   - If both exist -> run both modes
   - If neither exists -> inform the user and stop

4. **Read source artifacts**

   - For Requirements Mode: read all `specs/<capability>/spec.md` files
     listed under the change's artifact paths
   - For Technical Mode: read `design.md` from the change root

5. **Generate diagrams**

   Use the templates (`spec-diagrams.template.md` or `design-diagrams.template.md`)
   as the output structure. Fill in each section based on content extracted from
   the source artifacts.

6. **Write output**

   Write diagram files to the change root:
   - `spec-diagrams.md` for Requirements Mode
   - `design-diagrams.md` for Technical Mode

7. **Report results**

   Show which diagram sections were generated and which were omitted (with reason).

## Critical Constraints

- Omit diagram sections entirely if the prerequisite content does not exist
  in the source artifacts. Never invent data to fill a diagram.
- Use exact names and terminology from the source artifacts. Do not rename
  entities, states, or actors.
- Use only Mermaid-compatible syntax:
  - Sequence diagrams: use `-->>` for dotted async arrows, never `-.>>`
  - Flowcharts: use `-.->` for dotted arrows to external systems
- Cap sequence diagrams to one per Technical Mode execution.
- Diagrams are supplementary artifacts. They do NOT block the apply phase
  and are not tracked by OpenSpec status.