# System Patterns

## Architecture Overview

```
┌─────────────────────────────────────────────────┐
│                   CLI (typer)                    │
│  cgc index | analyze | find | list | watch | mcp│
├─────────────────────────────────────────────────┤
│               MCP Server (JSON-RPC)             │
│         server.py + tool_definitions.py         │
├────────────┬────────────┬───────────────────────┤
│  Indexing  │  Analysis  │   Graph Queries       │
│  Pipeline  │  Tools     │   (Cypher)            │
├────────────┴────────────┴───────────────────────┤
│             Graph Database Layer                 │
│   KùzuDB (default) | FalkorDB | Neo4j           │
├─────────────────────────────────────────────────┤
│           Parsing Layer                          │
│   tree-sitter (14 langs) + SCIP (fallback)      │
└─────────────────────────────────────────────────┘
```

## Source Code Layout

```
src/codegraphcontext/
├── server.py              ← MCP server (JSON-RPC main loop)
├── tool_definitions.py    ← MCP tool schemas
├── prompts.py             ← LLM system prompt
├── cli/
│   ├── main.py            ← typer CLI app entry point
│   ├── cli_helpers.py     ← CLI utility functions
│   ├── config_manager.py  ← configuration management
│   ├── setup_wizard.py    ← interactive setup
│   ├── visualizer.py      ← HTML graph visualization
│   └── registry_commands.py ← bundle registry commands
├── core/
│   ├── database.py        ← abstract DB interface
│   ├── database_kuzu.py   ← KùzuDB implementation
│   ├── database_falkordb.py      ← FalkorDB Lite implementation
│   ├── database_falkordb_remote.py ← FalkorDB remote implementation
│   ├── bundle_registry.py ← pre-indexed bundle management
│   ├── cgc_bundle.py      ← bundle file format
│   ├── jobs.py            ← background job tracking
│   └── watcher.py         ← file system watcher
├── tools/
│   ├── graph_builder.py   ← builds graph from parsed code
│   ├── code_finder.py     ← code search utilities
│   ├── system.py          ← system-level operations
│   ├── scip_indexer.py    ← SCIP protobuf index parser
│   ├── scip_pb2.py        ← SCIP protobuf definitions
│   ├── package_resolver.py ← local package resolution
│   ├── handlers/          ← MCP tool handler modules
│   ├── languages/         ← per-language tree-sitter parsers
│   └── query_tool_languages/ ← language-specific query helpers
└── utils/                 ← shared utilities
```

## Key Design Patterns

### Parsing Pipeline
1. **tree-sitter** parses source files into AST nodes
2. Per-language parser (e.g., `languages/typescript.py`) extracts symbols (functions, classes, methods) and relationships (calls, imports, inheritance)
3. **SCIP fallback** (our fork improvement): if tree-sitter misses symbols, SCIP index provides additional cross-references with precise scope/kind/definition info
4. `graph_builder.py` creates graph nodes and edges from parsed data

### Database Abstraction
- Abstract `DatabaseManager` interface in `core/database.py`
- Concrete implementations: KùzuDB (embedded, default), FalkorDB Lite (in-process), FalkorDB Remote, Neo4j
- All use Cypher query language for graph operations
- Selected via environment variable or CLI flag

### MCP Server Protocol
- `server.py` implements JSON-RPC over stdin/stdout
- Tool definitions in `tool_definitions.py` (separate from handlers)
- Handlers organized by domain in `tools/handlers/` (analysis, indexing, management, query, watcher)
- Tools exposed: add_code_to_graph, find_code, analyze_code_relationships, find_dead_code, calculate_cyclomatic_complexity, execute_cypher_query, etc.

### Graph Schema
- **Nodes**: Function, Class, Method, Module (with properties: name, file, line, scope, kind, parameters, return_type)
- **Edges**: CALLS, IMPORTS, INHERITS, CONTAINS, INSTANTIATES, USES, DECORATES
- Dead code query checks all outgoing edge types (our fix — upstream only checked CALLS)

### File Watching
- `watcher.py` uses `watchdog` library to monitor file system changes
- Automatically re-indexes changed files and updates the graph
- Supports `.cgcignore` for excluding paths (gitignore syntax)

## Error Handling Patterns
- Graceful fallback: if SCIP index unavailable, fall back to tree-sitter only
- Per-file error isolation: parsing failure in one file doesn't block others
- Job manager tracks indexing progress for long-running operations
