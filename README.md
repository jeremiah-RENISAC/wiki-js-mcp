# Wiki.js MCP Server

A comprehensive **Model Context Protocol (MCP) server** for Wiki.js integration with **hierarchical documentation** support. Perfect for organizations managing multiple repositories and large-scale documentation.

## 🚀 Quick Start

### 1. Environment Setup

First, clone this repository and set up environment variables:
```bash
# Copy environment template
cp config/example.env .env

# Edit .env with your credentials:
# - Set WIKIJS_API_URL to your Wiki.js instance
# - Set WIKIJS_TOKEN to your Wiki.js API key
```

### 2. Setup MCP Server
```bash
# Install Python dependencies
./setup.sh

# Update .env with Wiki.js API credentials:
# - Get API key from Wiki.js admin panel  
# - Set WIKIJS_TOKEN in .env file

# Test the connection
./test-server.sh

# Start MCP server manually only when debugging.
# Codex starts this script automatically after you configure the MCP server.
./start-server.sh
```

### 3. Configure Codex MCP

Codex stores MCP servers in `config.toml`. Add this to `~/.codex/config.toml`, or to `.codex/config.toml` inside a trusted project if you want the Wiki.js server scoped to that project:

```toml
[mcp_servers.wikijs]
command = "/absolute/path/to/wiki-js-mcp/start-server.sh"
cwd = "/absolute/path/to/wiki-js-mcp"
startup_timeout_sec = 20
tool_timeout_sec = 60
```

Restart Codex or start a new session after updating the config. In the Codex CLI/TUI, run `/mcp` to verify that the `wikijs` server is active.

## 🎯 Enhanced Codex Integration

### AGENTS.md Guidance for Documentation-First Development
Add this guidance to `~/.codex/AGENTS.md` for a global default, or to an `AGENTS.md` file in a repository where this workflow should apply:

```
# ~/.codex/AGENTS.md

## WikiJS MCP usage
- Use WikiJS only when work depends on internal context: services, environments, deployments, operations, playbooks, conventions. Use repository files for current code behavior. Use public/vendor docs for external APIs, frameworks, packages, and advisories.
- Before environment-dependent or operational changes, search WikiJS with relevant project, component, procedure, alert, symptom, environment, etc.
- Prefer pages with this structure when available:
  - `About`: high-level service overview, ownership, links, environments, architecture.
  - `Procedures`: planned, repeatable work while the system is healthy.
  - `Response`: alerts, incidents, symptoms, known errors, and unplanned investigation.
- Treat WikiJS as internal context that may be stale. If WikiJS conflicts with the repo, observed behavior, or public/vendor docs, rely on the source that owns that fact and mention the conflict.
- Write to WikiJS only when documenting standard processes, procedures, response playbooks, alert guidance, or component context; only after user approval unless explicitly asked.
- When writing, update existing pages instead of creating duplicates. Follow existing page standards, layout, or templates. Keep content factual, concise, operational, and project-agnostic where possible. Do not include secrets, customer data, or speculation.
- Do not permanently delete material content. Existing content may be updated, changed, or reworked as appropriate. When content appears obsolete, incorrect, or requested for removal, mark it for removal on the page for human review.
- When WikiJS materially affects the work, summarize pages consulted, internal facts relied on, documentation gaps or conflicts found.
```

This guidance helps Codex:
- ✅ Check documentation before suggesting implementations
- ✅ Follow existing patterns and conventions
- ✅ Maintain up-to-date documentation automatically
- ✅ Create structured documentation for new features
- ✅ Avoid duplicating existing functionality

### Usage Tips for Codex
```
# Before starting a new feature
"Search the documentation for authentication patterns before implementing login"

# When creating components
"Create nested documentation under frontend-app/components before building the React component"

# For API development
"Check existing API documentation and create endpoint docs using the established structure"

# During refactoring
"Update all related documentation pages for the files I'm about to modify"
```

## 🚀 Key Features

### 📁 **Hierarchical Documentation**
- **Repository-level organization**: Create structured docs for multiple repos
- **Nested page creation**: Automatic parent-child relationships
- **Auto-organization**: Smart categorization by file type (components, API, utils, etc.)
- **Enterprise scalability**: Handle hundreds of repos and thousands of files

### 🔧 **Core Functionality**
- **GraphQL API integration**: Full Wiki.js v2+ compatibility
- **File-to-page mapping**: Automatic linking between source code and documentation
- **Code structure analysis**: Extract classes, functions, and dependencies
- **Bulk operations**: Update multiple pages simultaneously
- **Change tracking**: Monitor file modifications and sync docs

### 🔍 **Smart Features**
- **Repository context detection**: Auto-detect Git repositories
- **Content generation**: Auto-create documentation from code structure
- **Search integration**: Full-text search across hierarchical content
- **Health monitoring**: Connection status and error handling

## 📊 MCP Tools (21 Total)

### 🏗️ **Hierarchical Documentation Tools**
1. **`wikijs_create_repo_structure`** - Create complete repository documentation structure
2. **`wikijs_create_nested_page`** - Create pages with hierarchical paths
3. **`wikijs_get_page_children`** - Navigate parent-child page relationships
4. **`wikijs_create_documentation_hierarchy`** - Auto-organize project files into docs

### 📝 **Core Page Management**
5. **`wikijs_create_page`** - Create new pages (now with parent support)
6. **`wikijs_update_page`** - Update existing pages
7. **`wikijs_get_page`** - Retrieve page content and metadata
8. **`wikijs_search_pages`** - Search pages by text (fixed GraphQL issues)

### 🗑️ **Deletion & Cleanup Tools**
9. **`wikijs_delete_page`** - Delete specific pages by ID or path
10. **`wikijs_batch_delete_pages`** - Batch delete with pattern matching and safety checks
11. **`wikijs_delete_hierarchy`** - Delete entire page hierarchies with multiple modes
12. **`wikijs_cleanup_orphaned_mappings`** - Clean up orphaned file-to-page mappings

### 🗂️ **Organization & Structure**
13. **`wikijs_list_spaces`** - List top-level documentation spaces
14. **`wikijs_create_space`** - Create new documentation spaces
15. **`wikijs_manage_collections`** - Manage page collections

### 🔗 **File Integration**
16. **`wikijs_link_file_to_page`** - Link source files to documentation pages
17. **`wikijs_sync_file_docs`** - Sync code changes to documentation
18. **`wikijs_generate_file_overview`** - Auto-generate file documentation

### 🚀 **Bulk Operations**
19. **`wikijs_bulk_update_project_docs`** - Batch update multiple pages

### 🔧 **System Tools**
20. **`wikijs_connection_status`** - Check API connection health
21. **`wikijs_repository_context`** - Show repository mappings and context

## 🏢 Enterprise Use Cases

### Multi-Repository Documentation
```
Company Documentation/
├── frontend-web-app/
│   ├── Overview/
│   ├── Components/
│   │   ├── Button/
│   │   ├── Modal/
│   │   └── Form/
│   ├── API Integration/
│   └── Deployment/
├── backend-api/
│   ├── Overview/
│   ├── Controllers/
│   ├── Models/
│   └── Database Schema/
├── mobile-app/
│   ├── Overview/
│   ├── Screens/
│   └── Native Components/
└── shared-libraries/
    ├── UI Components/
    ├── Utilities/
    └── Type Definitions/
```

### Automatic Organization
The system intelligently categorizes files:
- **Components**: React/Vue components, UI elements
- **API**: Endpoints, controllers, routes
- **Utils**: Helper functions, utilities
- **Services**: Business logic, external integrations
- **Models**: Data models, types, schemas
- **Tests**: Unit tests, integration tests
- **Config**: Configuration files, environment setup

## 📚 Usage Examples

### Create Repository Documentation
```python
# Create complete repository structure
await wikijs_create_repo_structure(
    "My Frontend App",
    "Modern React application with TypeScript",
    ["Overview", "Components", "API", "Testing", "Deployment"]
)

# Create nested component documentation
await wikijs_create_nested_page(
    "Button Component",
    "# Button Component\n\nReusable button with multiple variants...",
    "my-frontend-app/components"
)

# Auto-organize entire project
await wikijs_create_documentation_hierarchy(
    "My Project",
    [
        {"file_path": "src/components/Button.tsx"},
        {"file_path": "src/api/users.ts"},
        {"file_path": "src/utils/helpers.ts"}
    ],
    auto_organize=True
)
```

### Documentation Management
```python
# Clean up and manage documentation
# Preview what would be deleted (safe)
preview = await wikijs_delete_hierarchy(
    "old-project",
    delete_mode="include_root",
    confirm_deletion=False
)

# Delete entire deprecated project
await wikijs_delete_hierarchy(
    "old-project",
    delete_mode="include_root", 
    confirm_deletion=True
)

# Batch delete test pages
await wikijs_batch_delete_pages(
    path_pattern="*test*",
    confirm_deletion=True
)

# Clean up orphaned file mappings
await wikijs_cleanup_orphaned_mappings()
```

## ⚙️ Configuration

### Environment Variables
```bash
# Wiki.js Connection
WIKIJS_API_URL=https://your-wiki.example.com
WIKIJS_TOKEN=your_api_key_here

# Database & Logging
WIKIJS_MCP_DB=./wikijs_mappings.db
LOG_LEVEL=INFO
LOG_FILE=wikijs_mcp.log

# Repository Settings
REPOSITORY_ROOT=./
DEFAULT_SPACE_NAME=Documentation
```

### Authentication Options
1. **JWT Token** (Recommended): Use API key from Wiki.js admin panel
2. **Username/Password**: Traditional login credentials

## 🔧 Technical Architecture

### GraphQL Integration
- **Full GraphQL API support**: Native Wiki.js v2+ compatibility
- **Optimized queries**: Efficient data fetching and mutations
- **Error handling**: Comprehensive GraphQL error management
- **Retry logic**: Automatic retry with exponential backoff

### Database Layer
- **SQLite storage**: Local file-to-page mappings
- **Repository context**: Git repository detection and tracking
- **Change tracking**: File hash monitoring for sync detection
- **Relationship management**: Parent-child page hierarchies

### Code Analysis
- **AST parsing**: Extract Python classes, functions, imports
- **Structure detection**: Identify code patterns and organization
- **Documentation generation**: Auto-create comprehensive overviews
- **Dependency mapping**: Track imports and relationships

## 📈 Performance & Scalability

- **Async operations**: Non-blocking I/O for all API calls
- **Bulk processing**: Efficient batch operations for large projects
- **Caching**: Smart caching of page relationships and metadata
- **Connection pooling**: Optimized HTTP client management

## 🛠️ Development

### Project Structure
```
wiki-js-mcp/
├── src/
│   └── wiki_mcp_server.py      # Main MCP server implementation
├── config/
│   └── example.env             # Configuration template
├── pyproject.toml              # Poetry dependencies
├── requirements.txt            # Pip dependencies
├── setup.sh                    # Environment setup script
├── start-server.sh             # MCP server launcher
├── test-server.sh              # Interactive testing script
├── HIERARCHICAL_FEATURES.md    # Hierarchical documentation guide
├── DELETION_TOOLS.md           # Deletion and cleanup guide
├── LICENSE                     # MIT License
└── README.md                   # This file
```

### Dependencies
- **FastMCP**: Official Python MCP SDK
- **httpx**: Async HTTP client for GraphQL
- **SQLAlchemy**: Database ORM for mappings
- **Pydantic**: Configuration and validation
- **tenacity**: Retry logic for reliability

## 🔍 Troubleshooting

### Connection Issues
```bash
# Check Wiki.js is reachable
curl "$WIKIJS_API_URL/graphql"

# Verify authentication
./test-server.sh

# Debug mode
export LOG_LEVEL=DEBUG
./start-server.sh
```

### Common Problems
- **API permissions**: Ensure the API key has the Wiki.js permissions required for the tools you use
- **Python dependencies**: Run `./setup.sh` to reinstall

## 📚 Documentation

- **[Hierarchical Features Guide](HIERARCHICAL_FEATURES.md)** - Complete guide to enterprise documentation
- **[Deletion Tools Guide](DELETION_TOOLS.md)** - Comprehensive deletion and cleanup tools
- **[Configuration Examples](config/example.env)** - Environment setup

## 🤝 Contributing

1. Fork the repository
2. Create feature branch (`git checkout -b feature/amazing-feature`)
3. Commit changes (`git commit -m 'Add amazing feature'`)
4. Push to branch (`git push origin feature/amazing-feature`)
5. Open Pull Request

## 📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

## 🙏 Acknowledgments

- **Wiki.js Team**: For the excellent documentation platform
- **MCP Protocol**: For the standardized AI integration framework
- **FastMCP**: For the Python MCP SDK

---

**Ready to scale your documentation?** 🚀 Start with `wikijs_create_repo_structure` and build enterprise-grade documentation hierarchies! Use Codex guidance to ensure documentation-first development! 📚✨ 
