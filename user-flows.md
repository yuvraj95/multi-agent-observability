# User Flows — Multi-Agent Observability Module

**Live Prototype:** [agent-tracker-observability.replit.app](https://agent-tracker-observability.replit.app/)

---

## Flow 1 — Debug a Failed Run

**Actor:** AI Engineer  
**Trigger:** Agent run has failed in production. Engineer needs to find root cause.

1. Engineer opens the Observability module and lands on Run List
2. Filters by status = "Failed"
3. Identifies the relevant run by timestamp or run ID — clicks to open Run Detail
4. Run Summary Panel shows: status = Failed, failure reason surfaced at top (e.g., "Tool call failed at step 4: API timeout")
5. Engineer looks at Execution Graph — failed node is highlighted in red
6. Clicks the failed node — Node Detail Panel opens
7. Sees: input payload sent to the node, error message returned, stack trace
8. Identifies root cause (e.g., malformed input from previous node, downstream API failure)
9. Navigates back to the preceding node to inspect its output — confirms the upstream issue
10. Engineer has full root cause in under 10 minutes without touching raw logs

---

## Flow 2 — Inspect a Successful Run

**Actor:** Prompt Engineer  
**Trigger:** Run succeeded but token consumption was unexpectedly high. Engineer wants to understand where.

1. Opens Run List — filters by agent name and date range
2. Sorts by total tokens descending — identifies high-token run
3. Opens Run Detail — Run Summary shows total token count broken down
4. Opens Execution Graph — looks for nodes with high token usage (amber warning indicators)
5. Clicks into LLM call nodes one by one — Node Detail shows prompt tokens and completion tokens per call
6. Identifies a specific node where completion tokens spiked (verbose model response)
7. Reviews the input prompt sent to that node — identifies an overly open-ended prompt instruction
8. Takes finding back to prompt iteration — rewrites that node's prompt to constrain output length

---

## Flow 3 — Search and Filter Across Run History

**Actor:** Product Manager  
**Trigger:** Want to understand how often a specific agent fails and what the common failure patterns are.

1. Opens Run List
2. Filters by agent name = target agent, date range = last 30 days
3. Reviews status distribution — sees failure rate at a glance from the list
4. Filters further by status = Failed
5. Scans failure reasons across failed runs — identifies a recurring pattern (same node failing repeatedly)
6. Clicks into one representative failed run to understand the pattern in detail
7. Uses this data to write a prioritized bug ticket for the engineering team

---

## Flow 4 — Audit a Specific Run

**Actor:** Enterprise Operator  
**Trigger:** Compliance request requires a full record of what an agent did during a specific run.

1. Receives run ID from the requesting team
2. Opens Run List — uses search to find run by ID
3. Opens Run Detail
4. Reviews full execution graph — every node, input, output, and timestamp visible
5. Exports run detail (post-MVP) or screenshots node-level log for compliance record
6. Confirms the agent's decisions and outputs are auditable and documented

---

## Flow 5 — Monitor Live Run (Post-MVP)

**Actor:** AI Engineer  
**Trigger:** Long-running agent is executing — engineer wants to watch progress in real time.

1. Opens Run List — active run shown with status = Running
2. Clicks into run — Execution Graph shows completed nodes (green), current node (blue/pulsing), pending nodes (grey)
3. Engineer watches execution progress step by step
4. If a node fails mid-run, it turns red immediately — engineer can see failure without waiting for run to complete
5. Clicks failed node to inspect error in real time
