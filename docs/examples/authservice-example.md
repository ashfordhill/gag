# Example: Debugging AuthService Latency

This example demonstrates how the Code Knowledge Graph system transforms structured dependency information into natural language context that LLMs can reason about effectively.

## Scenario

An agent receives the task: **"AuthService is experiencing 500ms p95 latency, investigate"**

Without a knowledge graph, the agent would need to:
- Manually search for AuthService dependencies
- Check each dependency for recent changes
- Look for known issues in documentation
- Piece together relationships from multiple files

With the knowledge graph, relevant context is assembled automatically.

---

## Step 1: Graph Structure

The knowledge graph contains these nodes and edges for AuthService:

### Nodes

```
AuthService (v2.3.1)
├─ type: Service
├─ metadata:
│  ├─ version: "2.3.1"
│  ├─ repo: "services/auth"
│  └─ file_path: "services/auth/main.go"
├─ description: "Handles user authentication via JWT tokens"
├─ notes:
│  ├─ "Uses Redis for session storage"
│  ├─ "Rate limiting implemented at nginx level"
└─ gotchas:
   └─ "Token refresh can race under load"

KafkaLib (v2.3)
├─ type: Library
├─ metadata:
│  └─ version: "2.3"
├─ description: "Event streaming library"
└─ notes:
   └─ "Used for publishing user action events"

RedisClient (v4.1)
├─ type: Library
├─ metadata:
│  └─ version: "4.1"
├─ description: "Redis client library"
└─ notes:
   └─ "Session storage and caching"

MetricsLib (v1.5)
├─ type: Library
├─ metadata:
│  └─ version: "1.5"
└─ description: "Telemetry and monitoring"

ProtoBuf (v3.21)
├─ type: Library
└─ metadata:
   └─ version: "3.21"

ProtoBuf (v3.19)
├─ type: Library
└─ metadata:
   └─ version: "3.19"
```

### Edges

```
AuthService --depends_on--> KafkaLib v2.3
├─ annotation: "Event streaming for user actions"
├─ last_modified: "3 weeks ago"
└─ stability: "stable"

AuthService --depends_on--> RedisClient v4.1
├─ annotation: "Session storage"
├─ last_modified: "2 months ago"
└─ stability: "stable"

AuthService --depends_on--> MetricsLib v1.5
├─ annotation: "Telemetry and monitoring"
└─ stability: "stable"

KafkaLib v2.3 --depends_on--> ProtoBuf v3.21

MetricsLib v1.5 --depends_on--> ProtoBuf v3.19

AuthService --conflicts_with--> MetricsLib v1.5
└─ annotation: "Protobuf version mismatch (3.21 vs 3.19)"
```

### Recent Investigations

```
Investigation (2 weeks ago)
├─ title: "Debugged Kafka consumer lag issue"
├─ related_nodes: [AuthService, KafkaLib]
├─ root_cause: "GC pauses during traffic spike"
├─ solution: "Increased heap size, added alerting"
└─ result: "Lag reduced from 5min to <30sec"

Investigation (1 month ago)
├─ title: "Upgraded Redis client"
├─ related_nodes: [AuthService, RedisClient]
└─ result: "30% latency improvement"
```

### Constraints

```
Constraint
├─ category: "version"
├─ description: "KafkaLib must stay at v2.3.x (v2.4+ breaks retry logic)"
├─ affects: [AuthService, KafkaLib]
└─ severity: "warning"

Constraint
├─ category: "resource"
├─ description: "Requires minimum 2GB heap in production"
├─ affects: [AuthService]
└─ severity: "warning"
```

---

## Step 2: Graph-to-Text Rendering

The system executes a graph query targeting AuthService with depth=2, then renders the subgraph as structured natural language:

```
AuthService (v2.3.1)
Handles user authentication via JWT tokens
- Uses Redis for session storage
- Rate limiting implemented at nginx level
- Known issue: token refresh can race under load

Known Constraints:
⚠ Version pinned: KafkaLib must stay at v2.3.x (v2.4+ breaks retry logic)
⚠ Resource: Requires minimum 2GB heap in production
ℹ Deployment: Must deploy during low-traffic window (cache warmup needed)

Dependencies:
- KafkaLib v2.3: Event streaming for user actions
  ⚠ Conflicts with MetricsLib v1.5 (protobuf version mismatch)
  Last updated 3 weeks ago, stable
  
- RedisClient v4.1: Session storage
  Last updated 2 months ago, stable
  
- MetricsLib v1.5: Telemetry and monitoring
  Uses ProtoBuf v3.19

Used By (3 services):
- UserService: For login/logout flows
- PaymentService: For transaction authorization
- AdminDashboard: For user management

Recent Activity:
- 2 weeks ago: Debugged Kafka consumer lag issue
  Root cause: GC pauses during traffic spike
  Solution: Increased heap size, added alerting
  Result: Lag reduced from 5min to <30sec

- 1 month ago: Upgraded Redis client
  Result: 30% latency improvement

Transitive Conflicts:
- KafkaLib v2.3 requires ProtoBuf v3.21
- MetricsLib v1.5 requires ProtoBuf v3.19
- Cannot satisfy both dependencies simultaneously
```

---

## Step 3: LLM Reasoning

The LLM receives this rendered text (not the raw graph structure) and can immediately reason about the problem:

**LLM Analysis**:
> "Looking at the AuthService context, I notice several relevant factors for the latency issue:
> 
> 1. **Recent heap size increase** (2 weeks ago) suggests memory pressure has been an issue
> 2. **Redis client upgrade** (1 month ago) improved latency by 30%, but current latency is still high
> 3. **Protobuf version conflict** between KafkaLib and MetricsLib could cause runtime issues
> 4. **Known gotcha**: Token refresh can race under load
> 
> Most likely causes:
> - Memory pressure from increased traffic (past GC pauses suggest this pattern)
> - Redis connection pool exhaustion (the recent upgrade helped but may not be sufficient)
> - Token refresh race condition under current load
> 
> Recommended investigation steps:
> 1. Check current heap usage and GC metrics
> 2. Monitor Redis connection pool utilization
> 3. Look for token refresh errors in logs
> 4. Verify protobuf conflict isn't causing serialization overhead"

---

## Why This Works

### Without Graph-to-Text Rendering

If the LLM received raw graph data:
```json
{
  "nodes": [
    {"id": "auth-1", "type": "Service", "name": "AuthService"},
    {"id": "kafka-1", "type": "Library", "name": "KafkaLib"}
  ],
  "edges": [
    {"source": "auth-1", "target": "kafka-1", "type": "depends_on"}
  ]
}
```

The LLM would struggle to:
- Understand the significance of relationships
- Connect historical context to current state
- Identify relevant constraints and gotchas
- Reason about transitive dependencies

### With Graph-to-Text Rendering

The rendered text provides:
- **Natural language structure** that LLMs excel at processing
- **Prioritized information** (constraints and recent activity highlighted)
- **Contextual annotations** (why each dependency exists)
- **Temporal patterns** (recent investigations and changes)
- **Warning indicators** (⚠ symbols for conflicts and gotchas)

The LLM can immediately connect:
- Past investigations → Current symptoms
- Dependency relationships → Potential conflicts
- Resource constraints → Performance issues
- Known gotchas → Debugging strategies

---

## Key Takeaways

1. **Verified Structure**: Graph edges represent proven code relationships (imports, dependencies, API calls), not similarity scores
2. **Semantic Enrichment**: LLM adds meaning (annotations, descriptions, gotchas) that static analysis can't extract
3. **Query-Driven Assembly**: Context is compiled fresh for each task, not stored as static markdown
4. **Natural Language Output**: Renders complex graph traversals as readable text that LLMs can reason about
5. **Learning Over Time**: Past investigations become part of the context for future debugging

This example shows the core innovation: combining the precision of static analysis with the semantic understanding of LLMs, while using graph-to-text rendering to bridge the gap between structured data and LLM reasoning.
