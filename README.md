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
```
> kiro-cli --version
kiro-cli 1.28.0
```
- Commands that start with **/** should be run in a kiro cli instance
```
> /help
Switching to the [help] agent.
Welcome to Kiro CLI Help!
```

## Launching Kiro CLI
- Standard lauching Kri CLI
```
> kiro-cli
```
- Launching Kiro CLI with a setting
```
> kiro-cli --untrust-fs-read
```
(we will later see what this untrust setting does)

## Settings
Kiro has 2 types of settings:
- global configuration settings and
- project specific settings used in the active session
```
+-------------------------------------------------------------------+
|             Global               |            Workspace           |
+----------------------------------+--------------------------------+
|         ~/.kiro/settings         |   /workspace/.kiro/settings    |
+-------------------------------------------------------------------+
```
> **_IMPORTANT_**:  Workspace settings override global settings!

```bash 
# List all configured settings
> kiro-cli settings list

# List all available settings (with description)
> kiro-cli settings list --all

# Show specific setting
> kiro-cli settings chat.greeting.enabled

# Set the given setting
> kiro-cli settings chat.greeting.enabled true
