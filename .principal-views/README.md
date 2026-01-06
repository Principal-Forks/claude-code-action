# Architecture Documentation

This directory contains architecture diagrams for the Claude Code Action project using [Principal View](https://github.com/principal-ai/principal-view).

## Diagrams

### 1. High-Level Overview (`architecture-overview.canvas`)

**Purpose**: Bird's-eye view of the system's major phases and interactions.

**Best for**:

- Understanding the overall flow
- Onboarding new developers
- Explaining the system to stakeholders
- Quick reference

**Components**:

- **External Triggers**: GitHub webhooks and workflow events
- **Preparation Phase**: Authentication, validation, mode detection, data fetching
- **Execution Mode**: Tag mode (@claude mentions) vs Agent mode (automation)
- **Claude Execution**: Prompt generation, MCP setup, and AI processing
- **Results Processing**: Comment updates and output formatting
- **External Services**: GitHub API and Claude AI providers

### 2. Detailed Architecture (`architecture.canvas`)

**Purpose**: Complete technical view showing all components and their relationships.

**Best for**:

- Deep diving into specific components
- Understanding detailed data flows
- Development and debugging
- System design decisions

**Components** (22 total):

- Entry points (prepare, update-comment, format-turns)
- Security layer (authentication, validation)
- GitHub integration (context parser, data fetcher, data formatter)
- Mode system (tag mode, agent mode)
- Operations (branch manager, comment manager)
- Prompt generation
- MCP servers (comment, file ops, inline comments)
- Claude execution
- External services (GitHub API, Claude API)

**Edges** (33 total):

- Data flows (blue solid)
- API calls (purple dashed)
- Control flows (green solid)
- Service integrations (orange dotted)
- Configuration (indigo dashed)

## Component Library (`library.yaml`)

Defines reusable component types and edge types used across all diagrams:

**Node Types**:

- `entrypoint` - Entry points and exit points
- `mode-system` - Execution mode logic
- `github-integration` - GitHub API integration
- `authentication` - Auth and token management
- `validation` - Permission and trigger validation
- `operations` - Git and GitHub operations
- `mcp-server` - Model Context Protocol servers
- `prompt-generation` - Prompt creation
- `claude-execution` - Claude Code CLI execution
- `external-service` - External APIs and services

**Edge Types**:

- `data-flow` - Data transformation and movement
- `api-call` - External API interactions
- `control-flow` - Control and orchestration
- `service-integration` - Service-to-service communication
- `configuration` - Configuration and setup

## How to Use

### View Diagrams

1. **JSON Canvas Viewers**:

   - [Obsidian](https://obsidian.md/) with Canvas plugin
   - Any JSON Canvas compatible tool

2. **Command Line**:

   ```bash
   # Validate diagrams
   npx @principal-ai/principal-view-cli validate .principal-views/*.canvas

   # Check for issues
   npx @principal-ai/principal-view-cli lint
   ```

### Modify Diagrams

1. Edit the `.canvas` files directly (they're JSON)
2. Or use a JSON Canvas compatible editor
3. Validate after changes:
   ```bash
   npx @principal-ai/principal-view-cli validate .principal-views/*.canvas
   ```

### Pre-commit Hook

The project includes a Husky pre-commit hook that automatically validates `.principal-views` files before commits.

## Architecture Flow

### High-Level Flow

```
GitHub Event
    ↓
Preparation Phase
  - Authenticate (OIDC token exchange)
  - Parse event context
  - Validate permissions & triggers
  - Detect mode (Tag/Agent)
  - Fetch PR/issue data
  - Setup branches & comments
    ↓
Mode Selection
  - Tag Mode: @claude mentions, tracking comments
  - Agent Mode: Direct automation, scheduled
    ↓
Claude Execution
  - Generate prompt with context
  - Configure MCP servers
  - Execute Claude Code CLI
  - Process tool calls
    ↓
Results Processing
  - Update tracking comments
  - Add PR/branch links
  - Format conversation summary
```

### Key Integrations

**GitHub API**:

- REST: Comments, branches, commits, PRs
- GraphQL: Efficient data fetching

**Claude AI**:

- Multiple providers (Anthropic, Bedrock, Vertex, Foundry)
- MCP tool access for GitHub operations

**MCP Servers**:

- Comment Server: Update tracking comments
- File Ops Server: API-based commits
- Inline Comment Server: PR review comments
- GitHub Actions Server: Workflow access

## References

- [Principal View Documentation](https://github.com/principal-ai/principal-view)
- [JSON Canvas Specification](https://jsoncanvas.org/)
- [Claude Code Action Documentation](../README.md)
