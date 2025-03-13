# Serper Search and Scrape MCP Server
[![smithery badge](https://smithery.ai/badge/@marcopesani/mcp-server-serper)](https://smithery.ai/server/@marcopesani/mcp-server-serper)

A TypeScript-based MCP server that provides web search and webpage scraping capabilities using the Serper API. This server integrates with Claude Desktop to enable powerful web search and content extraction features.

<a href="https://glama.ai/mcp/servers/5zk327i0pj">
  <img width="380" height="200" src="https://glama.ai/mcp/servers/5zk327i0pj/badge" alt="serper-search-scrape-mcp-server MCP server" />
</a>

## Features

### Tools

- `google_search` - Perform web searches via Serper API
  - Rich search results including organic results, knowledge graph, "people also ask", and related searches
  - Supports region and language targeting
  - Optional parameters for location, pagination, time filters, and autocorrection
  - Supports advanced search operators:
    - `site`: Limit results to specific domain
    - `filetype`: Limit to specific file types (e.g., 'pdf', 'doc')
    - `inurl`: Search for pages with word in URL
    - `intitle`: Search for pages with word in title
    - `related`: Find similar websites
    - `cache`: View Google's cached version of a specific URL
    - `before`: Date before in YYYY-MM-DD format
    - `after`: Date after in YYYY-MM-DD format
    - `exact`: Exact phrase match
    - `exclude`: Terms to exclude from search results
    - `or`: Alternative terms (OR operator)
  
- `scrape` - Extract content from web pages
  - Get plain text and optional markdown content
  - Includes JSON-LD and head metadata
  - Preserves document structure

## Requirements

- Node.js >= 18
- Serper API key (set as `SERPER_API_KEY` environment variable)

## Development

Install dependencies:
```bash
npm install
```

Build the server:
```bash
npm run build
```

For development with auto-rebuild:
```bash
npm run watch
```

Run tests:
```bash
npm test                  # Run all tests
npm run test:watch       # Run tests in watch mode
npm run test:coverage    # Run tests with coverage
npm run test:integration # Run integration tests
```

### Environment Variables

Create a `.env` file in the root directory:

```
SERPER_API_KEY=your_api_key_here
```

### Debugging

Since MCP servers communicate over stdio, debugging can be challenging. We recommend using the [MCP Inspector](https://github.com/modelcontextprotocol/inspector), which is available as a package script:

```bash
npm run inspector
```

The Inspector will provide a URL to access debugging tools in your browser.

## Installation

### Installing via Smithery

To install Serper Search and Scrape for Claude Desktop automatically via [Smithery](https://smithery.ai/server/@marcopesani/mcp-server-serper):

```bash
npx -y @smithery/cli install @marcopesani/mcp-server-serper --client claude
```

### Claude Desktop

Add the server config at:
- MacOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%/Claude/claude_desktop_config.json`

```json
{
  "mcpServers": {
    "serper-search": {
      "command": "npx",
      "args": ["-y", "serper-search-scrape-mcp-server"],
      "env": {
        "SERPER_API_KEY": "your_api_key_here"
      }
    }
  }
}
```

### Cline

1. Open the Cline extension settings
2. Open "MCP Servers" tab
3. Click on "Configure MCP Servers"
4. Add the server config:

```json
{
  "mcpServers": {
    "github.com/marcopesani/mcp-server-serper": {
      "command": "npx",
      "args": ["-y", "serper-search-scrape-mcp-server"],
      "env": {
        "SERPER_API_KEY": "your_api_key_here"
      },
      "disabled": false,
      "autoApprove": ["google_search", "scrape"]
    }
  }
}
```

Additional Cline configuration options:
- `disabled`: Set to `false` to enable the server
- `autoApprove`: List of tools that don't require explicit approval for each use

### Cursor

1. Open the Cursor settings
2. Open "Features" settings
3. In the "MCP Servers" section, click on "Add new MCP Server"
4. Choose a name, and select "command" as "Type"
5. In the "Command" field, enter the following:

```
env SERPER_API_KEY=your_api_key_here npx -y serper-search-scrape-mcp-server
```

### Docker

You can also run the server using Docker. First, build the image:

```bash
docker build -t mcp-server-serper .
```

Then run the container with your Serper API key:

```bash
docker run -e SERPER_API_KEY=your_api_key_here mcp-server-serper
```

Alternatively, if you have your environment variables in a `.env` file:

```bash
docker run --env-file .env mcp-server-serper
```

For development, you might want to mount your source code as a volume:

```bash
docker run -v $(pwd):/app --env-file .env mcp-server-serper
```

Note: Make sure to replace `your_api_key_here` with your actual Serper API key.