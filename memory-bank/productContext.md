# Product Context

## Why This Project Exists
AI coding assistants (GitHub Copilot, Claude, Cursor) work with flat file context and lack structural code understanding. They cannot natively answer questions like "who calls this function?", "what classes inherit from X?", or "is this code dead?". CodeGraphContext bridges this gap by building a queryable code knowledge graph and exposing it via MCP protocol.

## Target Users
1. **AI assistants** — via MCP server, gaining structured code understanding for better answers
2. **Developers** — via CLI toolkit, for code analysis, dead code detection, complexity analysis, and visualization

## How It Should Work
1. **Index**: Point CGC at a codebase (`cgc index .`) — it parses all supported language files using tree-sitter (and optionally SCIP), extracting functions, classes, methods, calls, imports, inheritance
2. **Store**: Build a graph database with nodes (symbols) and edges (relationships) in KùzuDB, FalkorDB, or Neo4j
3. **Query**: AI assistants or developers query the graph for callers, callees, class hierarchies, dead code, complexity metrics, call chains
4. **Watch**: Optionally watch directories for changes and update the graph in real-time

## Dual Mode Operation
- **CLI Mode**: `cgc` command with subcommands (index, analyze, find, list, watch, etc.)
- **MCP Server Mode**: `cgc mcp start` exposes 16+ tools for AI agents via JSON-RPC

## UX Principles
- Zero-config database (KùzuDB embedded) — works out of the box
- Interactive setup wizard for MCP configuration
- Pre-indexed bundles (`.cgc` files) for popular repositories
- Interactive HTML visualizations for code graphs
- Support all major AI IDEs (VS Code, Cursor, Windsurf, Claude, Kiro)
