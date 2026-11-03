# Changelog

## 0.2.0 — Unreleased

- Renamed the plugin from `trilio-site-recovery` to `site-recovery-kubernetes`, following the naming rule (plugin name = repo name without `-ai-plugin`). Install with `site-recovery-kubernetes@trilio` and invoke the skill as `/site-recovery-kubernetes:tsr-guide`.
- Moved the plugin to `plugins/site-recovery-kubernetes/`. Its manifests and MCP files are now generated from `plugin.yaml` by trilio-agent-plugin-kit.
- Claude Code, Codex and Cursor all install from trilio-agent-marketplace. Removed this repo's Cursor and Junie marketplace files.
- Dropped Junie support: no `extension.json` or `mcp/.mcp.json` any more.
- The docs MCP server is now `trilio-site-recovery-kuberentes-docs` at `https://docs-tsr.trilio.io/api/mcp/trilio-site-recovery-for-kubernetes`. If you added it by hand for Codex, re-add it under the new name and URL.
- Added `evals/cases.yaml` with the five TSR eval cases the kit's phase 1 gate requires.

## 0.1.0 — 2026-10-23

- Initial scaffold: plugin for Claude Code, Codex, Cursor and Junie, the TSR docs MCP server, and the `tsr-guide` skill with workflows for prerequisites, installation, architecture, custom resources and troubleshooting.
