# Graph-Augmented-Generation (GAG)

A hybrid knowledge graph system that combines static program analysis with LLM-based semantic enrichment to create a living, queryable representation of codebases.

## Overview

Large codebases present a fundamental challenge for AI coding agents: limited context windows. When working across multiple services or repositories, agents struggle to understand dependency chains, cross-service relationships, and historical context. Traditional approaches like passing markdown spec files between agents have limitations—they can't capture the dynamic, interconnected nature of real code.

GAG solves this by building a **Code Knowledge Graph** where nodes represent code entities (services, libraries, functions) and edges represent verified relationships (dependencies, conflicts, interactions). Unlike document-based approaches, this graph combines the precision of static code analysis with the semantic understanding of LLMs. The graph stores both structured facts (extracted from code) and semantic meaning (added by LLMs).

The key innovation: instead of making LLMs reason about graph structures directly (which they're bad at), we execute graph queries to find relevant information, then **render subgraphs as natural language** before sending to the LLM. This gives agents structured, queryable context without overwhelming their context window.

## The Problem

**Passing information between coding agents via spec files has limitations:**

- Static markdown can't represent dynamic dependencies and relationships
- No way to query "what depends on this service?" or "what conflicts exist?"
- Can't trace transitive dependencies across repos
- Historical context (debugging sessions, known issues) gets lost
- LLMs must re-discover relationships that static analysis could prove

**Example**: An agent debugging AuthService latency needs to know:
- What does AuthService depend on? (KafkaLib, Redis, JWT library)
- What depends on AuthService? (UserService, PaymentService)
- Are there version conflicts? (KafkaLib v2.3 conflicts with MetricsLib due to ProtoBuf mismatch)
- What was tried before? (Previous investigation increased Redis pool size)

This context spans multiple files, repos, and historical knowledge. Markdown specs can't capture these interconnections.

## The Solution

**A Code Knowledge Graph with two-phase construction:**

### Phase 1: Structural Analysis (Deterministic)
Static analysis tools extract ground truth:
- Parse AST for imports, function calls, class hierarchies
- Extract dependencies from package manifests
- Analyze git history for change patterns
- Map cross-repo service-to-service calls

**Result**: Verified graph structure with precise relationships

### Phase 2: Semantic Enrichment (LLM-Powered)
Add meaning that static analysis can't extract:
- Describe purpose and responsibilities
- Annotate why dependencies exist ("for JWT validation", "event streaming")
- Detect version conflicts and constraints
- Document gotchas from debugging sessions

**Result**: Graph that understands both structure AND meaning

### Phase 3: Query & Rendering
When an agent needs context:
1. Execute graph query (e.g., "get dependencies of AuthService, depth 2")
2. Render subgraph as structured text with priorities
3. Send compiled text to LLM
4. LLM reasons about the text, not raw nodes/edges

## Key Insight

**LLMs are bad at traversing graph structures but excellent at reasoning about natural language.**

Instead of sending raw graph data:
```json
{"nodes": [{"id": "auth", "type": "service"}, ...], "edges": [...]}
```

We render it as readable text:
```
AuthService (v2.3.1)
Handles user authentication via JWT tokens

Known Constraints:
⚠ Version conflict: ProtoBuf mismatch (3.21 vs 3.19)

Dependencies:
- KafkaLib v2.3: Event streaming for user actions
  ⚠ Conflicts with MetricsLib v1.5 (protobuf version mismatch)
  Last updated 3 weeks ago, stable
```

The graph stores relationships deterministically. The rendering algorithm compiles relevant context. The LLM receives structured, readable text.

## How It Works

**See [System Overview Diagram](./docs/diagrams/) (to be created)**

```
Code Repos → Static Analysis → Graph Database → Graph Query → Render to Text → LLM Reasoning
              (AST, deps)      (nodes + edges)   (traversal)   (structured)    (insights)
```

**Two-phase construction:**
1. **Blue (Deterministic)**: Code → AST parser → Graph nodes and edges
2. **Orange (LLM)**: Graph → LLM analysis → Annotated nodes with descriptions, gotchas, constraints

**Query time (Green)**: Agent task → Graph query → Render subgraph to text → LLM receives context

### Example

For a concrete example of graph-to-text rendering, see [AuthService Example](./docs/examples/authservice-example.md).

**Graph structure:**
- AuthService depends on KafkaLib v2.3 (for event streaming)
- AuthService depends on MetricsLib v1.5 (for telemetry)
- KafkaLib depends on ProtoBuf v3.21
- MetricsLib depends on ProtoBuf v3.19
- **Conflict detected**: Two ProtoBuf versions

**Rendered text shows:**
- Direct dependencies with purpose annotations
- Detected conflicts with explanations
- Transitive dependency chains
- Historical context from past debugging

The LLM can now reason: "Two different ProtoBuf versions → runtime conflict likely. Need to align versions or isolate dependencies."

## Comparison to Alternatives

| Approach | Structure | Semantics | Best For | Limitations |
|----------|-----------|-----------|----------|-------------|
| **Traditional RAG** | Vector embeddings | Similarity-based | Unstructured docs | No explicit relationships |
| **GraphRAG** | LLM-extracted graph | Fuzzy relationships | Document corpora | Graph quality depends on LLM extraction |
| **Static Analysis** | AST/dependency tree | None | Code navigation | No semantic understanding |
| **GAG (This)** | Verified code structure | LLM-enriched | Cross-repo codebases | Requires setup and maintenance |

**When to use GAG:**
- Multi-service or multi-repo codebases
- Complex dependency chains
- Need to track transitive impacts (e.g., "what breaks if we upgrade X?")
- Want to accumulate debugging knowledge over time

**When to use alternatives:**
- Single small codebase: Static analysis tools sufficient
- Unstructured documentation: Traditional RAG works well
- Document analysis: GraphRAG for extracting relationships from text

## System Components

### Phase 1: Structural Analysis (Deterministic)

**Tools needed:**
- AST parsers (tree-sitter or language-specific)
- Dependency scanners (package.json, go.mod, requirements.txt)
- Code pattern matching (ripgrep, AST queries)
- Git analysis tools

**Outputs:**
- Service nodes with metadata (version, owner, file paths)
- Library nodes with version info
- Edges: `depends_on`, `calls_api`, `co_changes_with`

### Phase 2: Semantic Enrichment (LLM-Powered)

**Process:**
- For each node, LLM analyzes code and provides:
  - One-sentence description
  - Primary responsibilities
  - Gotchas and edge cases
  - Known constraints
- For each edge, LLM annotates why the relationship exists

**Outputs:**
- Enriched nodes with human-readable descriptions
- Annotated edges ("for JWT validation", "event streaming")
- Detected conflicts with severity levels

### Phase 3: Query & Rendering

**Graph-to-text rendering with priority tiers:**

**Tier 1 (Always include):**
- Target node description
- Critical constraints and warnings

**Tier 2 (High priority):**
- Direct dependencies with annotations
- Known conflicts
- Recent investigations (last 30 days)

**Tier 3 (Medium priority):**
- Reverse dependencies (what uses this?)
- Related concepts
- Co-change patterns

**Tier 4 (If budget allows):**
- Transitive dependencies (depth 2)
- Historical context (older investigations)

**Token budget management**: Render in priority order until budget exhausted, ensuring critical info always included.

## Current Status

**Phase**: Design & Documentation

- ✅ Core concept defined
- ✅ Architecture designed (graph schema, rendering algorithm, agent workflows)
- ✅ Comparison to alternatives researched
- ⏳ Documentation in progress
- ⏳ Visual diagrams planned
- ⬜ Implementation (not started)

**Next steps:**
1. Complete documentation with examples and diagrams
2. Build MVP with single-repo support
3. Test with real debugging scenarios
4. Iterate based on usefulness

## Roadmap

### Documentation Phase (Current Focus)
- **Goal**: Make the concept clear and digestible
- Simple examples with before/after
- Visual diagrams showing system flow
- Comparison to alternatives
- **Status**: In progress

### MVP (Future)
- **Goal**: Prove the concept works
- Single repository support
- Basic graph construction (Phase 1 only)
- Simple text rendering
- Test with real debugging tasks
- **Timeline**: TBD

### V2 (Future)
- Multi-repo support
- Investigation recording system
- Incremental graph updates
- Conflict detection
- Advanced rendering (priority-based)

### V3 (Future)
- Vector similarity for finding related issues
- Community detection for service clusters
- CI/CD integration for automatic updates
- Dashboard for graph visualization

## Why Not Just Use [X]?

### vs. Traditional RAG

**RAG approach:**
- Chunks code/docs into pieces
- Embeds chunks as vectors
- Retrieves similar chunks via semantic search

**Limitation**: No explicit relationships. Can't answer "what depends on this?" or "what conflicts exist?" Must rely on co-occurrence in retrieved chunks.

**GAG advantage**: Uses verified code structure (imports, dependencies) as ground truth, then adds semantic meaning on top.

### vs. GraphRAG (Microsoft)

**GraphRAG approach:**
- Extracts entities and relationships from unstructured text using LLMs
- Builds graph from extracted information

**Limitation**: Graph quality depends on LLM extraction accuracy. Relationships are inferred, not proven.

**GAG advantage**: Starts with precise relationships from static code analysis (provably correct), then LLM adds semantic understanding to verified structure.

**Key difference**: GraphRAG extracts fuzzy graphs from text. GAG builds precise graphs from code, then adds semantic fuzziness on top.

### vs. Static Analysis Tools (SonarQube, Sourcegraph)

**Static analysis:**
- Extracts code structure and dependencies
- Provides code navigation and search
- Detects patterns and anti-patterns

**Limitation**: No semantic understanding. Can't explain why dependencies exist or annotate gotchas. Doesn't learn from debugging sessions.

**GAG advantage**: Combines structural precision with semantic understanding. Learns and improves over time as investigations are recorded.

## Design Philosophy

### 1. Graph stores truth, LLM adds meaning
Static analysis provides verified facts (this imports that, this calls that). LLM provides interpretation and context (why it imports, what could go wrong).

### 2. Render graphs as text, don't make LLMs traverse them
Graph databases execute queries efficiently. LLMs reason about natural language effectively. Keep each in their domain of strength.

### 3. Learn from investigations
Record debugging sessions as Investigation nodes. Extract constraints from failures. Build institutional knowledge that improves context over time.

### 4. Context is assembled, not stored
No static markdown files that go stale. Context is dynamically compiled from graph queries based on what the agent needs right now.

## Contributing

This project is currently in the **design phase**. The architecture and approach are being documented before implementation begins.

**Feedback welcome on:**
- Core concept and approach
- Comparison to alternatives
- Use cases and applications
- Technical design decisions

**Not yet ready for:**
- Code contributions (no implementation yet)
- Feature requests (still defining MVP)

## References

- [Detailed Examples](./docs/examples/) - Concrete graph-to-text rendering examples
- [System Diagrams](./docs/diagrams/) - Visual architecture and flow diagrams (to be created)
- [Architecture Deep Dive](./docs/architecture/) - Detailed design documentation (future)
- [Implementation Plan](./docs/implementation/) - MVP and development roadmap (future)

---

**Status**: Design phase - feedback and ideas welcome | **License**: TBD | **Contact**: See repository owner
