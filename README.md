# Trilio Site Recovery for coding agents

This repository contains a plugin that helps coding agents set up and operate [Trilio Site Recovery](https://trilio.io) (TSR) on Kubernetes and OpenShift: checking prerequisites, installing TSR, configuring disaster recovery between clusters, running failover and failback, and troubleshooting.

The plugin has two parts:

- **The TSR docs MCP server.** Installing the plugin connects the agent to the official TSR documentation, which is generated from the TSR source. The agent searches it instead of relying on what it remembers about TSR.
- **The `tsr-guide` skill.** It runs when you mention Trilio Site Recovery, Trilio Site Recovery for Kubernetes, TSR or TSR-K. It guides prerequisites, installation, architecture, custom resources and troubleshooting from the matching docs pages, cites the pages it used, and matches the docs to your TSR version.

**Safety:** for TSR resources and operations, the agent may freely run read-only commands such as `oc get` or `oc describe`. Before anything that changes a TSR resource (applying, patching or deleting TSR custom resources or install objects, `tsrctl zone-credential` changes, or starting a failover or test failover), it shows you the exact command or manifest and the cluster it targets, then waits for your explicit yes. This applies even if you've auto-approved shell commands.

Workflows cover the TSR setup path. Failover and other day-2 operations aren't scripted yet; the agent answers them from the docs, under the same rules.

## Instructions

The plugin is available for Claude Code, Codex and Cursor, all from the [Trilio marketplace](https://github.com/trilioData/trilio-agent-marketplace).

### Claude Code

#### Installation

Run the following commands inside a Claude Code session.

1. Add the Trilio marketplace:
```
/plugin marketplace add trilioData/trilio-agent-marketplace
```

2. Install the plugin:
```
/plugin install site-recovery-kubernetes@trilio
```

#### Usage

Claude Code invokes the skill automatically when a task involves TSR. To invoke it explicitly:

```
/site-recovery-kubernetes:tsr-guide
```

#### Updating

```bash
claude plugin marketplace update trilio
claude plugin update site-recovery-kubernetes@trilio
```

Then run `/reload-plugins` in an open session.

### Codex

#### Installation

Run the following commands in a terminal.

1. Add the Trilio marketplace:
```
codex plugin marketplace add trilioData/trilio-agent-marketplace
```

2. Install the plugin:
```
codex plugin add site-recovery-kubernetes@trilio
```

If `codex mcp list` doesn't show the `trilio-site-recovery-kubernetes-docs` server after installing, add it by hand, using the URL from [Updating the docs URL](#updating-the-docs-url):

```bash
codex mcp add trilio-site-recovery-kubernetes-docs --url "<docs URL>"
```

#### Updating

Refresh the marketplace and reinstall the plugin so Codex replaces its cached copy:

```bash
codex plugin marketplace upgrade trilio
codex plugin remove site-recovery-kubernetes@trilio
codex plugin add site-recovery-kubernetes@trilio
```

### Cursor

#### Installation

1. Add the Trilio marketplace by running the following command in a terminal:
```
cursor-agent plugin marketplace add https://github.com/trilioData/trilio-agent-marketplace
```

2. Install the `site-recovery-kubernetes` plugin with the `/plugins` command inside a Cursor session.

#### Updating

```bash
cursor-agent plugin marketplace update trilio
```

Reopen Cursor. If the plugin is still on the previous version, reinstall it with the `/plugins` command.

## Updating the docs URL

The TSR docs MCP server is at:

```
https://docs-tsr.trilio.io/api/mcp/trilio-site-recovery-for-kubernetes
```

If this URL changes:

1. Update `mcpServers` in `plugins/site-recovery-kubernetes/plugin.yaml`, `DOCS_URL` in `scripts/check.sh`, and this section.
2. Bump the version and regenerate, as in [CONTRIBUTING.md](CONTRIBUTING.md#changing-manifests-or-releasing-a-version). `pluginkit generate` rewrites the MCP file for each agent: `.mcp.json` (Claude Code and Cursor) and `mcp.json` (Codex).

## Development

The plugin's source is `plugins/site-recovery-kubernetes/plugin.yaml` plus its skills. [trilio-agent-plugin-kit](https://github.com/trilioData/trilio-agent-plugin-kit) generates every agent's manifest and MCP file from it; CI fails if a generated file is edited by hand.

```bash
git clone git@github.com:trilioData/trilio-agent-plugin-kit.git .pluginkit -b v0.1.0
pip install pyyaml
python .pluginkit/pluginkit.py generate .    # after editing plugin.yaml or the skill
python .pluginkit/pluginkit.py validate .
sh scripts/check.sh                          # skill and README checks; needs jq
CHECK_ONLINE=1 sh scripts/check.sh           # also confirms the docs MCP server responds
```

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to change and evaluate the plugin.

## License

[Apache-2.0](LICENSE)
