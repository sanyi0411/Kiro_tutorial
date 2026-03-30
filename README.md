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

## Tools

## Agents

## Quit Kiro CLI
- If you are in a Kiro CLI chat session, you can exit and get back to your terminal
```
/quit
```
- `/quit` is the official way, but `/exit` also works
```
/exit
```