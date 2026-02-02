# Excalidraw Diagram Specifications

This document provides detailed specifications for creating the GAG (Graph-Augmented-Generation) system diagrams in Excalidraw. Each diagram is designed to illustrate a key concept visually.

**User Action Required**: Create the `.excalidraw` files manually based on these specifications, then export as `.png` for README embedding.

---

## Diagram 1: System Overview (HIGH PRIORITY)

### Purpose
Show the complete data flow from code to LLM reasoning, emphasizing the two-phase construction and the graph-to-text rendering step.

### File Names
- `system-overview.excalidraw`
- `system-overview.png`

### Layout
**Horizontal flow diagram (left to right)** with three distinct phases marked by color.

### Dimensions
- Recommended canvas size: 1400px wide × 600px tall
- Give plenty of whitespace between phases

### Color Scheme

**Phase 1: Static Analysis (Blue family)**
- Background: Light blue (#E3F2FD or similar)
- Borders: Medium blue (#2196F3)
- Text: Dark blue (#0D47A1)

**Phase 2: LLM Enrichment (Orange family)**
- Background: Light orange (#FFE0B2)
- Borders: Orange (#FF9800)
- Text: Dark orange (#E65100)

**Phase 3: Query Time (Green family)**
- Background: Light green (#E8F5E9)
- Borders: Green (#4CAF50)
- Text: Dark green (#1B5E20)

### Components (Left to Right)

#### 1. Input (Far Left)
**Element**: Rectangle with rounded corners
- **Label**: "Code Repositories"
- **Icon/Visual**: Add small code file icons or folder symbols inside
- **Examples**: Show labels like "services/auth", "services/api", "lib/metrics"
- **Style**: Neutral gray (#EEEEEE background, #757575 border)

#### 2. Phase 1: Static Analysis (Blue Section)
**Header**: Text label "Phase 1: Structural Analysis (Deterministic)"

**Three parallel boxes** (arranged vertically or staggered):

**Box 1**: "AST Parsing"
- Icon: Tree-like structure or `</>` symbol
- Size: Medium rectangle

**Box 2**: "Dependency Extraction"  
- Icon: Package symbol or chain links
- Size: Medium rectangle

**Box 3**: "Git History Analysis"
- Icon: Git branch or clock symbol
- Size: Medium rectangle

**Arrow from all three** pointing to:

**Box 4**: "Graph Database"
- Larger cylinder or database icon shape
- Show mini example nodes inside:
  - Small circles labeled: "AuthService", "KafkaLib", "Redis"
  - Small arrows between them labeled: "depends_on", "calls_api"
- Background: Slightly darker blue than surrounding area

#### 3. Phase 2: LLM Enrichment (Orange Section)
**Header**: Text label "Phase 2: Semantic Enrichment (LLM)"

**Arrow from Graph Database** pointing to:

**Box 1**: "LLM Analysis"
- Icon: Brain or sparkle/AI symbol
- Medium rectangle
- Example annotations inside box:
  - "Add descriptions"
  - "Annotate edges"
  - "Detect conflicts"

**Bi-directional arrow** (or curved return arrow) back to Graph Database with label:
- "Enriches nodes & edges"

**Show the enriched graph** with added annotations:
- Same node circles but with small text bubbles showing:
  - "Handles auth"
  - "Event streaming"
  - "⚠ Version conflict"

#### 4. Phase 3: Query Time (Green Section)
**Header**: Text label "Query Time: Context Assembly"

**Box 1**: "Agent Task"
- Small rectangle at top
- Label: "Debug AuthService latency"
- Style: Dotted border to indicate input

**Arrow down** to:

**Box 2**: "Graph Query"
- Medium rectangle
- Label: "Traverse graph (depth=2)"

**Arrow down** to:

**Box 3**: "Render to Text"
- Medium rectangle
- Label: "Compile subgraph → natural language"

**Arrow down** to:

**Box 4**: "Structured Context"
- Large text box showing sample output:
```
AuthService (v2.3.1)
Handles authentication...

Dependencies:
- KafkaLib v2.3: Event streaming
  ⚠ Conflicts with MetricsLib
```
- Style: Code/monospace font, light background

#### 5. Output (Far Right)
**Box**: "LLM Receives Context"
- Medium rectangle
- Icon: AI/brain symbol
- Style: Neutral or light purple

**Arrow to**:

**Box**: "Actionable Insights"
- Small rectangle
- Label: "Debugging strategy, root cause analysis"

### Arrows and Flow
- Use **solid arrows** for data flow
- Use **dashed arrows** for enrichment/annotation steps
- Label important arrows:
  - "Extracts structure"
  - "Adds semantics"
  - "Queries relationships"
  - "Natural language"

### Key Visual Elements
- **Add a legend** in bottom-right corner:
  - Blue box: "Deterministic (static analysis)"
  - Orange box: "LLM-powered (semantic)"
  - Green box: "Runtime (query-driven)"

- **Emphasize the innovation**: Draw a thick border or highlight around the "Render to Text" box with annotation: "KEY INNOVATION: LLMs receive text, not raw graph"

### Visual Style
- Use **rounded corners** for all rectangles (friendlier appearance)
- Use **cylinder shape** for database
- Use **cloud or brain icon** for LLM components
- Keep **text readable** (12-14pt minimum)
- Add **subtle shadows** to create depth

---

## Diagram 2: Graph-to-Text Example (HIGH PRIORITY)

### Purpose
Illustrate the key innovation: how a dependency graph with conflict gets rendered as readable text that LLMs can reason about.

### File Names
- `graph-to-text-example.excalidraw`
- `graph-to-text-example.png`

### Layout
**Split layout**: Graph structure on left (40%), rendered text on right (50%), arrow in middle (10%)

### Dimensions
- Recommended canvas size: 1200px wide × 800px tall

### Color Scheme
- **Service nodes**: Light blue rectangles (#BBDEFB)
- **Library nodes**: Light orange rounded rectangles (#FFCC80)
- **Conflict edge**: Red dashed line (#F44336)
- **Normal edges**: Gray solid arrows (#757575)
- **Text output box**: Light gray background (#F5F5F5)

---

### Left Side: The Graph Structure

**Header**: Text label "Graph Structure" (bold, 16pt)

#### Nodes (use appropriate shapes)

**Service Node (Rectangle with rounded corners)**:
```
┌─────────────────────┐
│  AuthService        │
│  v2.3.1            │
└─────────────────────┘
```
- Position: Top center-left
- Color: Light blue (#BBDEFB)
- Border: Medium blue
- Size: ~150px wide × 60px tall

**Library Node 1 (Rounded rectangle/pill shape)**:
```
┌────────────────┐
│ KafkaLib v2.3  │
└────────────────┘
```
- Position: Below-left of AuthService
- Color: Light orange (#FFCC80)
- Border: Orange
- Size: ~120px wide × 50px tall

**Library Node 2**:
```
┌────────────────┐
│ RedisClient    │
│ v4.1           │
└────────────────┘
```
- Position: Below-right of AuthService (to spread out the graph)
- Color: Light orange
- Size: ~120px wide × 50px tall

**Library Node 3**:
```
┌────────────────┐
│ MetricsLib     │
│ v1.5           │
└────────────────┘
```
- Position: Below AuthService (middle)
- Color: Light orange
- Size: ~120px wide × 50px tall

**Library Node 4**:
```
┌────────────────┐
│ ProtoBuf       │
│ v3.21          │
└────────────────┘
```
- Position: Below KafkaLib
- Color: Light yellow (#FFF9C4) - to distinguish version
- Size: ~100px wide × 50px tall

**Library Node 5**:
```
┌────────────────┐
│ ProtoBuf       │
│ v3.19          │
└────────────────┘
```
- Position: Below MetricsLib
- Color: Light yellow (#FFF9C4)
- Size: ~100px wide × 50px tall
- **Note**: Add a small red badge or exclamation mark to indicate this is a duplicate version

#### Edges (labeled arrows)

**Edge 1**: AuthService → KafkaLib
- Arrow: Solid gray
- Label: "depends_on" (small text above arrow)
- Annotation below: "event streaming" (in quotes, italic, small)

**Edge 2**: AuthService → RedisClient
- Arrow: Solid gray
- Label: "depends_on"
- Annotation: "session storage"

**Edge 3**: AuthService → MetricsLib
- Arrow: Solid gray
- Label: "depends_on"
- Annotation: "telemetry"

**Edge 4**: KafkaLib → ProtoBuf v3.21
- Arrow: Solid gray
- Label: "depends_on"
- No annotation (keep it simple)

**Edge 5**: MetricsLib → ProtoBuf v3.19
- Arrow: Solid gray
- Label: "depends_on"

**Edge 6**: AuthService ← → MetricsLib
- Arrow: **Red dashed line** (bidirectional or with conflict symbol)
- Label: "conflicts_with" (in red text)
- Annotation: "protobuf version mismatch" (in red, bold)
- **Visual emphasis**: Make this arrow thicker or add warning icon (⚠)

#### Visual Enhancements
- Add a **subtle background box** around all nodes (light gray border) to contain the "graph"
- Add small **node type icons**:
  - Service: Small gear icon
  - Library: Small package icon

---

### Middle: Arrow & Process

**Large curved arrow** pointing from left graph to right text:
- Label: "Graph Query & Rendering"
- Or: "render_subgraph_to_text()"
- Style: Thick, prominent, with gradient or glow
- Color: Purple or dark gray

**Optional**: Add small step indicators along the arrow:
- "1. Query: target=AuthService, depth=2"
- "2. Traverse relationships"
- "3. Render as natural language"

---

### Right Side: Rendered Text Output

**Header**: Text label "Rendered Context (for LLM)" (bold, 16pt)

**Large text box** showing the compiled output:
- Background: Light gray (#F5F5F5) or code editor style
- Border: Thin dark gray
- Font: Monospace (Courier New, Consolas, or Monaco)
- Font size: 10-12pt
- Padding: 20px

**Text content** (copy exactly, preserving formatting):

```
AuthService (v2.3.1)
Handles user authentication via JWT tokens

Known Constraints:
⚠ Version conflict: ProtoBuf mismatch (3.21 vs 3.19)

Dependencies:
- KafkaLib v2.3: Event streaming for user actions
  ⚠ Conflicts with MetricsLib v1.5 (protobuf version mismatch)
  Last updated 3 weeks ago, stable
  
- RedisClient v4.1: Session storage
  Last updated 2 months ago, stable
  
- MetricsLib v1.5: Telemetry and monitoring
  Uses ProtoBuf v3.19
  
Transitive Conflicts:
- KafkaLib v2.3 requires ProtoBuf v3.21
- MetricsLib v1.5 requires ProtoBuf v3.19
- Cannot satisfy both dependencies simultaneously
```

**Visual highlights within the text**:
- Make warning symbols (⚠) **red** or **orange**
- Make "Known Constraints:" header **bold**
- Make "Dependencies:" header **bold**
- Optionally: highlight the conflict lines with a light red background

---

### Bottom: Key Insight

**Text box** at bottom (spanning the width):
- Background: Light yellow (#FFFDE7) - like a sticky note
- Border: Dashed line
- Text (centered or left-aligned):

> **Key Insight**: LLMs excel at reasoning about natural language but struggle with raw graph structures. The rendering algorithm transforms verified relationships into structured, readable context.

---

### Visual Style
- Keep the **graph simple** (5-6 nodes max) so it's not overwhelming
- Use **different shapes** for node types (rectangles for services, rounded for libraries)
- Make the **conflict edge visually obvious** (red, dashed, thick)
- Ensure **text is readable** even when scaled down to ~800px wide
- Use **color sparingly** to highlight important elements

---

## Diagram 3: Phase Roadmap (MEDIUM PRIORITY - OPTIONAL)

### Purpose
Show the incremental implementation path from documentation to V3.

### File Names
- `phase-roadmap.excalidraw`
- `phase-roadmap.png`

### Layout
**Horizontal timeline** or **vertical milestone list** with progress indicators

### Dimensions
- Recommended canvas size: 1000px wide × 600px tall (horizontal)
- Or: 600px wide × 1000px tall (vertical)

### Color Scheme
- **Completed**: Green (#4CAF50)
- **In Progress**: Yellow/Orange (#FFC107)
- **Planned**: Light gray (#E0E0E0)

---

### Option A: Horizontal Timeline

**Timeline bar**: Horizontal line with milestones

#### Milestone 1: Documentation Phase (Current)
- Position: Left
- **Circle marker**: Green (filled) - indicates "current"
- **Label above**: "Documentation Phase"
- **Box below** with details:
  - ✅ Core concepts README
  - ✅ Example diagrams
  - ⏳ Design specifications
- **Status badge**: "In Progress" (orange)
- **Visual**: Add a "You are here" marker or arrow

#### Milestone 2: MVP
- Position: Center-left
- **Circle marker**: Yellow (outlined)
- **Label above**: "MVP"
- **Box below** with details:
  - Single repo support
  - Basic graph construction
  - Simple text rendering
  - Test with real debugging
- **Status badge**: "Planned" (gray)

#### Milestone 3: V2
- Position: Center-right
- **Circle marker**: Gray (outlined)
- **Label above**: "V2"
- **Box below** with details:
  - Multi-repo support
  - Investigation recording
  - Incremental updates
  - Conflict detection
- **Status badge**: "Planned" (gray)

#### Milestone 4: V3
- Position: Right
- **Circle marker**: Gray (outlined)
- **Label above**: "V3"
- **Box below** with details:
  - Vector similarity search
  - CI/CD integration
  - Advanced conflict detection
  - Dashboard visualization
- **Status badge**: "Future" (light gray)

**Connecting line**: Connect all circles with a horizontal line
- **Style**: Solid line between completed/current, dashed line for future

---

### Option B: Vertical Milestone Cards

**Card 1**: Documentation Phase
- **Position**: Top
- **Card style**: Rectangle with rounded corners
- **Background**: Light green gradient (indicates current phase)
- **Border**: Green, thick on left side
- **Icon**: Document/pencil icon
- **Header**: "Phase 0: Documentation" (bold)
- **Status**: "In Progress" badge (green)
- **Content**:
  ```
  Goal: Make the concept clear and digestible
  
  Deliverables:
  ✅ Core concepts README
  ✅ Concrete examples (AuthService)
  ✅ System diagrams
  ⏳ Diagram specifications
  
  Status: ~75% complete
  ```

**Card 2**: MVP
- **Position**: Below Card 1
- **Card style**: Same as Card 1
- **Background**: Light yellow gradient
- **Border**: Orange, thinner
- **Icon**: Hammer/tools icon
- **Header**: "Phase 1: MVP"
- **Status**: "Planned" badge (yellow)
- **Content**:
  ```
  Goal: Prove the concept works
  
  Deliverables:
  - Single repository support
  - Basic graph construction (Phase 1 only)
  - Simple text rendering
  - Test with real debugging tasks
  
  Timeline: TBD (after documentation complete)
  Complexity: 2-3 weeks
  ```

**Card 3**: V2
- **Position**: Below Card 2
- **Background**: Light gray
- **Border**: Gray
- **Icon**: Puzzle pieces or network icon
- **Header**: "Phase 2: V2"
- **Status**: "Future" badge (gray)
- **Content**:
  ```
  Goal: Production-ready features
  
  Deliverables:
  - Multi-repo support
  - Investigation recording system
  - Incremental graph updates
  - Conflict detection
  - Advanced rendering (priority-based)
  
  Timeline: 4-6 weeks after MVP
  ```

**Card 4**: V3
- **Position**: Bottom
- **Background**: Very light gray
- **Border**: Light gray
- **Icon**: Rocket or star icon
- **Header**: "Phase 3: V3"
- **Status**: "Vision" badge (very light gray)
- **Content**:
  ```
  Goal: Advanced features and polish
  
  Deliverables:
  - Vector similarity for related issues
  - Community detection for service clusters
  - CI/CD integration
  - Dashboard for graph visualization
  
  Timeline: Future (6+ months out)
  ```

**Connecting elements**:
- **Arrow** from bottom of Card 1 to top of Card 2
- **Arrow** from Card 2 to Card 3
- **Arrow** from Card 3 to Card 4
- **Style**: Solid arrows between current→next, dashed for future

---

### Visual Style (Roadmap)
- Use **progress indicators** (checkboxes, progress bars)
- Use **color to indicate status** (green=done/current, yellow=next, gray=future)
- Keep **text minimal** - just key deliverables
- Add **icons** to make it visually interesting
- Consider a **"You are here"** indicator on the current phase

---

## General Excalidraw Tips

### Text Formatting
- **Headers**: 16-18pt, bold
- **Body text**: 12-14pt, regular
- **Annotations**: 10-12pt, italic or lighter color
- Use **consistent fonts** throughout (Excalidraw's default "Hand-drawn" or "Virgil" font works well)

### Color Consistency
- Stick to the **color schemes** defined above
- Use **lighter backgrounds** with **darker borders** for readability
- Use **red/orange** for warnings and conflicts
- Use **green** for completed/stable items
- Use **blue** for deterministic/structural elements
- Use **orange** for LLM/semantic elements

### Shapes and Icons
- **Rectangles**: Services, processes, boxes
- **Rounded rectangles**: Libraries, modules
- **Circles/ellipses**: Nodes, milestones
- **Cylinders**: Databases
- **Diamonds**: Decision points (if needed)
- **Clouds**: LLM/AI components
- Use **Excalidraw library** for icons (search for database, code, AI, etc.)

### Arrows and Connectors
- **Solid arrows**: Data flow, dependencies
- **Dashed arrows**: Enrichment, optional paths
- **Thick arrows**: Important flows
- **Colored arrows**: Conflicts (red), success (green)
- Add **labels** to arrows to explain relationships

### Layout and Spacing
- Use **consistent spacing** between elements (~50-80px)
- **Align elements** (use Excalidraw's alignment tools)
- Leave **adequate whitespace** - don't cram everything together
- **Group related elements** visually (with subtle background boxes if needed)

### Exporting
- Export as **PNG with transparent background** (if diagram will be on white background in docs)
- Or export with **white background** for consistency
- Recommended export width: **1200-1400px** for high-resolution rendering in docs
- Save both `.excalidraw` source file and `.png` export

---

## File Checklist

After creating diagrams, ensure you have:

- [ ] `docs/diagrams/system-overview.excalidraw`
- [ ] `docs/diagrams/system-overview.png`
- [ ] `docs/diagrams/graph-to-text-example.excalidraw`
- [ ] `docs/diagrams/graph-to-text-example.png`
- [ ] `docs/diagrams/phase-roadmap.excalidraw` (optional)
- [ ] `docs/diagrams/phase-roadmap.png` (optional)

## Embedding in README

Once created, update README.md to embed diagrams:

```markdown
## How It Works

![System Overview](./docs/diagrams/system-overview.png)

*The complete data flow from code repositories to LLM insights, showing two-phase construction and graph-to-text rendering.*

### Example: Graph-to-Text Rendering

![Graph-to-Text Example](./docs/diagrams/graph-to-text-example.png)

*How dependency graphs with conflicts are rendered as structured natural language for LLM reasoning.*
```

---

## Verification Questions

After creating diagrams, ask yourself:

1. **Clarity**: Can someone unfamiliar with the project understand the diagram in 30 seconds?
2. **Accuracy**: Does the diagram correctly represent the architecture described in the README?
3. **Visual Hierarchy**: Are the most important elements visually prominent?
4. **Readability**: Is all text readable when the diagram is scaled to ~800px wide?
5. **Consistency**: Do colors and shapes have consistent meaning across diagrams?

If you answer "no" to any of these, iterate on the diagram design.

---

## Next Steps

1. **Create Diagram 1** (System Overview) - highest priority
2. **Create Diagram 2** (Graph-to-Text Example) - highest priority
3. **Review and iterate** based on clarity
4. **Optional**: Create Diagram 3 (Phase Roadmap)
5. **Embed diagrams** in README.md
6. **Commit** both `.excalidraw` source files and `.png` exports

These specifications provide everything needed to create clear, informative diagrams that visually communicate the GAG system architecture and key innovations.
