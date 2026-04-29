[![MseeP.ai Security Assessment Badge](https://mseep.net/pr/x-hannibal-mcp-memento-badge.png)](https://mseep.ai/app/x-hannibal-mcp-memento)

# MCP Memento

[![Python Version](https://img.shields.io/badge/python-3.10+-blue.svg)](https://www.python.org/downloads/)
[![License](https://img.shields.io/badge/license-MIT-green.svg)](LICENSE)
[![MCP Protocol](https://img.shields.io/badge/MCP-Protocol-blueviolet)](https://spec.modelcontextprotocol.io/)
[![Latest Release](https://img.shields.io/badge/release-v0.2.37-purple.svg)](https://github.com/x-hannibal/mcp-memento/releases/tag/v0.2.37)
[![Beta](https://img.shields.io/badge/status-beta-orange.svg)](https://github.com/x-hannibal/mcp-memento/issues)

Intelligent memory management for MCP clients with confidence tracking, relationship mapping, and knowledge quality maintenance.

Memento is an MCP server that provides persistent memory capabilities across multiple platforms:
- **IDEs**: Zed, Cursor, Windsurf, VSCode, Claude Desktop
- **CLI Agents**: Gemini CLI, Claude CLI, custom agents
- **Programmatic Usage**: MCP client (Python), Docker deployment, CLI export/import
- **Applications**: Any MCP-compatible application

Build a personal or team knowledge base that grows smarter over time, accessible from all your development tools.

## Table of Contents

- [🌱 A Gentle Introduction](#a-gentle-introduction)
- [✨ Key Features](#key-features)
- [🚀 Quick Start](#quick-start)
- [⚙️ Configuration](#configuration)
- [📖 Core Concepts](#core-concepts)
- [🔗 Integrations](#integrations)
- [🛠️ Basic Usage Examples](#basic-usage-examples)
- [📚 Documentation Structure](#documentation-structure)
- [🏗️ Architecture Overview](#architecture-overview)
- [📜 Background](#background)
- [🙏 Acknowledgments](#acknowledgments)
- [🤝 Contributing](#contributing)
- [📄 License](#license)
- [🔗 Links](#links)

<a name="a-gentle-introduction"></a>
## 🌱 A Gentle Introduction

**What is Memento?**
Imagine you're solving a complex bug, figuring out a tricky configuration, or establishing a new coding pattern. Usually, you'd forget the details in a few weeks. Memento is a "long-term memory drive" for your AI assistant. It allows your AI to save these solutions, decisions, and facts so it can recall them instantly across different projects, even months later.

**💡 The Agentic Mindset: A Guide for Traditional Developers**
If you are used to deterministic software (where things happen automatically because a script says so), interacting with AI agents requires a slight mental shift. 

Memento is **not an autonomous agent** that watches your screen and magically decides what to remember. Instead, Memento is a *toolbelt* provided to your AI assistant (like Claude, Cursor, or Gemini). 

* **The AI is the worker:** It needs to be told when to use the toolbelt. Nothing is saved without explicit instruction or a pre-defined rule.
* **You are the manager:** You control what gets stored. You can either tell the AI during a chat (*"Save this database connection string"*), or you can give the AI standard operating procedures (via system prompts or `.cursorrules`/`CLAUDE.md` files) so it knows to automatically save certain things, like bug fixes or architecture decisions.

**How to build the habit:**
1. **Start of session:** Ask your AI, *"What do we know about the authentication system?"* to pull context.
2. **During work:** When you fix a tricky issue, say, *"We fixed the Redis timeout. Store this solution."*
3. **End of session:** Tell your AI, *"Store a summary of what we accomplished today."*

Alternatively, you can add custom instructions to your AI (see our [Agent Configuration Guide](docs/AGENT_CONFIGURATION.md)) to make it automatically execute these steps without you having to ask every time.

<a name="key-features"></a>
## ✨ Key Features

### 🧠 Intelligent Confidence System
- **Automatic decay**: Unused knowledge loses confidence over time (5% monthly)
- **Critical protection**: Security/auth/API key memories never decay
- **Boost on validation**: Confidence increases when knowledge is successfully used
- **Smart ordering**: Search results ranked by `confidence × importance`

### 🔗 Relationship Mapping
- **35 relationship types**: SOLVES, CAUSES, IMPROVES, USED_IN, etc. across 7 semantic categories (see [Relationship Types Reference](docs/RELATIONSHIPS.md))
- **Graph navigation**: Find connections between concepts
- **Pattern detection**: Identify recurring solution patterns

### 📊 Three Profile System
| Profile | Tools | Best For |
|---------|-------|----------|
| **Core** | 13 tools | All users - Essential operations |
| **Extended** | 17 tools | Power users - Statistics, contextual search, decay control |
| **Advanced** | 25 tools | Administrators - Graph analysis |

### 🗃️ Cross-Platform Storage
- **SQLite backend**: Zero dependencies, local storage
- **Full-text search**: Fast, fuzzy matching across all memories
- **Automatic maintenance**: Confidence decay, relationship integrity
- **Shared database**: Same database works across all integrations

<a name="quick-start"></a>
## 🚀 Quick Start

### 1. Installation

```bash
# Install with pipx (recommended for MCP servers)
pipx install mcp-memento

# Or with pip
pip install mcp-memento
```

### 2. Basic Configuration

Memento supports multiple configuration methods. For clarity, we recommend using **one method consistently**:

**Method 1: CLI Arguments** (recommended - most explicit)
```json
{
  "mcpServers": {
    "memento": {
      "command": "memento",
      "args": ["--profile", "extended", "--db", "~/.mcp-memento/context.db"]
    }
  }
}
```

**Method 2: Environment Variables**
```json
{
  "mcpServers": {
    "memento": {
      "command": "memento",
      "args": [],
      "env": {
        "MEMENTO_PROFILE": "extended",
        "MEMENTO_DB_PATH": "~/.mcp-memento/context.db"
      }
    }
  }
}
```

**Method 3: YAML Configuration File**
Create `~/.mcp-memento/config.yaml`:
```yaml
profile: extended
db_path: ~/.mcp-memento/context.db
```
Then use minimal JSON config:
```json
{
  "mcpServers": {
    "memento": {
      "command": "memento",
      "args": []
    }
  }
}
```

**CLI Agents** (Gemini CLI):
```bash
gemini --mcp-servers memento
```
> **Note**: The exact flag syntax depends on your Gemini CLI version. Refer to [AGENT_CONFIGURATION.md](./docs/AGENT_CONFIGURATION.md) for version-specific setup instructions.

### 3. First Steps

Once configured, your AI assistant can now:

```python
# Store solutions and knowledge
store_memento(
    type="solution",
    title="Fixed Redis timeout with connection pooling",
    content="Increased connection timeout to 30s and added connection pooling...",
    tags=["redis", "timeout", "production_fix"],
    importance=0.8
)

# Find knowledge later
recall_mementos(query="Redis timeout solutions")
```

> **📌 Note**: The code above represents **MCP tool calls** — instructions you give
> your AI assistant (Claude, Cursor, Gemini, etc.) to invoke Memento's tools.
> This is **not** a Python library you can `import`. For programmatic Python access
> see the [Python Integration Guide](docs/integrations/PYTHON.md).

**💬 Natural Language**: You can also interact with Memento through natural conversation. Just tell your AI assistant things like "Remember that..." or "Store this..." or "Memento..."- no code required.


<a name="core-concepts"></a>
## 📖 Core Concepts

For a deep dive into Memento's concepts (Confidence System, Tagging, Relationships), please read the comprehensive [RULES.md](./docs/RULES.md) and [RELATIONSHIPS.md](./docs/RELATIONSHIPS.md) documentation.

<a name="integrations"></a>
## 🔗 Integrations

Memento works with all major development tools:

| Platform | Configuration Guide | Notes |
|----------|---------------------|-------|
| **Zed Editor** | [IDE Integration](docs/integrations/IDE.md#zed-editor) | Native MCP support |
| **Cursor** | [IDE Integration](docs/integrations/IDE.md#cursor) | AI-powered editor |
| **Windsurf** | [IDE Integration](docs/integrations/IDE.md#windsurf) | Modern code editor |
| **VSCode** | [IDE Integration](docs/integrations/IDE.md#vscode) | Via MCP extension |
| **Claude Desktop** | [IDE Integration](docs/integrations/IDE.md#claude-desktop) | Desktop application |
| **Gemini CLI** | [Agent Integration](docs/integrations/AGENT.md#gemini-cli) | Google's CLI agent |
| **Claude CLI** | [Agent Integration](docs/integrations/AGENT.md#claude-cli) | Anthropic's CLI agent |
| **Python / MCP Client** | [Python Integration](docs/integrations/PYTHON.md) | Embed server or call via MCP client |
| **Docker / CLI** | [API & Programmatic](docs/integrations/API.md) | MCP client, Docker, export/import |

**See also**: [Integration Overview](docs/INTEGRATION.md) for guidance on choosing the right integration.

<a name="basic-usage-examples"></a>
## 🛠️ Basic Usage Examples

The examples below show the **MCP tool calls** that an AI assistant (Zed, Cursor,
Claude, Gemini CLI, …) executes on your behalf when you ask it to remember or
retrieve something. They are written in a Python-like pseudocode that mirrors the
MCP tool interface — they are **not** a Python library you import directly.

> To call these tools programmatically from Python, use the `mcp` client library.
> See [Python Integration](docs/integrations/PYTHON.md) for a working example.

### Store and Retrieve Knowledge

```python
# Store a solution — the AI calls this tool when you say "remember this fix"
solution_id = store_memento(
    type="solution",
    title="Fixed memory leak in WebSocket handler",
    content="Added proper cleanup in on_close()...",
    tags=["websocket", "memory", "python"],
    importance=0.9
)

# Natural language search — called when you ask "what do you know about X"
results = recall_mementos(query="WebSocket memory leak", limit=5)

# Tag-based search — for precise filtering
redis_solutions = search_mementos(tags=["redis"], memory_types=["solution"])
```

### Manage Confidence

```python
# Find potentially obsolete knowledge
low_confidence = get_low_confidence_mementos(threshold=0.3)

# Boost confidence after verification
boost_memento_confidence(
    memory_id=verified_solution_id,
    boost_amount=0.15,
    reason="Verified in production deployment"
)
```

### Create Relationships

```python
# Link solution to problem
create_memento_relationship(
    from_memory_id=solution_id,
    to_memory_id=problem_id,
    relationship_type="SOLVES",  # See all 35 types in docs/RELATIONSHIPS.md
    strength=0.9,
    context="Connection pooling resolved the timeout issue"
)

# Explore connected knowledge
related = get_related_mementos(
    memory_id=solution_id,
    relationship_types=["RELATED_TO", "USED_IN"],
    max_depth=2
)
```

### Natural Language Interaction (Chat-Based)

Memento works through natural language conversations. The AI assistant interprets intent and calls the appropriate tools automatically.

**Store information:**
```
User: Remember that we solved Redis timeout with connection pooling
AI: ✅ Memento stored - "Redis timeout solution: connection pooling"
```

**Retrieve knowledge:**
```
User: What do you remember about Redis timeout?
AI: Found 2 solutions: 1) Connection pooling... 2) Query optimization...
```

**Using the "Memento" keyword:**
```
User: Memento the deployment script is in /scripts/deploy.sh
AI: ✅ Memento stored - "Deployment script location: /scripts/deploy.sh"
```

The AI can also store important information automatically when configured with the guidelines in [AGENT_CONFIGURATION.md](./docs/AGENT_CONFIGURATION.md).

<a name="configuration"></a>
## ⚙️ Configuration

Memento supports multiple configuration sources (in order of precedence):

1. **Command-Line Arguments** (highest priority)
   ```bash
   memento --profile advanced --db ~/custom/path/memento.db --log-level DEBUG
   ```

2. **Environment Variables**
   ```bash
   export MEMENTO_PROFILE="advanced"
   export MEMENTO_DB_PATH="~/custom/path/memento.db"
   export MEMENTO_LOG_LEVEL="DEBUG"
   export MEMENTO_ALLOW_CYCLES="false"   # Allow cycles in relationship graph
   ```

3. **YAML Configuration Files**
   - Project config: `./memento.yaml` in current directory (overrides global)
   - Global config: `~/.mcp-memento/config.yaml`

**Priority Order**: CLI Arguments > Environment Variables > Project YAML > Global YAML > Defaults

4. **Default Values** (lowest priority)

### Supported YAML Keys

The following keys are **read and applied** by the configuration loader. Any other keys present in the YAML file are silently ignored.

| Key | Type | Default | Description |
|-----|------|---------|-------------|
| `db_path` | string | `~/.mcp-memento/context.db` | SQLite database file path |
| `profile` | string | `core` | Tool profile (`core`, `extended`, `advanced`) |
| `logging.level` | string | `INFO` | Log level (`DEBUG`, `INFO`, `WARNING`, `ERROR`) |
| `features.allow_relationship_cycles` | bool | `false` | Allow cyclic relationships in the graph |

> **Note**: The `memento.yaml` template shipped with the project contains additional
> commented sections (`confidence`, `search`, `performance`, `memory`, `fts`, `project`).
> These are **not yet implemented** — they are aspirational placeholders for future
> releases and have no effect on the current server behaviour.

### Example Configuration Files

**Project configuration** (`./memento.yaml`):
```yaml
db_path: ~/.mcp-memento/context.db
profile: extended
logging:
  level: INFO
features:
  allow_relationship_cycles: false
```

**Global configuration** (`~/.mcp-memento/config.yaml`):
```yaml
db_path: ~/.mcp-memento/global.db
profile: extended
logging:
  level: INFO
```

<a name="documentation-structure"></a>
## 📚 Documentation Structure

### Essential Guides
- **[Tools Reference](docs/TOOLS.md)** - Complete guide to all MCP tools
- **[Confidence System](docs/DECAY_SYSTEM.md)** - How confidence tracking works
- **[Relationship Types](docs/RELATIONSHIPS.md)** - All 35 relationship types with examples
- **[Usage Rules](docs/RULES.md)** - Best practices and conventions
- **[Agent Configuration](docs/AGENT_CONFIGURATION.md)** - Templates for AI agents

### Integration Guides
- **[Integration Overview](docs/INTEGRATION.md)** - Choosing the right integration
- **[IDE Integration](docs/integrations/IDE.md)** - Zed, Cursor, Windsurf, VSCode, Claude Desktop
- **[Python Integration](docs/integrations/PYTHON.md)** - MCP client usage, server embedding, CLI export/import
- **[Agent Integration](docs/integrations/AGENT.md)** - CLI agents and custom applications
- **[API & Programmatic Integration](docs/integrations/API.md)** - MCP client (Python), Docker deployment, CLI export/import

### Development & Advanced Topics
- **[Database Schema](docs/dev/SCHEMA.md)** - Technical database structure
- **[Contributing Guidelines](CONTRIBUTING.md)** - Development setup and workflow

<a name="architecture-overview"></a>
## 🏗️ Architecture Overview

### Database Schema
Memento uses a unified SQLite schema accessible from all integrations:
- **Core tables**: `nodes` (memory storage), `relationships` (directed graph)
- **Full-text search**: `nodes_fts` — FTS5 virtual table for fast searching (falls back to LIKE-based search if FTS5 is unavailable)
- **Confidence tracking**: Automatic decay with protection for critical memories

### Consistent Behavior
The system works identically across all platforms:
1. **Same database**: All tools access the same SQLite file
2. **Same confidence tracking**: Updates from one tool reflected everywhere
3. **Same search ranking**: Results ordered by `confidence × importance`
4. **Same relationship types**: 35 semantic relationship types available everywhere

<a name="background"></a>
## 📜 Background

Memento is a simplified, lightweight fork of [MemoryGraph](https://github.com/memory-graph/memory-graph) by Gregory Dickson, optimized for MCP integration across IDEs and CLI agents.

The fork focuses on portability and token efficiency: it removes heavy dependencies (NetworkX, multi-backend storage, bi-temporal tracking, multi-tenant architecture) in favor of a SQLite-only backend with confidence-based decay and guideline-driven storage.

### Team Collaboration & Remote Deployment

Multiple users can share a SQLite database (e.g., on network storage) using tagging conventions (`team:[name]`, `author:[name]`). Memento can also run as a remote MCP server, though all clients share the same database without tenant isolation. See [Team Collaboration guidelines](docs/AGENT_CONFIGURATION.md#advanced-team-collaboration) for details.

For true multi-tenancy, use the original MemoryGraph project.

### When to Choose MemoryGraph vs Memento?
- **Use Memento**: For lightweight, cross-platform memory management in IDEs and CLI tools
- **Use MemoryGraph**: For enterprise use cases requiring multi-tenancy, bi-temporal tracking, or custom backends

<a name="acknowledgments"></a>
## 🙏 Acknowledgments

Memento is built upon the solid foundation of Gregory Dickson's [MemoryGraph](https://github.com/memory-graph/memory-graph) project. We're grateful for his pioneering work in memory management systems.

This fork maintains compatibility with MemoryGraph's core concepts while adapting them for the specific needs of MCP integration and modern development tooling. For users requiring the full power of MemoryGraph's advanced features, we recommend exploring the original project.

## 🧪 Beta Status

mcp-webgate is in **beta**. Core functionality is stable and the server is used in production,
but the configuration API may still change before 1.0.

**Feedback is very welcome.** If something doesn't work as expected, behaves oddly,
or you have a use case that isn't covered:

→ [Open an issue on GitHub](https://github.com/x-hannibal/mcp-memento/issues)

Bug reports, configuration questions, and feature requests all help shape the roadmap.

<a name="contributing"></a>
## 🤝 Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](CONTRIBUTING.md) for detailed guidelines on:
- Development setup and workflow
- Code style and conventions
- Testing requirements
- Documentation standards
- Pull request process

<a name="license"></a>
## 📄 License

MIT License - see [LICENSE](LICENSE) for details.

<a name="links"></a>
## 🔗 Links

- **[GitHub Repository](https://github.com/x-hannibal/mcp-memento)** - Source code and issues
- **[MCP Protocol](https://spec.modelcontextprotocol.io/)** - Model Context Protocol specification
- **[PyPI Package](https://pypi.org/project/mcp-memento/)** - Python Package Index
- **[MCP Registry](https://registry.modelcontextprotocol.io/?q=mcp-memento&all=1)** - Model Context Protocol Registry

---

**Need help?** Check the [documentation](docs/) or open an [issue](https://github.com/x-hannibal/mcp-memento/issues) on GitHub.

<!-- mcp-name: io.github.x-hannibal/mcp-memento -->
