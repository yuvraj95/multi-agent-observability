# Information Architecture — Multi-Agent Observability Module

## Product Structure

The observability module is organized around three levels of granularity — from the broadest (all runs) to the most specific (a single node within a run). Users move fluidly between these levels during a debugging session.

```
Observability Module
│
├── Run List (All Runs)
│   ├── Filters: status, agent, date range, error type, duration
│   ├── Search: run ID, error message, tool name
│   └── Run Row: ID, agent, status, start time, duration, tokens
│
├── Run Detail
│   ├── Run Summary Panel
│   │   ├── Status (success / failure / running)
│   │   ├── Total duration
│   │   ├── Total tokens consumed
│   │   ├── Total tool calls
│   │   └── Failure reason (if failed)
│   │
│   ├── Execution Graph
│   │   ├── Nodes: LLM calls, tool calls, decision branches, sub-agents
│   │   ├── Edges: execution flow between nodes
│   │   ├── Node color coding: green / red / amber
│   │   └── Timeline view (linear execution order + relative duration)
│   │
│   └── Node Detail Panel (on click)
│       ├── Node type (LLM / Tool / Decision / Sub-agent)
│       ├── Input payload
│       ├── Output payload
│       ├── Execution time
│       ├── Token usage (prompt + completion) — LLM nodes only
│       ├── Tool name + response — tool call nodes only
│       └── Error message + stack trace — failed nodes only
│
└── Agent Analytics (Post-MVP)
    ├── Failure rate over time
    ├── Average latency trend
    ├── Token consumption trend
    └── Most frequently failing nodes
```

---

## Node Types

| Node Type | What It Represents | Key Metrics Shown |
|---|---|---|
| LLM Call | A call to a language model | Prompt tokens, completion tokens, latency, model name |
| Tool Call | An external tool or API invocation | Tool name, input, output, latency, success/failure |
| Decision Branch | A conditional logic step | Condition evaluated, branch taken |
| Sub-agent | A nested agent invocation | Sub-run ID, status, duration, link to sub-run detail |
| Start / End | Run entry and exit points | Timestamp, overall status |

---

## Status System

| Status | Color | Meaning |
|---|---|---|
| Success | Green | Node or run completed without errors |
| Failed | Red | Node or run encountered an error — failure reason surfaced |
| Warning | Amber | Node completed but with degraded performance (high latency, token spike) |
| Running | Blue | Node or run currently in progress |
| Skipped | Grey | Node was not executed (conditional branch not taken) |

---

## Navigation Patterns

- **Run List → Run Detail:** Click any run row
- **Run Detail → Node Detail:** Click any node in the execution graph
- **Node Detail → Related Run:** Click sub-run ID to open nested run (for sub-agent nodes)
- **Run Detail → Run List:** Breadcrumb navigation
- **Cross-run navigation:** Filter and search always available from Run List
