# Spec and build

## Configuration
- **Artifacts Path**: {@artifacts_path} → `.zenflow/tasks/{task_id}`

---

## Agent Instructions

Ask the user questions when anything is unclear or needs their input. This includes:
- Ambiguous or incomplete requirements
- Technical decisions that affect architecture or user experience
- Trade-offs that require business context

Do not make assumptions on important decisions — get clarification first.

---

## Workflow Steps

### [x] Step: Technical Specification
<!-- chat-id: 65e01a1b-fcc4-4a5a-9fcb-b1693a410cc0 -->

**Complexity Assessment**: Medium-Hard

Created comprehensive specification in `spec.md` that includes:
- Documentation strategy organized in phases (Phase 0: Core Concepts, Phase 1: Architecture, Phase 2: Implementation)
- README structure and content outline
- Visual design artifacts (4 diagrams identified, 2 high priority)
- Scope breakdown (MVP documentation vs. future extended docs)
- File creation plan
- Verification criteria

**Key decisions**:
- Focus on digestible core concepts first, not overwhelming detail
- Use concrete examples from brainstorm (AuthService, KafkaLib)
- Create visual diagrams to illustrate key innovations
- Defer implementation details to future docs/ folder
- Excalidraw diagrams are feasible with available information

Replaced generic Implementation step with 6 concrete documentation steps.

---

### [x] Step: Create Core README
<!-- chat-id: 2b85ed13-c49d-4998-b9f4-29a73d99e766 -->

Rewrite `README.md` following the structure defined in `spec.md`:
- Overview section (2-3 paragraphs)
- Problem statement
- Solution approach
- Key insight (graph-to-text rendering)
- Simple example (reference to docs/examples/)
- Comparison table (vs RAG, GraphRAG, Static Analysis)
- System components overview
- Current status and roadmap
- Design philosophy (4 key principles)

**Verification**:
- [x] README is scannable with clear headers
- [x] Core concept explained in 5-10 minutes of reading
- [x] Comparison to alternatives is clear
- [x] Current status accurately reflects design phase
- [x] Not overwhelming with implementation details

---

### [x] Step: Create Concrete Example
<!-- chat-id: 8848a8c6-3c41-455d-99da-b456c0d2a94a -->

Create `docs/examples/authservice-example.md` with concrete graph-to-text example:
- Small dependency graph (AuthService → KafkaLib, Redis)
- Show graph structure (nodes and edges)
- Show rendered text output
- Explain how this helps LLM reasoning
- Use real content from the brainstorm

**Verification**:
- [x] Example is concrete and realistic
- [x] Before/after clearly shown
- [x] Demonstrates the key innovation

---

### [x] Step: Create Diagram Specifications
<!-- chat-id: 9371ea1d-d782-4f94-9426-7067a019ce6f -->

Create detailed specifications document for Excalidraw diagrams:
- Document in `docs/diagrams/diagram-specs.md`
- Include specifications from `spec.md` for:
  - Diagram 1: System Overview (data flow, two-phase construction)
  - Diagram 2: Graph-to-Text Example (concrete rendering)
  - Diagram 3: Phase Roadmap (optional/future)
- Provide example content, labels, colors, layout guidance
- User will create the actual Excalidraw files manually

**User Action Required**:
After this step completes, user will manually create:
- `docs/diagrams/system-overview.excalidraw` + PNG export
- `docs/diagrams/graph-to-text-example.excalidraw` + PNG export

**Verification**:
- [x] Specifications are detailed enough to create diagrams
- [x] Example content provided (node labels, edge labels, text)
- [x] Color coding specified
- [x] Layout guidance clear

---

### [x] Step: Create docs/ Directory Structure
<!-- chat-id: 95cdb7f0-ab89-4bf5-a1c9-d65fe386fdaf -->

Set up documentation organization for future content:
- Create `docs/` directory
- Create `docs/examples/` directory
- Create `docs/diagrams/` directory
- Create `docs/architecture/` directory (placeholder for future)
- Create `docs/implementation/` directory (placeholder for future)
- Add `.gitkeep` files or placeholder READMEs as needed

**Verification**:
- [x] Directory structure matches spec.md plan
- [x] Placeholder READMEs explain what will go in each folder
- [x] Links from main README point to correct locations

---

### [x] Step: Final Review and Report
<!-- chat-id: f28203f5-f293-47ab-a96f-9b0d46e60e2a -->

Review all documentation and create completion report:
1. Verify all README sections are complete
2. Check that diagrams render correctly
3. Ensure example is clear and helpful
4. Validate links between documents work
5. Read through as a first-time visitor

Create `{@artifacts_path}/report.md` with:
- What documentation was created
- How it was verified
- Suggested next steps (future phases)
- Any open questions or decisions deferred
