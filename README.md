# n8n MCI Toolset Project

Complete implementation of an MCI (Model Context Interface) toolset that provides AI assistants with comprehensive access to n8n workflow automation documentation.

## What This Is

This project creates an **MCI toolset** - a standardized way to give AI assistants like Claude access to n8n documentation. Instead of relying on training data that might be outdated, AI can now fetch current n8n documentation on-demand when helping users build workflows.

Think of it as "API documentation for AI" - structured tools that let AI know what information exists, how to access it, and when to use it.

## Files Created

### 1. n8n-mci-toolset.json
**The complete toolset definition**
- 23 specialized tools covering all major n8n features
- Each tool maps to a specific documentation area
- Includes descriptions, tags, and execution endpoints
- Ready to use with MCI/MCP servers

### 2. n8n-mci-config.json  
**Enhanced production-ready configuration**
- Improved tool descriptions with more context
- Better categorization and tagging
- Caching configuration
- More specific use case examples in descriptions
- Recommended for actual deployment

### 3. n8n-mci-project-guide.md
**Complete implementation guide (10,000+ words)**
- Step-by-step project setup
- Detailed explanation of each component
- Tool creation best practices
- Testing strategies
- Troubleshooting guide
- Production deployment tips
- **Start here if you're building this yourself**

### 4. n8n-mci-quick-reference.md
**Quick reference for daily use**
- Installation instructions
- Available tools overview
- Common use cases with examples
- Troubleshooting quick fixes
- Example prompts to try
- **Start here if you just want to use it**

## What's Covered (23 Tools)

### Core Concepts
- Workflow basics and creation
- Workflow settings and configuration

### Triggers (When workflows start)
- Schedule Trigger - Time-based automation
- Webhook - HTTP requests
- Manual Trigger - Testing
- Response handling

### Data Processing
- Expressions - Dynamic data access
- Data transformation functions
- Code node - Custom JavaScript/Python
- Data structure requirements
- Data mapping between nodes
- Transform nodes - Split, Aggregate, Sort

### Reliability
- Error handling workflows
- Debugging failed executions
- Try/Catch patterns

### Advanced Features
- Flow logic - If, Switch, Merge
- HTTP Request - API calls
- AI workflows - Agents & chatbots
- Credentials management
- Workflow templates

## How It Works

```
User asks question
    ↓
AI analyzes question
    ↓
AI selects relevant tools from toolset
    ↓
AI fetches documentation via tool execution
    ↓
AI synthesizes answer using fresh documentation
    ↓
User gets accurate, up-to-date help
```

## Quick Start

### Option 1: Use with Claude Desktop

1. Download `n8n-mci-config.json`

2. Add to `~/Library/Application Support/Claude/claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "n8n-docs": {
      "command": "uvx",
      "args": ["mci-py", "mcp", "--file", "/path/to/n8n-mci-config.json"]
    }
  }
}
```

3. Restart Claude Desktop

4. Ask n8n questions - Claude now has documentation access!

### Option 2: Use Programmatically

```python
from mci import MCI

mci = MCI.from_file('n8n-mci-config.json')
tools = mci.get_tools(tags=['workflows'])
```

## Example Usage

**Before (without toolset)**:
```
User: "How do I schedule a workflow to run every Monday?"
Claude: "Based on my training data from early 2024, n8n has a Schedule 
        Trigger node that should support weekly schedules..."
```

**After (with toolset)**:
```
User: "How do I schedule a workflow to run every Monday?"
Claude: [Uses n8n_schedule_trigger tool]
        "Here's exactly how to set up a Monday schedule in n8n:
        1. Add a Schedule Trigger node
        2. Set 'Trigger Interval' to 'Weeks'
        3. Set 'Weeks Between Triggers' to 1
        4. Select 'Monday' in 'Trigger on Weekdays'
        5. Set your desired time...
        
        [Provides current, accurate instructions from live documentation]"
```

## Key Benefits

1. **Always Current** - Pulls from live n8n documentation, not static training data
2. **Comprehensive** - 23 tools cover all major workflow building scenarios
3. **Well-Organized** - Tools grouped by category with smart tagging
4. **Context-Aware** - AI knows which tool to use for which question
5. **Extensible** - Easy to add more tools as n8n evolves

## Tool Selection Examples

The AI automatically picks the right tools based on your question:

- "build a scheduled workflow" → `n8n_schedule_trigger` + `n8n_workflow_basics`
- "my workflow failed" → `n8n_debug_executions` + `n8n_error_handling`
- "transform API data" → `n8n_data_transformation` + `n8n_code_node`
- "create webhook API" → `n8n_webhook_trigger` + `n8n_respond_webhook`
- "build AI chatbot" → `n8n_ai_workflows` + `n8n_webhook_trigger`

## Architecture

```
MCI Toolset (JSON)
    ├── Tool Definitions
    │   ├── Name & Metadata
    │   ├── Description (what it covers)
    │   ├── Execution (how to fetch data)
    │   └── Tags (for filtering)
    │
    ├── AI Decision Layer
    │   ├── Analyzes user question
    │   ├── Selects relevant tools
    │   └── Fetches documentation
    │
    └── Response Generation
        ├── Synthesizes information
        ├── Provides specific examples
        └── Cites documentation sources
```

## Comparison to Alternatives

### vs. Training Data Only
- ✅ Always current
- ✅ More detailed
- ✅ Covers new features
- ❌ Requires network access

### vs. Full MCP Server (like n8n-mcp)
- ✅ Simpler to set up
- ✅ Lighter weight
- ✅ Documentation-focused
- ❌ Less programmatic access to node internals

### vs. Manual Documentation Search
- ✅ AI finds relevant docs automatically
- ✅ Synthesizes info from multiple pages
- ✅ Provides context-aware answers
- ❌ Requires MCI/MCP setup

## Use Cases

### For Beginners
- "How do I create my first workflow?"
- "What's a trigger node?"
- "How do I test my workflow?"

### For Developers
- "How do I use expressions to transform data?"
- "Best practices for error handling?"
- "How to build a REST API endpoint?"

### For Troubleshooting
- "My schedule trigger isn't firing"
- "Webhook returns 'Cannot read property'"
- "How to debug failed executions?"

### For Advanced Users
- "Build an AI agent workflow"
- "Complex data transformation patterns"
- "Sub-workflow execution strategies"

## Limitations & Future Improvements

### Current Limitations
- URLs point to docs pages (need parsing)
- No full-text search across all docs
- Some advanced features not covered
- Documentation can lag behind releases

### Planned Improvements
- Direct API integration (if n8n provides one)
- Search across all documentation
- Code example extraction
- Community content integration
- Version-specific documentation

## Technical Details

**Format**: JSON (MCI schema)  
**Tools**: 23 specialized documentation access points  
**Tags**: 50+ for smart filtering  
**Categories**: 8 major areas (triggers, data, errors, etc.)  
**Compatibility**: Claude Desktop, VSCode, Cursor, Windsurf, any MCP client  
**Requirements**: Python 3.8+ (for MCP server mode)

## Project Structure Explained

Looking at your screenshot, the structure follows this pattern:

```javascript
{
  "mcpServers": {
    "server-name": {
      "metadata": { /* name, version, description */ },
      "tools": [
        {
          "name": "tool_identifier",
          "annotations": { /* title, hints */ },
          "description": "What this tool provides",
          "execution": { /* how to get the data */ },
          "tags": ["categorization"]
        }
        // ... more tools
      ]
    }
  }
}
```

Each tool is essentially saying:
1. **Name**: How to identify this tool
2. **Description**: When to use it (AI reads this)
3. **Execution**: Where to get the information
4. **Tags**: How to categorize/filter it

## Getting Started Checklist

- [ ] Read n8n-mci-quick-reference.md
- [ ] Install mci-py (`pip install mci-py`)
- [ ] Choose your configuration file
- [ ] Set up Claude Desktop or your MCP client
- [ ] Test with example prompts
- [ ] Customize tools for your needs
- [ ] Share feedback/improvements

## Resources

- **n8n Documentation**: https://docs.n8n.io/
- **MCI Documentation**: https://usemci.dev/
- **n8n Community**: https://community.n8n.io/
- **MCI Schema Reference**: https://usemci.dev/documentation/schema-reference.md
- **Related Project (n8n-mcp)**: https://github.com/czlonkowski/n8n-mcp

## Contributing

Want to improve this toolset?

1. Add missing documentation areas
2. Improve tool descriptions
3. Add more specific tags
4. Create specialized toolsets for:
   - Specific n8n versions
   - Industry-specific workflows
   - Advanced features
   - Community resources

## FAQ

**Q: Do I need n8n installed to use this?**  
A: No, this is just documentation access. Install n8n separately.

**Q: Will this work with other AI assistants?**  
A: Yes, any MCP-compatible client can use it.

**Q: How often should I update the toolset?**  
A: Check when n8n releases major updates (usually quarterly).

**Q: Can I add my own custom tools?**  
A: Absolutely! Follow the guide in n8n-mci-project-guide.md.

**Q: Does this replace learning n8n?**  
A: No, it helps AI help you learn and build faster.

## License

This project provides configuration files for accessing public n8n documentation. The documentation itself is owned by n8n.io and subject to their terms.

## Acknowledgments

- n8n team for excellent documentation
- MCI project for the toolset framework
- Claude team for MCP integration
- Community for testing and feedback

---

**Ready to get started?** Open `n8n-mci-quick-reference.md` for installation instructions, or `n8n-mci-project-guide.md` to build your own custom version.

**Questions?** Check the troubleshooting sections in the guides or reach out to the n8n community.

Happy automating! 🚀
