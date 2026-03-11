# Project Brief: CodeGraphContext

## Overview
CodeGraphContext (CGC) is an MCP server and CLI toolkit that indexes local code repositories into a queryable graph database, providing structural code understanding to AI assistants. Fork of [CodeGraphContext/CodeGraphContext](https://github.com/CodeGraphContext/CodeGraphContext) maintained at [andriispivakelectrodosg/CodeGraphContext](https://github.com/andriispivakelectrodosg/CodeGraphContext).

## Problem Statement
AI coding assistants lack structural understanding of codebases — they see files as flat text without knowing call graphs, class hierarchies, or dead code. Code graph databases solve this but require complex setup and don't integrate natively with AI workflows.

## Goals
- Index code repositories into a graph database with functions, classes, methods, calls, imports
- Provide MCP server interface for AI assistants to query code structure
- Provide CLI toolkit for direct developer use (analysis, visualization, dead code detection)
- Support 14+ programming languages via tree-sitter parsing
- Support SCIP indexing as an alternative/complement to tree-sitter for precise cross-references
- Provide multiple database backends (KùzuDB, FalkorDB, Neo4j)

## Non-Goals
- Full IDE integration (that's the VS Code extension's job)
- Runtime code analysis or profiling
- Code generation or modification

## Fork Improvements (Our Contributions)
1. SCIP fallback indexing when tree-sitter parsing is incomplete
2. Fixed dead code Cypher query to check multiple edge types (CALLS, INSTANTIATES, IMPORTS, INHERITS, USES)
3. Fixed anonymous callback parent context walking (symbol resolution)
4. SCIP scope and kind fields mapped to graph nodes
5. SCIP environment variable support (SCIP_INDEX_PATH)
6. .js → .ts file resolution for TypeScript projects
7. --scip CLI flag for explicit SCIP indexing
8. protobuf dependency added for SCIP support

## Success Criteria
- Accurate code graph for TypeScript, Python, JavaScript, and other supported languages
- Dead code detection with minimal false positives
- Responsive MCP server for AI assistant queries
- SCIP integration providing better precision than tree-sitter alone for supported languages
