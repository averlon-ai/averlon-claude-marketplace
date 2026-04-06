# Container Remediation Plugin

Docker and container security analysis with vulnerability detection and remediation, powered by Averlon's intelligence — directly inside Claude Code.

## What It Does

This plugin connects Claude Code to Averlon's MCP server, giving Claude real-time access to container vulnerability data and remediation recommendations. When you ask Claude to fix vulnerabilities in your Dockerfiles, it uses Averlon's curated intelligence to identify the right package upgrades and apply them.

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/) installed and running (the Averlon MCP server runs as a container)
- Averlon API keys configured (see [Getting Started](../README.md#getting-started))

## Usage

Once the plugin is installed, invoke the remediation skill:

```
/averlon-remediate <path-to-Dockerfile>
```

For example:

```
/averlon-remediate Dockerfile
```

The skill walks you through:

1. Selecting vulnerability filters (defaults: `Recommended,Critical,HighRCE`)
2. Optionally providing an image repository for more accurate matching. It's always best to be explicit with your image repository — you can find the exact name by going to [Averlon Images](https://app.prod.averlon.io/images), locating your image repository, and copying its name.
3. Fetching vulnerability recommendations from Averlon
4. Applying the remediation to your Dockerfile

### Available Filters

| Filter | What it targets |
|--------|----------------|
| `Recommended` | Averlon-recommended upgrades — curated, low-noise fixes |
| `Exploited` | Known-exploited vulnerabilities (CISA KEV, etc.) |
| `Critical` | Critical severity CVEs |
| `High` | High severity CVEs |
| `HighRCE` | High severity with Remote Code Execution |
| `Medium` | Medium severity CVEs |
| `MediumApplication` | Medium severity in application-layer packages only |
| `Low` | Low severity CVEs |
| `LowApplication` | Low severity in application-layer packages only |

## Troubleshooting

**MCP server fails to connect**
- Verify Docker is running: `docker ps`
- Check your environment variables are set: `echo $AVERLON_MCPCLIENT_API_KEY`
- Ensure the Docker image is accessible: `docker pull ghcr.io/averlon-security/averlon-mcp:sha-a8e5b91`

**"No recommendations found"**
- Verify the Dockerfile path is correct
- If using `image-repository`, ensure it matches the image registered in Averlon
- Try broadening your filters (e.g., `Recommended,Critical,High,Medium`)

**Plugin not showing up**
- Run `/reload-plugins` after installation
- Check `/plugin` > Errors tab for loading issues
