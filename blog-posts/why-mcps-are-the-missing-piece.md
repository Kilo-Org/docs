# Why MCPs Are the Missing Piece Your AI Assistant Needs
### And How They Actually Work (Spoiler: It's Not What You Think)

You know that moment when you ask Claude to "search for the latest React documentation" and it confidently starts typing out what looks like search results? Here's the thing that'll blow your mind: **Claude isn't actually searching anything**. Neither is ChatGPT when it "browses the web" or Kilo Code when it "edits your files."

Plot twist: LLMs don't use APIs. They can't. They literally don't know how.

## The Great AI Illusion

Let me ask you a question that sounds obvious but isn't:

```
Question: How do LLMs use APIs? 
Answer: They don't!
Question: ...How do we make them do it?
Answer: We give them tools!
```

When you see Kilo Code editing your files, it's not Sonnet directly manipulating your filesystem. When Claude "searches" something, it's not Opus making HTTP requests. The AI models are doing what they do best: **writing formatted text commands**. Then the *agent* (Kilo Code, Claude's interface, etc.) reads those commands and actually executes the tools.

It's like having a really smart person who can only communicate through written notes, and you're the one with hands who actually does the work. The AI writes "please create a file called `app.js` with this content," and Kilo Code goes "got it" and creates the file.

## How This Actually Works Under the Hood

Here's where it gets interesting. Every time you send a message to an AI model, the request contains two parts:

1. **Your input** - what you actually typed
2. **System prompt** - the behind-the-scenes instructions (including tool definitions)

Those tool definitions are crucial. They're written in a specific XML format that tells the model "hey, if you need to write to a file, format your response like this: `<write_to_file><path>...</path><content>...</content></write_to_file>`"

Kilo Code ships with about 17 native tools - [`write_to_file()`](https://kilocode.ai/docs/features/tools/write-to-file), [`execute_command()`](https://kilocode.ai/docs/basic-usage/how-tools-work), [`browser_action()`](https://kilocode.ai/docs/features/browser-use), and more. Each one is carefully defined in the system prompt so the AI knows exactly how to "ask" for what it needs.

But here's the problem: there are millions of APIs out there. GitHub's API alone has hundreds of endpoints. Slack, Notion, databases, custom internal tools - the list is endless. We can't build native tools for everything.

## Enter MCP: The Universal Tool Protocol

This is where Model Context Protocol (MCP) comes in. Published by Anthropic in November 2024, MCP is essentially a standardized way to give AI models access to external tools without hardcoding them into every AI assistant.

Think of it like this: instead of Kilo Code needing to know about every possible API, MCP servers act as translators. Each MCP server says "hey, I can handle GitHub operations" or "I've got Slack covered" and provides the tool definitions the AI needs.

An MCP server does two main things:
1. **Tells the AI what tools are available** - "I have tools for creating issues, reading repositories, managing pull requests"
2. **Executes tools when requested** - Actually makes the API calls when the AI asks

Meanwhile, the AI agent (Kilo Code) does its part:
1. **Collects available MCP tools** and adds them to the system prompt
2. **Routes tool requests** - either uses native tools directly or calls the appropriate MCP server

## Real Examples That Actually Matter

Let's look at some MCP servers that are genuinely useful:

**GitHub MCP Server** provides over 65 different tools. Instead of the AI saying "I can't access your GitHub," it can now create issues, review pull requests, check repository stats, and manage releases. All through standardized tool calls.

**Context7** might only provide two tools, but don't underestimate it. It maintains up-to-date documentation for thousands of software projects. When you're working with a library that just released version 4 but the AI was trained on version 3 docs, Context7 bridges that gap with current information.

**Database MCP servers** let you query your production database directly from your AI assistant. "Show me all users who signed up this week" becomes a simple request instead of a manual SQL session.

## The Reality Check: It's Not All Sunshine

MCPs are powerful, but they come with real costs - and I mean that literally:

**Token overhead**: Every MCP tool definition gets added to your prompt. More tools = more tokens = higher costs.

**Latency**: Tool execution adds delays. Network calls to external APIs aren't instant.

**Complexity**: More moving parts mean more potential failure points. MCP servers can crash, APIs can be down, authentication can fail.

**Context window consumption**: Tool responses eat into your available context. A large API response might push important context out of the window.

**Cost multiplication**: You're not just paying for the AI's thinking - you're paying for all those tool definitions in every single request.

This is why you shouldn't just install every MCP server you find. Be strategic:
- Turn off servers you're not actively using
- Disable specific tools within servers that you don't need
- Use project-specific configurations instead of global installs when possible
- Monitor your token usage and costs

## Getting Started Without the Overwhelm

Ready to try MCP? Here's the practical approach:

1. **Start small** - Pick one specific problem you want to solve (like GitHub integration)
2. **Install locally first** - Use project-specific `.kilocode/mcp.json` files instead of global configuration
3. **Test thoroughly** - Make sure the server works reliably before depending on it
4. **Monitor costs** - Keep an eye on how MCP usage affects your API bills

<img src="https://kilocode.ai/docs/img/using-mcp-in-kilo-code/mcp-installed-config.png" alt="MCP server configuration in Kilo Code" width="600" />

The setup is straightforward in Kilo Code. Head to `Settings → MCP Servers → Installed` and you can configure both global and project-specific servers. The interface makes it easy to enable/disable servers and individual tools as needed.

## Why This Changes Everything

MCP isn't just another integration method - it's a fundamental shift in how AI assistants can interact with the world. Instead of being limited to a fixed set of capabilities, your AI can now adapt to your specific workflow and tools, even proprietary.

The protocol is still young (remember, it was only published in November 2024), but the potential is massive. We're moving from "AI assistants that can do some predefined tasks" to "AI assistants that can learn to use any tool you need."

## The Bottom Line

MCPs solve a real problem: the gap between what AI models can theoretically do and what they can actually access. They're not perfect - they add complexity and cost - but they're the missing piece that makes AI assistants genuinely useful for specialized workflows.

Don't rush to install every MCP server you can find. Start with one that solves a specific problem you have, learn how it works, and expand from there. Your future self (and your API bill) will thank you.

The era of AI assistants that can only work with built-in tools is ending. MCPs are how we get to AI that works with *your* tools.

---

*Want to dive deeper into MCP? Check out our [comprehensive MCP documentation](https://kilocode.ai/docs/features/mcp/overview) for setup guides and examples. Got questions? Join our [Discord community](https://kilo.love/discord) where developers share their favorite MCP servers and configurations.*