# OpenROD - Open RAG on Demand

**OpenROD** is an open-source MCP (Model Context Protocol) server that enables shared memory and context between multiple AI clients like Claude Desktop and Cursor. By leveraging a vector database and tagging system, OpenROD creates a unified knowledge layer that persists across different AI applications.

## 🚀 Features

- **Cross-Client Memory Sharing**: Share context and knowledge between Claude Desktop, Cursor, and other MCP-compatible clients
- **Vector Database Storage**: Efficient storage and retrieval using vector embeddings for semantic search
- **Tagging System**: Organize and categorize memories with flexible tagging
- **Web UI**: User-friendly interface for manual memory management
- **MCP Integration**: Seamless integration with the Model Context Protocol ecosystem
- **Docker Deployment**: Easy deployment using Docker Compose
- **Local-First**: Runs entirely on your local machine for privacy and control

## 🏗️ Architecture

OpenROD operates as a bridge between your AI clients and a persistent knowledge base:

```
┌─────────────────┐    ┌──────────────┐    ┌─────────────────┐
│   Claude        │    │              │    │     Cursor      │
│   Desktop       │◄──►│   OpenROD    │◄──►│                 │
│                 │    │  MCP Server  │    │                 │
└─────────────────┘    │              │    └─────────────────┘
                       │              │
                       │   ┌─────────┴──────────┐
                       │   │ Vector Database    │
                       │   │ (Tagged Memories)  │
                       │   └─────────┬──────────┘
                       │             │
                       │   ┌─────────┴──────────┐
                       │   │     Web UI         │
                       │   │ (Memory Management)│
                       └───┴────────────────────┘
```

## 📋 Prerequisites

- Docker and Docker Compose
- Compatible MCP clients (Claude Desktop, Cursor, etc.)
- Minimum 4GB RAM recommended
- 2GB available disk space

## 🛠️ Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/Open-ROD/openrod.git
   cd openrod
   ```

2. **Configure environment variables**
   ```bash
   cp .env.example .env
   # Edit .env with your preferred settings
   ```

3. **Start the services**
   ```bash
   docker-compose up -d
   ```

4. **Verify installation**
   - Web UI: http://localhost:8080
   - MCP Server: localhost:3000
   - Health check: http://localhost:3000/health

## ⚙️ Configuration

### MCP Client Setup

#### Claude Desktop
Add to your Claude Desktop configuration file:

```json
{
  "mcpServers": {
    "openrod": {
      "command": "node",
      "args": ["path/to/openrod/mcp-client.js"],
      "env": {
        "OPENROD_URL": "http://localhost:3000"
      }
    }
  }
}
```

#### Cursor
Configure in your Cursor settings:

```json
{
  "mcp.servers": [
    {
      "name": "openrod",
      "url": "http://localhost:3000"
    }
  ]
}
```

### Environment Variables

| Variable | Description | Default |
|----------|-------------|---------|
| `OPENROD_PORT` | MCP server port | `3000` |
| `WEB_UI_PORT` | Web interface port | `8080` |
| `VECTOR_DB_PATH` | Database storage path | `./data/vectordb` |
| `MAX_MEMORY_SIZE` | Maximum memory size (MB) | `1024` |
| `DEFAULT_TAGS` | Default tags for memories | `general` |

## 📖 Usage

### Adding Memories via Web UI

1. Navigate to http://localhost:8080
2. Click "Add Memory"
3. Enter your content and tags
4. Save to make it available across all connected clients

### Adding Memories via MCP

From any connected MCP client, use the memory functions:

```
# Add a new memory
store_memory("Important project details", ["project", "planning"])

# Search memories
search_memories("project planning")

# List all tags
list_tags()
```

### Memory Management

- **Tagging**: Use descriptive tags to organize memories (`work`, `personal`, `research`, etc.)
- **Search**: Semantic search finds relevant memories even with different wording
- **Updates**: Memories can be updated or deleted through the web UI
- **Backup**: Regular backups are automatically created in `./backups/`

## 🔧 API Reference

### MCP Functions

| Function | Description | Parameters |
|----------|-------------|------------|
| `store_memory` | Store new memory | `content: string, tags: string[]` |
| `search_memories` | Search existing memories | `query: string, limit?: number` |
| `get_memory` | Get specific memory | `id: string` |
| `update_memory` | Update existing memory | `id: string, content: string, tags: string[]` |
| `delete_memory` | Delete memory | `id: string` |
| `list_tags` | List all available tags | None |

### REST API

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/health` | GET | Server health status |
| `/memories` | GET | List all memories |
| `/memories` | POST | Create new memory |
| `/memories/:id` | GET | Get specific memory |
| `/memories/:id` | PUT | Update memory |
| `/memories/:id` | DELETE | Delete memory |
| `/search` | POST | Search memories |
| `/tags` | GET | List all tags |

## 🐳 Docker Services

The Docker Compose setup includes:

- **openrod-server**: Main MCP server application
- **openrod-ui**: Web interface for memory management
- **vector-db**: Vector database for memory storage
- **nginx**: Reverse proxy for routing

## 🔒 Security & Privacy

- **Local-First**: All data stays on your machine
- **No External Calls**: No data sent to external services
- **Encrypted Storage**: Vector database uses encryption at rest
- **Access Control**: Web UI can be protected with authentication

## 🛠️ Development

### Building from Source

```bash
# Clone and setup
git clone https://github.com/Open-ROD/openrod.git
cd openrod

# Install dependencies
npm install

# Development mode
npm run dev

# Build for production
npm run build
```

### Running Tests

```bash
# Unit tests
npm test

# Integration tests
npm run test:integration

# E2E tests
npm run test:e2e
```

## 🤝 Contributing

We welcome contributions! Please see our [Contributing Guide](CONTRIBUTING.md) for details.

1. Fork the repository
2. Create a feature branch
3. Make your changes
4. Add tests if applicable
5. Submit a pull request

## 📄 License

This project is licensed under the Apache 2.0 License - see the [LICENSE](LICENSE) file for details.

## 🙋 Support

- **Issues**: [GitHub Issues](https://github.com/Open-ROD/openrod/issues)
- **Discussions**: [GitHub Discussions](https://github.com/Open-ROD/openrod/discussions)

## 🗺️ Roadmap

- [ ] Plugin system for custom memory processors
- [ ] Multi-user support with role-based access
- [ ] Integration with external knowledge bases
- [ ] Mobile app for memory management
- [ ] Advanced analytics and insights
- [ ] Cloud deployment options

## ✨ Acknowledgments

- Built on the [Model Context Protocol](https://modelcontextprotocol.io/)
- Vector database powered by QDrant
- Web UI built with Angular
- Community contributors and testers

---

**Made with ❤️ by the OpenROD community**