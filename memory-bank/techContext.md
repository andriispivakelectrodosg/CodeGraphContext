# Tech Context

## Repository
- **Fork**: [andriispivakelectrodosg/CodeGraphContext](https://github.com/andriispivakelectrodosg/CodeGraphContext)
- **Upstream**: [CodeGraphContext/CodeGraphContext](https://github.com/CodeGraphContext/CodeGraphContext)
- **Local path**: /home/narayanaya/CodeGraphContext
- **Version**: 0.3.0
- **License**: MIT

## Language & Runtime
- **Python**: 3.10–3.14
- **Package manager**: pip (pyproject.toml-based)
- **Entry point**: `cgc` CLI via `codegraphcontext.cli.main:app`

## Core Dependencies
| Package | Purpose |
|---------|---------||
| tree-sitter + tree-sitter-language-pack | Source code parsing (14 languages) |
| typer[all] + rich + inquirerpy | CLI framework, rich output, interactive prompts |
| neo4j | Neo4j graph database driver |
| falkordb + falkordblite | FalkorDB graph database drivers |
| kuzu | KùzuDB embedded graph database (default backend) |
| watchdog | File system monitoring for live updates |
| python-dotenv | Environment variable management |
| stdlibs | Python stdlib detection |
| pyyaml | YAML configuration parsing |
| pathspec | .cgcignore pattern matching |
| nbformat + nbconvert | Jupyter notebook parsing |
| protobuf (optional, `[scip]`) | SCIP index parsing |

## Dev Dependencies
- pytest, pytest-asyncio — testing
- black — code formatting

## Supported Languages (tree-sitter parsers)
Python, JavaScript, TypeScript, TypeScriptJSX, Java, C, C++, C#, Go, Rust, Ruby, PHP, Swift, Kotlin, Dart, Perl, Scala, Elixir, Haskell

## Database Backends
| Backend | Platform | Setup | Default |
|---------|----------|-------|---------||
| KùzuDB | All (Win/Mac/Linux) | Zero-config embedded | **Yes** |
| FalkorDB Lite | Unix only (Linux/macOS/WSL) | In-process, Python 3.12+ | No |
| FalkorDB Remote | All | External server | No |
| Neo4j | All | Docker or native install | No |

## Build & Run
```bash
# Development install
pip install -e ".[scip,dev]"

# Run CLI
cgc --help

# Run MCP server
cgc mcp start

# Run tests
pytest tests/
```

## Project Structure
```
pyproject.toml         ← package config, dependencies, entry points
src/codegraphcontext/  ← main source package
tests/                 ← unit, integration, e2e, performance tests
docs/                  ← documentation (MkDocs)
scripts/               ← utility scripts
vscode-extension/      ← VS Code extension (separate)
k8s/                   ← Kubernetes deployment configs
```

## CI/CD
- GitHub Actions workflows in `.github/`
- Tests: pytest with unit/integration/e2e separation
- Markers: `@pytest.mark.integration`, `@pytest.mark.e2e`, `@pytest.mark.slow`

## Constraints
- Must maintain backward compatibility with upstream CGC API surface
- SCIP support is opt-in via `[scip]` extra
- FalkorDB Lite only works on Unix with Python 3.12+
- Git push requires MCP GitHub API tools (user216 has no CLI push access to andriispivakelectrodosg org)
