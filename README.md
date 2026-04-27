# DT-Fellowship-ReflectionTree

**Candidate:** Gopal Kumar  
**Assignment:** DT Fellowship — The Daily Reflection Tree  

---

## Structure

```
/tree/
  reflection-tree.json     ← Part A: full tree data (47 nodes)
/agent/
  index.html               ← Part B: web agent (single file, no dependencies, no LLM)
/transcripts/
  both-personas.md         ← Part B: two full run transcripts with divergence table
write-up.md                ← Part A: design rationale (psychological sources, trade-offs)
README.md                  ← This file
```

---

## How to Run the Agent

Open `agent/index.html` in any modern browser. No server needed, no npm install, no API keys.

The tree is embedded directly in the HTML file as a JavaScript object (mirroring the structure in `reflection-tree.json`). In a production version, the agent would fetch the JSON file at runtime — the data structure is identical.

---

## How to Read the Tree

Open `tree/reflection-tree.json`. Each node has:

| Field | Meaning |
|---|---|
| `id` | Unique node identifier |
| `parentId` | Parent node (null for roots) |
| `type` | `start`, `question`, `decision`, `reflection`, `bridge`, `summary`, `end` |
| `text` | What the employee sees. `{NODE_ID}` gets replaced with earlier answers. |
| `options` | For questions: pipe-separated choices. For decisions: routing rules. |
| `target` | Explicit jump target (overrides parent-child lookup) |
| `signal` | State tally: `axis1:internal`, `axis2:contribution`, etc. |
| `axis` | Display label (I, II, III) for UI |
| `axisName` | Display name for UI |

### Tracing a Path

To trace a path manually:
1. Start at `START` → follows `target: "A1_OPEN"`
2. At `A1_OPEN` (question): employee picks an option → stored as `answers["A1_OPEN"]`
3. Next is `A1_D1` (decision): reads `answers["A1_OPEN"]` → routes to `A1_Q_HIGH` or `A1_Q_LOW`
4. Continue until `SUMMARY` → reads accumulated signals to determine reflection

### Decision Node Syntax

```
"answer=Clear skies — things went my way:A1_Q_HIGH"
```
Reads: "If `answers[parentId]` equals 'Clear skies — things went my way', go to `A1_Q_HIGH`."

Multiple conditions separated by `;`. Multiple matching values separated by `|`.

---

## Node Count

| Type | Count |
|---|---|
| start | 1 |
| question | 11 |
| decision | 10 |
| reflection | 12 |
| bridge | 2 |
| summary | 1 |
| end | 1 |
| **Total** | **47** |

Requirement met: 25+ nodes, 8+ question nodes, 4+ decision nodes, 4+ reflection nodes, 2+ bridge nodes, all 3 axes.

---

## Design Principles

1. **No LLM at runtime.** The agent is a static HTML file. Zero network calls during a session.
2. **Deterministic.** Same answers → same path → same reflections. Every time.
3. **Fixed options only.** No free text. All branching is lookup-based.
4. **No moralizing.** Both "victim" and "victor" paths receive warm, honest reflections — not grades.
5. **Axes flow as one conversation.** Bridge nodes explicitly name the progression.
