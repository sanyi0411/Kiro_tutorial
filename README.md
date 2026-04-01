# Kiro_tutorial

As a software developer, I will focus on the CLI version of Kiro

## Links

- https://kiro.dev/
- https://kiro.dev/docs/cli/

Install Kiro as discussed in the official tutorial

## About Kiro CLI

- Kiro is an agentic tool for developers
- There are 3 forms of Kiro: 
    - a standalone IDE (based on VSCode)
    - a VSCode extension
    - a command line tool _(this is what I focus on here)_

## Command types

- Commands that start with `kiro-cli` supposed to be run in your terminal but outside of kiro cli
```bash
$ kiro-cli --version
kiro-cli 1.28.0
```
- Commands that start with **/** should be run in a Kiro CLI chat session
```bash
$ /help
Switching to the [help] agent.
Welcome to Kiro CLI Help!
```

## Diagnostic

- Check details of installation, generate system information report

```bash
kiro-cli diagnostic
kiro-cli diagnostic --format <plain, json, json-pretty>
kiro-cli diagnostic --verbose
```

## Settings

Kiro has 2 types of settings:
- **global** configuration settings and
- project specific or **workspace** settings used in the active session
```
+-------------------------------------------------------------------+
|             Global             |             Workspace            |
+--------------------------------+----------------------------------+
|        ~/.kiro/settings        |    /workspace/.kiro/settings     |
+-------------------------------------------------------------------+
```
> **_IMPORTANT_**:  Workspace settings override global settings!

### Usage

```bash
kiro-cli settings <COMMAND>
kiro-cli settings [OPTIONS] [KEY] [VALUE]
```
> **_IMPORTANT_**:  You cannot alter settings from within Kiro CLI

### Commands

#### Open settings file in editor
```bash
kiro-cli settings open
```

#### List all configured settings
```bash
kiro-cli settings list
```

#### Show help message
```bash
kiro-cli settings help
```

### Show and set a value

- A setting consists of a key and value

> **_IMPORTANT_**:  By default a setting is set in the global scope, unless the `--workspace` option is used (see below, in Options)

#### Show a specific setting
```bash
kiro-cli settings chat.greeting.enabled
```

#### Set the given setting
```bash
kiro-cli settings chat.greeting.enabled true
```
### Options
#### List all available settings (with description)
```bash
kiro-cli settings list --all
```

#### Delete a setting
```bash
kiro-cli settings --delete chat.defaultModel
```

#### Set a value in the global settings (this is the default)
```bash
kiro-cli settings --global chat.enableNotifications true
```

#### Set a value in the workspace settings
```bash
kiro-cli settings --workspace chat.enableNotifications true
```

#### Show settings list in formatted JSON
```bash
kiro-cli settings list --format json-pretty
```

#### Show help message
```bash
kiro-cli settings --help
```
- List of possible settings:
    - https://kiro.dev/docs/cli/reference/settings/


## Launching Kiro CLI

#### Standard lauching Kiro CLI:
- Below two commands are **equivalent**!
```bash
kiro-cli
kiro-cli chat
```
- See more about `chat` sessions below

#### Launching Kiro CLI with a setting:
```bash
kiro-cli --untrust-fs-read
```
- We will later see what this untrust setting does

## Chat
- You can have different chat sessions with Kiro CLI
- Each chat session starts fresh, with no memory of previous sessions
- Starting a new session is like talking to a fresh instance of the assistant 
    - Knows your project structure and conventions but has no memory of past conversations
- Each chat session has it's own isolated context
    - Messages, decisions, and reasoning don't carry over from one session to another
- Chat sessions are automatically saved on every conversation turn
    - Session are stored per-directory in the database, located in `~/.kiro/`
- You can resume any saved chat session

#### Start a new chat
```bash
kiro-cli chat
# Or the equivalent command:
kiro-cli
```
```
/chat new
```

#### Start a new chat with a question
```bash
kiro-cli chat "What is the weather today?"
```
```
/chat new How do I set up a React project
```

#### Resume most recent chat session
```bash
kiro-cli chat --resume
```

#### Select which previous chat session to resume
```bash
kiro-cli chat --resume-picker
```
```
/chat resume
```

#### Save a chat session to file
```
/chat save <path>
```

#### Load a chat session from file
```
/chat load <path>
```

#### List all available sessions
```bash
kiro-cli chat --list-sessions
```

#### Delete a chat session
```bash
kiro-cli chat --delete-session <SESSION_ID>
```

## Models
- Kiro gives you access to different models
    - Different models have different strenghts
    - Choose the one that is best for your workflow
    - or you can let Kiro decide by choosing `Auto`

#### Set the default model
- You preference will be stored in `~/.kiro/settings/cli.json`
- For every new chat session, the given model will be used
```bash
kiro-cli settings chat.defaultModel [MODEL]
```
- Set the currently used model as default model
```
/model set-current-as-default
```

#### Choose a model
- Brings up an interactive model selection tool
```
/model
```
- Choose a specific model
```
/model [NAME]
```

## Context
- Context is the additional information that gets provided alongside your messages to help Kiro give better, more relevant responses
- Context gives Kiro background knowledge about the project and environment
- Forms of context: 
    1. Steering files
        - Files in `~/.kiro/steering`
        - These files are automatically included in every message
    2. `/context` command
        - Additional files or information
    3. System context
        - Automatically detected information, e.g. OS, cwd, current time

#### Show context rule configuration and matched files
```
/context show
```

#### Add new context rule
```
/context add <PATH>
```
- Add context even if the matched files exceed size limits
```
/context add --force <PATH>
```

#### Remove specific context rules
```
/context remove <PATHS>
```

#### Remove all context rules
```
/context clear
```

### Context window
- The `context window` is the total amount of text that Kiro can process in a single conversation
- It includes the system prompt, steering files, context entries, the full conversation history and responses
- The `context window` is finite. If the conversation gets too long, older messages may get truncated or Kiro may lose track of earlier details
- The context window usage (in %) is shown at every prompt
```
1% >
```

## Knowledge base
- Knowledge base is a persistent, centralized repository of organized information
- It is a curated set of documents that get fed into the context
- You can store, search and manage contextual information
- Knowledge base persists across chat sessions
- You can limit useage of context by using knowledge base
- Examples for Knowledge base:
    - Company wikis
    - Customer support help centers
    - Medical/legal reference databases
    - Vector databases
- In Kiro `Knowledge Base` is an experimental feature at the moment
- In Kiro `Knowledge Base`s are stored per agent
    - One agent cannot access the knowledge base of another agent

#### Enable knowledge base
- In Kiro you have to first enable the Knowledge Base feature
```
kiro-cli settings chat.enableKnowledge true
```

#### Display all entries in the knowledge base
```
/knowledge show
```

#### Add entry to the knowledge base
```
/knowledge add --name <NAME> --path <PATH>
```

## Tools
- `tools` can be interpreted as "capabilities"
- With `tools` Kiro can interact with it's environment and get things done
- `tools` include
    - File operations: read, create, edit, search
    - Bash: run shell commands
    - Code intelligence: search symbols, find references, parse, go-to-definition, etc
    - Grep: run `grep` for search
    - Glob: run `glob`
    - AWS CLI: make AWS API calls directly
    - Subagents: delegate subtasks to run in parallel

#### Show currently set up tools and permissions
```
/tools
```
- Output:
```
Tool              ~Tokens    Permission
▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔▔
Built-in
- code               1.4k    trust read-only operations
- shell               150    not trusted
- read                870    trust working directory
- write               490    not trusted
- glob                260    trust working directory
- grep                610    trust working directory
- introspect          230    trusted
- report                -    not trusted
- session             760    not trusted
- aws                 420    trust read-only commands
- subagent            850    not trusted
  Total              6.0k
```

#### Show the input schema for available tools
```
/tools schema
```

#### Set a specified tool permission to trusted or untrusted
````
/tools trust <TOOL_NAME>
````
````
/tools untrust <TOOL_NAME>
````

#### Set all tool's permission to trusted
```
/tools trust-all
```

#### Reset all permission levels to default
```
/tools reset
```

## MCP
- Model-Context-Protocol servers
- MCP is an open protocol that standardizes how applications provide context to LLMs
- MCP is the transport layer, and tools are what get exposed through it
- MCP servers are somewhat like plugins to Kiro
- MCP enables communication between Kiro and locally running MCP server
- MCP servers extend Kiro's capabilities, they provide additional tools and resources
- When an MCP server is connected, its tools become available to Kiro, just like the native tools
- These locally running servers can connect to
    - Databases
    - APIs
    - Workflows

## Agents

- An agent is a configurable persona, that defines how Kiro behaves during the session
- You can create multiple agents
- Custom agents provide a way to customize Kiro's behaviour
- Each agent has its own system prompt, allowed tools, behaviour config
- Agents are defined by their configuration file
    - The file specifies which tools the agent can access, permission, contexts
- There are
    - Built-in agents: `kiro_default`, `kiro_help`, `kiro_planner`. These cannot be edited
    - Global agents: stroed in `~/.kiro/agents/`
    - Workspace agents: stored in `/workspace/.kiro/agents`
- You can pre-approve or restrict tools for specific agents
- You can include relevant context, load project files, documentations for different agents

#### List available agents
```bash
kiro-cli agent list
```
```
/agent list
```
#### Create new agent
```bash
kiro-cli agent create <NAME>
```
```
/agent create
/agent generate
```
- This will prompt will you to set a name, description, scope etc

- Create new agent from an existing agent
```
/agent create --from kiro_default
```

- Create new agent is a specified directory
```bash
kiro-cli agent create <NAME> --directory <PATH>
```
```
/agent create <NAME> --directory <PATH>
```

- Create a new agent immediately with description
```
/agent create <NAME> --description <TEXT>
```

- Create a new agent with an MCP server
```
/agent create <NAME> --mcp-server <SERVER>
```

#### Edit an existing agent
```
/agent edit <NAME>
```

#### Switch to another agent
- To swith between agents use the `swap` command
    - This opens an interactive editor
```
/agent swap
```

#### Set the default agent
- The default agent is `kiro-default`
- You can change the default agent
```
/agent set-default <NAME>
```


## Skills


## Code intelligence
1. Kiro has some built-in code intelligence
    - It works for 18 languages
    - Can search symbols, lookup definitions etc
    - No need to install language server
2. You can integrate Language Server Protocol (LSP) for more precision
    - Requires language server installation
    - Enhances precision

#### Get overview of the workspace
```
/code overview
```

#### Get overview of a specific directory
```
/code overview <PATH>
```

#### Generate documentation for the codebase
```
/code summary
```
- In the opened interactive session you can choose the output format

#### Initialize LSP
- Run this in the project root
```
/code init
```
- This creates `lsp.json` in the workspace level

## Steering
- Steering gives Kiro persistent knowledge about the workspace, through .md files
- This ensures Kiro consistently follows established patterns, libraried, standards
- Instead of explaining the project conventions in every chat, steering files store this information
- Steering file should be shared among developers for standardization
- Steering can be 
    1. `workspace scoped`, stored in `workspace/.kiro/steering`
    2. Or `global scoped`, stored in `~/.kiro/steering`
- The global steering can be used to define centralized steering that apply to entire teams
- Foundational steering files:
    - `product.md`: defines product's purpose, target users, key features, business objectives
    - `tech.md`: defines chosen frameworks, libraries, dev tools, technical constraints
    - `structure.md`: defines file organization, naming conventions, import patterns, architectural decisions
- You can create your own, specialized guidance, e.g. `api-standards.md`
    - Use markdown syntax and natural language
- You can steer agents via the `AGENTS.md` file

## Prompts
- Prompts are reusable templates
- There are 3 types of prompts:
    1. **Local prompts**: Project specific, stored in `workspace/.kiro/prompts`
    2. **Global prompt**: User-wide prompts, available across all projects, stored in `~/.kiro/prompts`
    3. **MCP prompts**: Provided by MCP servers
- You can create, edit, and organize prompts
- Prompts can take arguments

#### List available prompts
```
/prompts list
```

#### Get details of a specific prompt
```
/prompt details <NAME>
```

#### Create new local prompt
- Using the interactive editor
```
/prompts create --name <NAME>
```

- Create prompt in one go, without the interactive editor
```
/prompts create --name <NAME> --content <CONTENT>
```

- Create new prompt in the global scope, intead of local
```
/prompts create --global --name <NAME>
```

#### Edit existing prompt
```
/prompts edit <NAME>
```

#### Use prompt
- Use the `@` sign to invoke a prompt
```
@my-prompt
```
- Or use the slash command
```
/prompts get <NAME> [ARGUMENTS]
```

#### Prompts with the same name
- If multiple prompts have the same name, the priority decides with one will be used
    1. Local - highest priority
    2. Global
    3. MCP - lowest priority

## Experimental features
- Kiro includes experimental features
- These provide advanced functionality
- They can be toggled on or off
- To open the interactive menu:
```
/experiment
```

## Quit Kiro CLI
- If you are in a Kiro CLI chat session, you can exit and get back to your terminal
```
/quit
```
- `/quit` is the official way, but `/exit` also works
```
/exit
```