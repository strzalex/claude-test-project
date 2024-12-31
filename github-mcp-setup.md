# Setting Up MCP for GitHub: A Troubleshooting Guide

This repository documents the process of setting up the Model Context Protocol (MCP) with GitHub integration in Claude Desktop, including all encountered challenges and their solutions.

## Initial Setup

1. Create a configuration file for Claude Desktop:
   ```bash
   # On macOS
   code ~/Library/Application\ Support/Claude/claude_desktop_config.json
   # On Windows
   code %APPDATA%\Claude\claude_desktop_config.json
   ```

2. Initial configuration attempt:
   ```json
   {
     "mcpServers": {
       "github": {
         "command": "npx",
         "args": [
           "-y",
           "@modelcontextprotocol/server-github"
         ],
         "env": {
           "GITHUB_PERSONAL_ACCESS_TOKEN": "your_token_here"
         }
       }
     }
   }
   ```

## Challenges and Solutions

### 1. GitHub Personal Access Token
**Challenge**: Server required GitHub Personal Access Token
**Solution**: 
- Create token at GitHub Settings > Developer settings > Personal access tokens
- Add token to configuration's env section
- Keep token secure and never share publicly

### 2. npm Permission Issues
**Challenge**: Error installing global npm package
```
npm error Error: EACCES: permission denied
```

**Solution**: Set up npm global directory in user space
```bash
mkdir ~/.npm-global
npm config set prefix '~/.npm-global'
export PATH=~/.npm-global/bin:$PATH
```

### 3. Node.js Path Issues
**Challenge**: `env: node: No such file or directory` error
**Solution**: Use full path to Node.js in configuration
```json
{
  "mcpServers": {
    "github": {
      "command": "/usr/local/bin/node",
      "args": [
        "/usr/local/lib/node_modules/npm/bin/npx-cli.js",
        "@modelcontextprotocol/server-github"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your_token_here",
        "PATH": "/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
      }
    }
  }
}
```

## Debugging Tips

1. Check MCP logs:
   ```bash
   tail -f ~/Library/Logs/Claude/mcp*.log
   ```

2. Test server manually:
   ```bash
   GITHUB_PERSONAL_ACCESS_TOKEN=your_token npx -y @modelcontextprotocol/server-github
   ```

3. Look for server-specific logs:
   ```bash
   cat ~/Library/Logs/Claude/mcp-server-github.log
   ```

## Final Working Configuration

```json
{
  "mcpServers": {
    "github": {
      "command": "/usr/local/bin/node",
      "args": [
        "/usr/local/lib/node_modules/npm/bin/npx-cli.js",
        "@modelcontextprotocol/server-github"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "your_token_here",
        "PATH": "/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin"
      }
    }
  }
}
```

## Verification

To verify successful setup:
1. Check for the hammer icon (🔨) in Claude Desktop
2. Click the hammer icon to view available GitHub tools
3. Test basic GitHub operations through Claude

## Additional Resources

- [MCP Documentation](https://modelcontextprotocol.io)
- [GitHub MCP Server Documentation](https://github.com/modelcontextprotocol/servers/tree/main/src/github)
- [Claude Desktop Documentation](https://claude.ai/docs)

## Contributing

Feel free to contribute to this documentation by submitting pull requests or creating issues if you encounter additional challenges or have alternative solutions.
