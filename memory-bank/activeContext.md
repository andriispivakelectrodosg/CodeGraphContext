# Active Context

## Current Focus
Initial fork setup — 10 improvements implemented and merged to main, memory-bank initialized

## Recent Changes
- Forked CodeGraphContext/CodeGraphContext to andriispivakelectrodosg/CodeGraphContext
- Implemented 10 fixes across 7 files on `feat/scip-improvements` branch
- Squash-merged all fixes to `main` (commit `f92655e`)
- Cloned fork to /home/narayanaya/CodeGraphContext for development in separate VS Code profile
- Created memory-bank structure (this session)

## Changes Summary (10 Fixes)
1. **pyproject.toml**: Added `protobuf>=4.21.0` to `[scip]` optional dependencies
2. **system.py**: Fixed dead code Cypher query — checks CALLS, INSTANTIATES, IMPORTS, INHERITS, USES edges (was only CALLS)
3. **system.py**: Fixed anonymous callback parent context walking — walks up parse tree to find named function/method/class parent
4. **scip_indexer.py**: SCIP fallback indexing — parses SCIP protobuf index for symbols tree-sitter misses
5. **scip_indexer.py**: SCIP scope field mapped from symbol descriptor
6. **scip_indexer.py**: SCIP kind field mapped from SymbolInformation
7. **scip_indexer.py**: SCIP_INDEX_PATH environment variable support
8. **typescript.py**: .js → .ts file resolution for import paths
9. **javascript.py**: Same .js → .ts resolution for JavaScript parser
10. **graph_builder.py** + **main.py**: `--scip` CLI flag for explicit SCIP indexing

## Current Decisions
- Development managed in separate VS Code profile (not as submodule of vscode-memory-bank)
- SCIP support is opt-in (extra dependency, CLI flag)
- KùzuDB as default database backend (upstream choice, maintained)

## Next Steps
1. Set up Python venv and install CGC with `[scip]` extras
2. Test SCIP indexing against a real TypeScript project
3. Sync with upstream for any new features/fixes
4. Explore improving language parsers (tree-sitter query accuracy)
5. Consider contributing fixes back to upstream
