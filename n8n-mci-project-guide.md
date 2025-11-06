# Building n8n Documentation MCI Toolset Project

## Project Overview
This guide will walk you through creating an MCI (Model Context Interface) toolset that provides Claude and other AI assistants with access to n8n workflow documentation. The toolset wraps n8n documentation into tools that AI can use to help build and troubleshoot workflows.

## What You'll Build
A complete MCI toolset with 23 specialized tools covering:
- Workflow basics and components
- Trigger nodes (Manual, Schedule, Webhook)
- Error handling and debugging
- Data transformation and expressions
- Code nodes and custom logic
- Flow control and conditional logic
- HTTP requests and API integration
- AI workflow building
- And more...

## Prerequisites
- Basic understanding of JSON
- Familiarity with n8n (helpful but not required)
- Python 3.8+ (if running as MCP server)
- Claude Desktop or other MCP-compatible client (optional, for testing)

## Project Structure

```
your-project/
├── mci/
│   ├── n8n-workflows/         # Main toolset directory
│   │   ├── toolset.json       # Tool definitions
│   │   └── README.md          # Documentation
│   └── config.json            # MCI configuration
├── n8n-mci-toolset.json       # Complete toolset (what we created)
└── README.md                  # Project documentation
```

## Step 1: Understanding the MCI Schema

Based on the MCI schema reference at https://usemci.dev/documentation/schema-reference.md, here's what each tool needs:

```json
{
  "name": "tool_name",
  "annotations": {
    "title": "Human readable title",
    "readOnlyHint": true
  },
  "description": "Detailed description of what this tool provides",
  "execution": {
    "type": "http",
    "method": "GET",
    "url": "https://api-endpoint-url"
  },
  "tags": ["tag1", "tag2", "tag3"]
}
```

### Key Components:
- **name**: Unique identifier (snake_case recommended)
- **annotations.title**: Display name for the tool
- **annotations.readOnlyHint**: Indicates the tool only reads data
- **description**: Detailed explanation (AI uses this to decide when to call the tool)
- **execution**: How to fetch the data (HTTP, CLI, file, etc.)
- **tags**: Categorization for filtering and organization

## Step 2: Setting Up the Basic Structure

Create your main MCI configuration file (`mci.json`):

```json
{
  "version": "1.0.0",
  "metadata": {
    "name": "n8n Documentation Toolset",
    "description": "Comprehensive n8n workflow building documentation",
    "author": "Your Name",
    "homepage": "https://your-project-url"
  },
  "toolsets": [
    {
      "source": "./mci/n8n-workflows/toolset.json",
      "tags": ["n8n", "workflows", "documentation"]
    }
  ]
}
```

## Step 3: Creating Individual Tools

Each tool should map to a specific documentation area. Here's the pattern I used:

### Example 1: Workflow Basics Tool
```json
{
  "name": "n8n_workflow_basics",
  "annotations": {
    "title": "Docs: Workflow Basics",
    "readOnlyHint": true
  },
  "description": "Get fundamental information about n8n workflows including creation, activation, components (nodes, connections, sticky notes), and workflow templates. Learn what workflows are, how to create them, and basic workflow structure.",
  "execution": {
    "type": "http",
    "method": "GET",
    "url": "https://docs.n8n.io/workflows/"
  },
  "tags": ["n8n", "workflows", "basics", "getting-started"]
}
```

### Example 2: Schedule Trigger Tool
```json
{
  "name": "n8n_schedule_trigger",
  "annotations": {
    "title": "Docs: Schedule Trigger",
    "readOnlyHint": true
  },
  "description": "Detailed documentation on the Schedule Trigger node for running workflows at fixed intervals. Covers trigger intervals (seconds, minutes, hours, days, weeks, months), cron expressions, timezone configuration, and troubleshooting schedule timing issues. Essential for automated periodic workflows.",
  "execution": {
    "type": "http",
    "method": "GET",
    "url": "https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.scheduletrigger/"
  },
  "tags": ["n8n", "schedule", "cron", "automation", "timing"]
}
```

## Step 4: Tool Categories and Coverage

I've created 23 tools covering these areas:

### Core Workflow Concepts (5 tools)
1. **Workflow Basics** - Creation, structure, components
2. **Trigger Nodes** - All trigger types overview
3. **Workflow Execution** - Running and activating workflows
4. **Workflow Settings** - Configuration options
5. **Workflow Templates** - Pre-built solutions

### Specific Triggers (4 tools)
6. **Schedule Trigger** - Time-based automation
7. **Webhook Node** - HTTP request handling
8. **Respond to Webhook** - Custom responses
9. **Manual Trigger** - Testing workflows

### Error Handling (2 tools)
10. **Error Handling** - Error workflows, Try/Catch
11. **Execution Debugging** - Troubleshooting failures

### Data Processing (6 tools)
12. **Expressions** - Dynamic data manipulation
13. **Data Transformation** - Helper functions
14. **Code Node** - JavaScript/Python custom logic
15. **Data Structure** - Array of objects format
16. **Data Mapping** - Linking node outputs
17. **Transformation Nodes** - Split, Aggregate, Sort, etc.

### Advanced Features (6 tools)
18. **Flow Logic** - If, Switch, Merge, Loop
19. **HTTP Request** - API calls
20. **AI Workflows** - AI Agent, chat models
21. **Credentials** - Authentication management
22. **Node Parameters** - Configuration guide
23. **Workflow Sharing** - Collaboration features

## Step 5: Writing Effective Descriptions

The description is crucial - it tells AI when to use the tool. Follow this pattern:

```
[Primary purpose] including [key features]. 
Covers [specific topics], [more topics], and [even more topics]. 
[Why it's useful or when to use it].
```

**Good description:**
"Comprehensive guide to the Webhook node for receiving data from external services via HTTP requests. Covers webhook URLs, authentication methods (basic auth, header auth, IP whitelist), response modes (immediate, when last node finishes, using Respond to Webhook node, streaming), and building API endpoints."

**Bad description:**
"Webhook documentation."

## Step 6: Choosing the Right Tags

Tags help with filtering and discovery. Use multiple categories:

1. **Platform tags**: `n8n`, `mci`, `documentation`
2. **Feature tags**: `webhook`, `schedule`, `error`, `ai`
3. **Concept tags**: `triggers`, `data`, `flow`, `security`
4. **Level tags**: `beginner`, `advanced`, `getting-started`
5. **Action tags**: `debugging`, `transformation`, `automation`

## Step 7: URL Selection Strategy

For the execution URL, I used the actual n8n documentation URLs. However, you could also:

1. **Use the main docs API** (if available)
2. **Create a proxy service** that fetches and formats content
3. **Point to specific documentation pages** (what I did)
4. **Use a custom API** you build to aggregate content

In my implementation, I used:
```json
"execution": {
  "type": "http",
  "method": "GET",
  "url": "https://docs.n8n.io/api/api-reference/"
}
```

This is a placeholder. In a production system, you'd want to either:
- Use n8n's actual API endpoints (if they exist)
- Build a scraper to fetch documentation
- Use the MCP server approach (like n8n-mcp on GitHub)

## Step 8: Complete Toolset Structure

Your final `toolset.json` should look like:

```json
{
  "mcpServers": {
    "n8n-workflows": {
      "command": "mcp",
      "args": ["serve", "n8n-workflows"],
      "metadata": {
        "name": "n8n Workflow Documentation",
        "description": "Access comprehensive documentation for building and managing n8n workflows",
        "version": "1.0.0"
      },
      "tools": [
        // All 23 tool definitions here
      ]
    }
  }
}
```

## Step 9: Testing Your Toolset

### Option 1: Use Claude Desktop

1. Add to your Claude Desktop config (`claude_desktop_config.json`):

```json
{
  "mcpServers": {
    "n8n-docs": {
      "command": "uvx",
      "args": ["mci-py", "mcp", "--file", "/path/to/n8n-mci-toolset.json"]
    }
  }
}
```

2. Restart Claude Desktop
3. Check if tools appear in the tools menu
4. Test with prompts like:
   - "How do I create a schedule trigger in n8n?"
   - "Explain error handling in n8n workflows"
   - "Show me how to use expressions in n8n"

### Option 2: Use the CLI

```bash
# Install MCI
pip install mci-py

# Run the toolset
mcix run --file ./n8n-mci-toolset.json

# Or use programmatically in Python
from mci import MCI

mci = MCI.from_file('n8n-mci-toolset.json')
tools = mci.get_tools()
```

## Step 10: Enhancements and Next Steps

### Immediate Improvements:
1. **Fix the execution URLs** - Point to actual working endpoints
2. **Add response parsing** - Transform HTML docs to structured data
3. **Add caching** - Store documentation locally
4. **Add filtering** - Allow tag-based tool filtering

### Advanced Features:
1. **Multi-source aggregation** - Combine official docs + community content
2. **Search functionality** - Full-text search across all docs
3. **Example extraction** - Pull code examples from documentation
4. **Interactive tutorials** - Step-by-step workflow builders
5. **Version-specific docs** - Support different n8n versions

### Production Readiness:
1. **Error handling** - Gracefully handle failed requests
2. **Rate limiting** - Respect documentation site limits
3. **Authentication** - If docs require auth
4. **Monitoring** - Track tool usage and performance
5. **Documentation** - Write clear usage guides

## Example Usage Scenarios

### Scenario 1: Building a Scheduled Email Report

**User asks:** "I want to create a workflow that sends me an email every Monday morning with data from an API"

**AI uses these tools:**
1. `n8n_schedule_trigger` - Learn how to set up Monday schedule
2. `n8n_http_request` - Understand API calls
3. `n8n_workflow_basics` - Overall workflow structure
4. `n8n_workflow_execution` - How to activate the workflow

### Scenario 2: Debugging a Failed Workflow

**User asks:** "My workflow keeps failing and I don't know why"

**AI uses these tools:**
1. `n8n_execution_debugging` - Debug failed executions
2. `n8n_error_handling` - Set up error workflows
3. `n8n_workflow_execution` - Check execution logs

### Scenario 3: Processing Complex Data

**User asks:** "I need to transform JSON data from an API into a different format"

**AI uses these tools:**
1. `n8n_expressions` - Dynamic data manipulation
2. `n8n_data_transformation` - Helper functions
3. `n8n_code_node` - Write custom transformation logic
4. `n8n_transformation_nodes` - Use built-in transform nodes

## Best Practices

### 1. Tool Granularity
- Don't make tools too broad (one tool for all of n8n)
- Don't make them too narrow (one tool per parameter)
- Aim for logical groupings (one tool per major concept)

### 2. Description Quality
- Be specific about what the tool covers
- Include key terms AI might search for
- Explain when to use this vs other tools
- Use natural language, not technical jargon

### 3. Maintenance
- Update when n8n releases new features
- Test tools periodically
- Monitor which tools are most used
- Remove or merge underused tools

### 4. Performance
- Cache documentation content
- Use efficient data formats
- Minimize unnecessary tool calls
- Consider rate limits

## Troubleshooting

### Tool Not Appearing
- Check JSON syntax (use validator)
- Verify file paths are correct
- Ensure MCI is properly installed
- Check Claude Desktop logs

### Tool Returns No Data
- Verify URL is accessible
- Check if authentication needed
- Test URL in browser first
- Look at error messages

### Wrong Tool Selected
- Improve description specificity
- Add more relevant tags
- Consider splitting into multiple tools
- Test with different prompts

## Resources

- **MCI Documentation**: https://usemci.dev/
- **n8n Documentation**: https://docs.n8n.io/
- **n8n MCP Server**: https://github.com/czlonkowski/n8n-mcp
- **MCP Protocol**: https://modelcontextprotocol.io/

## Conclusion

You now have a complete MCI toolset for n8n documentation! This setup gives AI assistants comprehensive knowledge about n8n workflows, enabling them to help users build, debug, and optimize their automation workflows.

The key is that AI can now "read" n8n documentation on-demand, rather than relying on training data that might be outdated or incomplete.

## Next Steps

1. Test the toolset with real workflows
2. Gather feedback on missing documentation areas
3. Add more specialized tools for advanced features
4. Share your toolset with the community
5. Consider contributing back to n8n documentation

Happy automating!
