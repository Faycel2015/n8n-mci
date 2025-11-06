# n8n MCI Toolset - Quick Reference Guide

## What This Toolset Provides

This MCI toolset gives AI assistants (like Claude) direct access to n8n workflow documentation. Instead of relying on potentially outdated training data, AI can now fetch current documentation on-demand.

## Files Included

1. **n8n-mci-toolset.json** - Complete toolset with all 23 tools
2. **n8n-mci-config.json** - Enhanced version with better descriptions and caching
3. **n8n-mci-project-guide.md** - Full implementation guide

## Installation & Setup

### For Claude Desktop

1. Locate your Claude config file:
   - **macOS**: `~/Library/Application Support/Claude/claude_desktop_config.json`
   - **Windows**: `%APPDATA%\Claude\claude_desktop_config.json`

2. Add this configuration:

```json
{
  "mcpServers": {
    "n8n-docs": {
      "command": "uvx",
      "args": [
        "mci-py",
        "mcp",
        "--file",
        "/full/path/to/n8n-mci-config.json"
      ]
    }
  }
}
```

3. Restart Claude Desktop

4. Verify tools appear in the attachment button (📎)

### For VSCode / Cursor / Windsurf

Add to your workspace MCP configuration following each editor's specific format. See the main guide for details.

### Programmatic Use (Python)

```python
from mci import MCI

# Load the toolset
mci = MCI.from_file('n8n-mci-config.json')

# Get all tools
tools = mci.get_tools()

# Filter by tags
workflow_tools = mci.get_tools(tags=['workflows'])
trigger_tools = mci.get_tools(tags=['triggers'])
```

## Available Tools (23 Total)

### Fundamentals (2 tools)
- `n8n_workflow_basics` - Core workflow concepts
- `n8n_create_workflow` - Creating and running workflows

### Triggers (4 tools)
- `n8n_schedule_trigger` - Time-based automation
- `n8n_webhook_trigger` - HTTP request handling
- `n8n_manual_trigger` - Testing workflows
- `n8n_respond_webhook` - Custom webhook responses

### Data Processing (6 tools)
- `n8n_expressions` - Dynamic data manipulation
- `n8n_data_transformation` - Helper functions
- `n8n_code_node` - JavaScript/Python logic
- `n8n_data_structure` - Data format requirements
- `n8n_data_mapping` - Node output/input linking
- `n8n_transformation_nodes` - Split, Aggregate, Sort, etc.

### Error Handling (2 tools)
- `n8n_error_handling` - Error workflows & Try/Catch
- `n8n_debug_executions` - Troubleshooting failures

### Advanced Features (9 tools)
- `n8n_flow_logic` - If, Switch, Merge, Loop
- `n8n_http_request` - API calls
- `n8n_workflow_settings` - Configuration options
- `n8n_workflow_activation` - Going to production
- `n8n_ai_workflows` - AI Agent & chat models
- `n8n_quickstart` - Beginner tutorial
- `n8n_credentials` - Authentication management
- `n8n_workflow_templates` - Pre-built solutions

## Common Use Cases

### Use Case 1: "Build me a workflow that..."

**Example**: "Build me a workflow that sends me an email every Monday with new GitHub issues"

**AI will use**:
1. `n8n_create_workflow` - Learn workflow creation
2. `n8n_schedule_trigger` - Set up Monday schedule
3. `n8n_http_request` - Fetch GitHub issues
4. `n8n_workflow_activation` - Activate for automatic runs

### Use Case 2: "My workflow is failing..."

**Example**: "My workflow keeps failing with 'Cannot read property of undefined'"

**AI will use**:
1. `n8n_debug_executions` - Debug the failure
2. `n8n_error_handling` - Set up error handling
3. `n8n_expressions` - Check data access syntax
4. `n8n_data_structure` - Verify data format

### Use Case 3: "How do I transform data..."

**Example**: "How do I convert an array of objects to CSV format?"

**AI will use**:
1. `n8n_data_transformation` - Helper functions
2. `n8n_code_node` - Custom transformation
3. `n8n_transformation_nodes` - Built-in transform nodes

### Use Case 4: "Create an API endpoint..."

**Example**: "I need to create an API that accepts JSON and returns processed data"

**AI will use**:
1. `n8n_webhook_trigger` - Set up webhook
2. `n8n_respond_webhook` - Return custom response
3. `n8n_data_mapping` - Process incoming data
4. `n8n_credentials` - Add authentication

### Use Case 5: "Build an AI chatbot..."

**Example**: "Create a workflow that uses GPT-4 to answer customer questions"

**AI will use**:
1. `n8n_ai_workflows` - AI Agent setup
2. `n8n_webhook_trigger` - Receive questions
3. `n8n_respond_webhook` - Send AI responses
4. `n8n_credentials` - OpenAI authentication

## Tool Selection Logic

The AI automatically selects tools based on:

1. **Keywords in your question**:
   - "schedule" → `n8n_schedule_trigger`
   - "error", "failing" → `n8n_error_handling`
   - "webhook", "API" → `n8n_webhook_trigger`
   - "transform data" → `n8n_data_transformation`

2. **Task complexity**:
   - Simple questions → 1-2 tools
   - Complex workflows → 3-5 tools
   - Debugging → 2-4 tools

3. **Context awareness**:
   - Beginner → `n8n_quickstart`, `n8n_workflow_basics`
   - Advanced → `n8n_code_node`, `n8n_flow_logic`

## Tags for Filtering

You can ask AI to focus on specific areas:

```
- "Show me only trigger-related documentation"
  → Uses tags: ['triggers']

- "I need help with data processing"
  → Uses tags: ['data', 'transformation']

- "Help with production reliability"
  → Uses tags: ['errors', 'reliability', 'production']

- "Getting started resources"
  → Uses tags: ['getting-started', 'beginner', 'tutorial']
```

## Best Practices

### For Users:
1. **Be specific** about what you're building
2. **Mention error messages** when debugging
3. **Share context** about your workflow structure
4. **Ask follow-up questions** to go deeper

### For AI:
1. **Use multiple tools** for complex questions
2. **Start with basics** for beginners
3. **Check error handling** for production workflows
4. **Suggest templates** when available

## Limitations & Workarounds

### Current Limitations:
- URLs point to docs.n8n.io (may need parsing)
- No full-text search capability
- Documentation might be outdated between updates
- Some advanced features not covered

### Workarounds:
1. **Multiple tool calls** - Combine tools for complete picture
2. **Manual verification** - Check n8n docs for latest info
3. **Community resources** - n8n forum and GitHub
4. **Template library** - Use real workflow examples

## Troubleshooting

### Problem: Tools not appearing in Claude
**Solution**: 
- Verify config file path is absolute
- Check JSON syntax is valid
- Restart Claude Desktop completely
- Check logs in Claude's Developer Tools

### Problem: Wrong tool selected
**Solution**:
- Make your question more specific
- Mention exact n8n features by name
- Reference the tool directly: "Use n8n_schedule_trigger to explain..."

### Problem: Tool returns irrelevant info
**Solution**:
- Tools fetch full documentation pages
- AI extracts relevant sections
- Ask more specific follow-up questions
- Request examples from the documentation

## Updating the Toolset

When n8n releases new features:

1. Review n8n changelog
2. Identify new documentation pages
3. Add new tools to the JSON file
4. Update existing tool descriptions
5. Test with common questions
6. Update this guide

## Integration with Existing MCP Servers

This toolset works alongside:

- **n8n-mcp** (GitHub: czlonkowski/n8n-mcp) - More detailed node info
- **filesystem MCP** - Access local workflow files
- **web MCP** - Fetch live n8n documentation
- **Custom MCPs** - Your organization's n8n standards

Combine them in your config:

```json
{
  "mcpServers": {
    "n8n-docs": {
      "command": "uvx",
      "args": ["mci-py", "mcp", "--file", "/path/to/n8n-mci-config.json"]
    },
    "n8n-detailed": {
      "command": "npx",
      "args": ["-y", "n8n-mcp"]
    }
  }
}
```

## Performance Tips

1. **Caching** - Tools cache responses for 1 hour (configurable)
2. **Rate limiting** - Respects n8n docs rate limits
3. **Batch requests** - AI makes multiple tool calls efficiently
4. **Lazy loading** - Tools only called when needed

## Community & Support

- **n8n Documentation**: https://docs.n8n.io/
- **n8n Community Forum**: https://community.n8n.io/
- **n8n GitHub**: https://github.com/n8n-io/n8n
- **MCI Documentation**: https://usemci.dev/
- **This Toolset Issues**: [Create GitHub issue]

## Quick Command Reference

```bash
# Install MCI
pip install mci-py

# Validate toolset
mcix validate --file n8n-mci-config.json

# List all tools
mcix tools --file n8n-mci-config.json

# Test a specific tool
mcix test n8n_workflow_basics --file n8n-mci-config.json

# Run as MCP server
mcix run --file n8n-mci-config.json
```

## Example Prompts to Try

Once configured, try these with Claude:

```
1. "How do I create a workflow that runs every hour?"

2. "My webhook workflow returns 'Cannot read property json of undefined'. How do I fix this?"

3. "Show me how to transform data from an API response before sending to another service"

4. "Build a workflow that monitors a folder for new files and processes them"

5. "How do I add error handling so I get notified when my workflow fails?"

6. "Create an API endpoint that accepts POST requests with JSON data"

7. "What's the difference between Continue on Error and an Error Workflow?"

8. "How do I schedule a workflow to run on the first Monday of every month?"

9. "Build a chatbot using OpenAI's GPT-4"

10. "How do I debug a workflow that worked yesterday but fails today?"
```

## Version History

- **v1.0.0** (2025-11-06): Initial release with 23 tools
  - Core workflow documentation
  - Trigger nodes coverage
  - Data processing tools
  - Error handling guides
  - AI workflow support

## Next Steps

1. **Try it out** - Install and test with real questions
2. **Customize** - Add tools for your specific needs
3. **Share feedback** - Report issues or suggestions
4. **Contribute** - Improve descriptions and coverage
5. **Extend** - Build additional toolsets for your organization

---

**Remember**: This toolset makes AI smarter about n8n, but you're still the automation expert. Use AI as a knowledgeable assistant, not a replacement for understanding your workflows.

Happy automating! 🚀
