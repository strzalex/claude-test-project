# Setting up Brave Search MCP Server for Claude Desktop

This guide covers the setup and debugging process for integrating the Brave Search MCP server with Claude Desktop.

## Prerequisites
- Node.js installed (check with `node --version`)
- Brave Search API key
- Claude Desktop application

## Setup Steps

1. Create/edit Claude Desktop config file:
```bash
# macOS
~/Library/Application Support/Claude/claude_desktop_config.json
# Windows
%APPDATA%\Claude\claude_desktop_config.json
```

2. Add Brave Search configuration:
```json
{
  "darkMode": "dark",
  "scale": 0,
  "mcpServers": {
    "brave-search": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-brave-search"
      ],
      "env": {
        "BRAVE_API_KEY": "YOUR_API_KEY_HERE"
      }
    }
  }
}
```

3. Install MCP server package:
```bash
npm install -g @modelcontextprotocol/server-brave-search
```

4. Start MCP server:
```bash
BRAVE_API_KEY=your_api_key npx @modelcontextprotocol/server-brave-search
```

You should see: "Brave Search MCP Server running on stdio"

5. Restart Claude Desktop

## Common Issues and Solutions

### 1. EACCES Permission Error
If you get permission errors during npm installation:
```bash
sudo npm install -g @modelcontextprotocol/server-brave-search
```

### 2. Config File JSON Syntax
- Ensure valid JSON format
- Use straight quotes (`"`) not curly quotes (`"`)
- Check for trailing commas

### 3. Server Not Responding
- Verify server is running ("Brave Search MCP Server running on stdio")
- Check API key is correctly set
- Restart both server and Claude Desktop

### 4. Integration Testing
Test the connection with a simple search query in Claude:
```json
{
  "tool_name": "brave_web_search",
  "tool_args": {
    "query": "test",
    "count": 1
  }
}
```

## Debugging Tips

1. Check Node.js installation:
```bash
node --version
npm --version
```

2. Verify package installation:
```bash
npm list -g @modelcontextprotocol/server-brave-search
```

3. Monitor server output:
```bash
DEBUG=* BRAVE_API_KEY=your_api_key npx @modelcontextprotocol/server-brave-search
```

4. Check active ports:
```bash
# macOS
lsof -i -P | grep LISTEN
# Windows
netstat -tulpn | grep LISTEN
```

## File Locations
- Config file (macOS): `~/Library/Application Support/Claude/claude_desktop_config.json`
- Config file (Windows): `%APPDATA%\Claude\claude_desktop_config.json`

Remember to always restart Claude Desktop after making configuration changes.