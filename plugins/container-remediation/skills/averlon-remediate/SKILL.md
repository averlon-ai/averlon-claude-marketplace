---
name: averlon-remediate
description: Guides container vulnerability remediation using Averlon MCP. Use when the user wants to fix container vulnerabilities, update Dockerfile packages, or get Averlon container recommendations.
argument-hint: [dockerfile-path]
user-invocable: true
---

# Averlon Container Recommendation & Remediation

You are helping the user remediate container vulnerabilities using the Averlon MCP tools. Follow these steps in order.

## Step 1: Gather Inputs

If `$ARGUMENTS` is provided, use it as the Dockerfile path. Otherwise, ask the user for the **Dockerfile path** (relative to the repo root).

Then print the available filters as a table so the user can see all options:

| Filter | What it targets |
|--------|----------------|
| `Recommended` | Only Averlon-recommended upgrades — curated, low-noise fixes |
| `Exploited` | Only known-exploited vulnerabilities (CISA KEV, etc.) — urgent, active threats |
| `Critical` | Critical severity CVEs — highest CVSS scores |
| `High` | High severity CVEs |
| `HighRCE` | High severity with Remote Code Execution — exploitable remotely |
| `Medium` | Medium severity CVEs |
| `MediumApplication` | Medium severity in application-layer packages only |
| `Low` | Low severity CVEs |
| `LowApplication` | Low severity in application-layer packages only |

Then use AskUserQuestion with **two questions** in a single call:

1. **Filters** (header: "Filters") — "Enter vulnerability filters as comma-separated values (e.g. Recommended,Exploited,Critical,HighRCE). Leave blank for default: Recommended,Critical,HighRCE" — provide just two options: `Recommended,Exploited,Critical,HighRCE (Recommended)` as the default preset, and `Recommended,Exploited,Critical,High`. The user can type custom comma-separated filters via "Other".

2. **Image repo** (header: "Image repo") — "Enter an optional image repository to help match the Dockerfile to the correct container image (e.g. docker.io/library/nginx). Leave blank to skip." — provide two options: `Skip` to skip, and a placeholder option. The user can type a custom repository via "Other".

If the user selects the default preset or leaves filters blank, use `Recommended,Exploited,Critical,HighRCE`. If they select `All`, use all filters joined with `,`.

## Step 2: Detect Git Repository

Run `git remote get-url origin` via bash to automatically determine the git repository URL. Use this as the `gitRepo` parameter.

## Step 3: Get Container Recommendations

Call the `averlon_get_container_recommendations` MCP tool with:
- `gitRepo`: the git remote URL from Step 2
- `path`: the Dockerfile path from Step 1
- `filters`: the selected filters from Step 1, joined with `,` (e.g. `Recommended,Critical,HighRCE`)
- `imageRepository`: the image repository from Step 1 (omit if not provided)

Present the results to the user — summarize the vulnerabilities found, grouped by layer and package, highlighting CVE IDs, severity, and available fixes.

## Step 4: Load Remediation Skill

Call the `averlon_get_remediation_skill_file` MCP tool with:
- `name`: `remediate-container-local`
- `file`: `SKILL.md`

Read the returned skill instructions carefully.

Follow the skill instructions step by step to remediate the vulnerabilities.