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

If `codex mcp list` doesn't show the `trilio-site-recovery-kubernetes-docs` server after installing, add it by hand:

```bash
codex mcp add trilio-site-recovery-kubernetes-docs --url https://docs-tsr.trilio.io/api/mcp/trilio-site-recovery-for-kubernetes
```

#### Updating

```bash
codex plugin marketplace upgrade trilio
```

This also updates the installed plugin. Start a new Codex session to use the new version.

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

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to set up the repo, validate and evaluate changes, and release a version.

## License

[Apache-2.0](LICENSE)
