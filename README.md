# Averlon Claude Marketplace

A collection of Averlon security plugins for [Claude Code](https://docs.claude.com/en/quickstart).

## Getting Started

### 1. Get your Averlon API keys

You need an **MCPClient-scoped** API key pair from Averlon.

1. Sign in to the [Averlon Console](https://averlon.io)
2. Navigate to **API Keys** > **Create New Key**
3. Select scope: **MCPClient**
4. Create the key pair and copy both values: `Key ID` and `Key Secret`

> Requires Averlon admin access. Ask an Averlon org admin to create the key pair if you don't have admin access.

### 2. Set environment variables

Add these to your shell profile (`~/.zshrc`, `~/.bashrc`, etc.):

```bash
export AVERLON_MCPCLIENT_API_KEY="<your Key ID>"
export AVERLON_MCPCLIENT_SECRET_KEY="<your Key Secret>"
```

Reload your shell or run `source ~/.zshrc` to apply.

### 3. Add the marketplace

From within Claude Code, run:

```
/plugin marketplace add https://github.com/averlon-ai/averlon-claude-marketplace.git
```

### 4. Install a plugin

```
/plugin install <plugin-name>@averlon-claude-marketplace
```

For example, to install the container remediation plugin:

```
/plugin install container-remediation@averlon-claude-marketplace
```

This opens the plugin UI where you can choose the installation scope — user (available across all projects), project, or team.

### 5. Restart Claude Code

After installing a plugin, restart Claude Code for the new plugins/skills to be initialized:

```
/exit
```

Then reopen Claude Code.

## Available Plugins

| Plugin | Description | Docs |
|--------|-------------|------|
| `container-remediation` | Docker and container security analysis with vulnerability detection and remediation | [View docs](docs/container-remediation.md) |
