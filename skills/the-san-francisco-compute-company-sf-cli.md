---
name: sf-cli
description: >
  Use this skill for SF Compute CLI (`sf`) tasks: installing, authenticating,
  managing capacities, instances, orders, images, procurements, or deployments.
  Triggers: "sf cli", "sf login", "sf instances", "sf capacities", "sf pools", sf orders",
  "sf availability", "sell compute", "buy compute", "sfcompute".
metadata:
  version: 0.0.1
---

# SF Compute CLI Skill

CRUD SF Compute instances, orders, images, pools, and capacities from the terminal.

## When To Use

- Install, upgrade, or authenticate the CLI
- CRUD operations on instances, orders, capacities, images, firewalls, subnets
- Set up procurements or deployments
- Check availability, billing, events, limits

## Key Rule: Use `sf agents ctx`

**Always run `sf agents ctx` first.** It outputs JSON with every command, flag, alias, default, and examples.

## Installation

```bash
curl -fsSL https://cli.sfcompute.com | bash
sf upgrade      # update
sf version      # check the cli version
sf uninstall    # remove
```

## Authentication

```bash
sf login        # browser-based
sf me           # current account
```

For scripts: set `SF_BEARER_TOKEN` or `SF_API_KEY`.

## Core Concepts

| Resource | Purpose |
|----------|---------|
| **Capacity** | Container of compute allocation over time |
| **Instance** | GPU machine running on a capacity |
| **Order** | Buy or sell compute time |
| **Instance SKU** | Identifier for a specific instance configuration |
| Workspace | Group resources to isolate teams or environments |

Creating/destroying instances doesn't affect billing — only orders do.

## Rules

- Agents should ensure `sf` commands output JSON
- Set NO_COLOR=1
- Turn off auto upgrades
- Disable interactive prompts

## Guidelines

- Use cli tools like jq instead of python or perl scripts to parse command output
- You may need to switch workspaces or use `--all` to answer the users questions

## Resources

- Docs: <https://docs.sfcompute.com>
- Install: <https://cli.sfcompute.com>
- `man sf` for built-in docs
