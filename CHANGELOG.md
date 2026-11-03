# Changelog

## 0.2.0 — Unreleased

- Renamed the plugin from `trilio-site-recovery` to `site-recovery-kubernetes`, following the naming rule (plugin name = repo name without `-ai-plugin`). Install with `site-recovery-kubernetes@trilio` and invoke the skill as `/site-recovery-kubernetes:tsr-guide`.
- Moved the plugin to `plugins/site-recovery-kubernetes/`. Its manifests and MCP files are now generated from `plugin.yaml` by trilio-agent-plugin-kit.
- Claude Code and Codex install from trilio-agent-marketplace; Cursor and Junie still install from this repo.

## 0.1.0 — 2026-10-23

- Initial scaffold: plugin for Claude Code, Codex, Cursor and Junie, the TSR docs MCP server, and the `tsr-guide` skill with workflows for prerequisites, installation, architecture, custom resources and troubleshooting.
