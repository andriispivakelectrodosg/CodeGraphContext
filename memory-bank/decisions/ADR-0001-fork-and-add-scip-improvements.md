# ADR-0001: Fork CodeGraphContext and Add SCIP Improvements

## Status
Accepted

## Context
CodeGraphContext (upstream) uses tree-sitter for code parsing, which has limitations:
- Dead code Cypher query only checks `CALLS` edges, producing false positives for classes used via `INSTANTIATES`, `IMPORTS`, `INHERITS`, or `USES`
- Anonymous callbacks lack parent context, causing symbol resolution failures
- No SCIP integration for precise cross-references in languages with SCIP indexers (TypeScript, Go, Java, etc.)
- `.js` → `.ts` file resolution missing for TypeScript projects

## Decision
Fork CodeGraphContext to `andriispivakelectrodosg/CodeGraphContext` and implement 10 improvements:
1. Fix dead code query to check all edge types
2. Fix anonymous callback parent context walking
3. Add SCIP fallback indexing (scope, kind, env variable)
4. Add .js → .ts file resolution
5. Add `--scip` CLI flag
6. Add protobuf dependency for SCIP

## Consequences
- More accurate dead code detection (fewer false positives)
- Better symbol resolution in complex code patterns
- SCIP provides precise cross-references where tree-sitter is approximate
- Must maintain fork and periodically sync with upstream
- SCIP is opt-in to avoid forcing extra dependencies on all users
