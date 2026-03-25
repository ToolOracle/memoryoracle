# MemoryOracle

**Persistent long-term memory for AI agents. Agents never forget.**

10 tools. SQLite FTS5 full-text search. Per-namespace isolation. GDPR-compliant forget. Cross-references with FeedOracle Trust Layer.

```
npx -y mcp-remote https://mcp.feedoracle.io/memory/mcp/
```

## The problem

Every MCP tool call is stateless. When Claude Desktop restarts, when Cursor closes, when the session ends — the agent forgets everything. No user preferences, no project history, no learned rules, no portfolio tracking.

MemoryOracle gives agents a persistent, searchable memory that survives restarts, works across clients, and gets more valuable over time.

## Tools

| Tool | What it does |
|------|-------------|
| `store_memory` | Store a fact, preference, decision, or rule. Deduplicates automatically. |
| `query_memory` | Full-text search with BM25 ranking and recency boost |
| `update_fact` | Update a memory with new info. Preserves version history (last 10 changes) |
| `forget` | Permanently delete by ID, category, age, or everything. GDPR Art. 17 |
| `summarize_session` | End-of-session capture: summary + extracted facts, decisions, action items |
| `list_memories` | Browse with filters: category, importance, recency, access frequency |
| `cross_reference` | Verify a memory against FeedOracle Trust Layer evidence |
| `memory_stats` | Dashboard: count, storage, categories, most accessed, recent |
| `export_memories` | Export all as JSON. GDPR Art. 20 data portability |
| `health_check` | Service status |

## Memory categories

| Category | When to use |
|----------|------------|
| `fact` | Verified information: "USDC is MiCA authorized" |
| `preference` | User/agent preferences: "Conservative allocation, no USDT" |
| `decision` | Decisions made: "Blocked USDT for EU clients" |
| `observation` | Patterns noticed: "RLUSD peg stable for 30 days" |
| `rule` | Business rules: "Never recommend assets with peg deviation > 2%" |
| `session` | Session summaries with linked facts |
| `portfolio` | Portfolio positions and allocations |
| `action_item` | Open tasks: "Check RLUSD status in April" |

## Example: Compliance agent workflow

**Session 1** — Agent does MiCA compliance checks:
```json
{"name": "store_memory", "arguments": {
  "content": "USDT blocked for EU — MiCA NOT AUTHORIZED. Preflight: STOP.",
  "category": "decision", "importance": 10,
  "tags": ["usdt", "mica", "blocked"],
  "namespace": "compliance-bot"
}}
```

**Session 2** — Next day, agent remembers:
```json
{"name": "query_memory", "arguments": {
  "query": "USDT EU compliance status",
  "namespace": "compliance-bot"
}}
```
→ Instantly recalls the decision from yesterday. No re-checking needed.

**End of session** — Auto-capture:
```json
{"name": "summarize_session", "arguments": {
  "summary": "Reviewed stablecoin portfolio. USDT blocked. RLUSD pending.",
  "key_facts": ["USDC authorized", "EURC authorized", "USDT blocked"],
  "decisions": ["Block USDT for EU"],
  "action_items": ["Check RLUSD in April 2026"],
  "namespace": "compliance-bot"
}}
```

## Why this is different

| Feature | MemoryOracle | Others |
|---------|-------------|--------|
| **Search** | FTS5 BM25 ranking + recency boost | Basic keyword |
| **Categories** | 8 structured types | Flat key-value |
| **Session capture** | Auto-extracts facts, decisions, actions | Manual only |
| **Trust Layer** | Cross-reference with signed evidence | None |
| **GDPR** | Art. 17 forget + Art. 20 export | Usually missing |
| **Namespaces** | Per-agent isolation | Shared global |
| **History** | Version tracking (last 10 changes) | Overwrite |
| **TTL** | Auto-expiry with configurable hours | None |

## x402 Pay-per-call

```
POST https://tooloracle.io/x402/memory/mcp/
```

| Tool | Units | Price |
|------|-------|-------|
| `store_memory` | 2 | $0.02 |
| `query_memory` | 5 | $0.05 |
| `summarize_session` | 8 | $0.08 |
| `cross_reference` | 5 | $0.05 |
| `list_memories` | 3 | $0.03 |
| `update_fact` | 2 | $0.02 |
| `forget` | 1 | $0.01 |

## Connect

```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://mcp.feedoracle.io/memory/mcp/"]
    }
  }
}
```

## Part of ToolOracle

MemoryOracle is part of [ToolOracle](https://tooloracle.io) — 50+ MCP servers, 530+ tools. Powered by [FeedOracle](https://feedoracle.io) compliance infrastructure.

The memory layer that makes Decision Preflight, Trust Layer, ISO 20022, and every other tool exponentially more valuable.


## Read More

📝 [We Built an AI Agent That Runs 24/7, Never Forgets, and Checks Its Own Work](https://tooloracle.io/blog/autonomous-ai-agent-memory-scheduler-mcp) — How Memory + Scheduler + Decision Preflight work together as one integrated autonomous agent stack. Real tool outputs, no theory.

## License

MIT
