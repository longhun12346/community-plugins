# Electron Debug

Electron Debug is a Skill-only Codex plugin for coding Agents working on Electron applications. It guides an Agent to collect bounded runtime evidence after a code change by attaching to a running local Electron application through the project's Node Inspector and renderer CDP discovery endpoints.

The plugin distributes the Skill. It does not bundle the MCP server, register MCP automatically, start or stop Electron, execute page code, inspect the DOM, take screenshots, set breakpoints, or decide whether a business requirement passed. The MCP source and build instructions remain in [electron-agent-debug](https://github.com/longhun12346/electron-agent-debug).

## Safe first run

Start a new Codex task after installation and explicitly provide the project prerequisites:

`Use $electron-debug after I have started the Electron app with the project's existing command and provided the actual local Inspector/CDP discovery endpoints. Collect baseline and regression evidence for the change; report BLOCKED if a prerequisite is missing.`

The Agent must supply the Electron project directory, existing start or reload command, expected behavior, regression command, required endpoint URLs, and target selection condition. Ports and paths are chosen by the Agent or the project; this plugin does not prescribe fixed values.

## Install

After this plugin is accepted into the community marketplace:

`codex plugin marketplace add tonyloehr/community-plugins --ref main --sparse .agents/plugins --sparse plugins/electron-agent-debug
codex plugin add electron-agent-debug@community-plugins`

Start a new Codex task after installation so the Skill is loaded. Installing the plugin does not configure the MCP server.

## Configure the MCP prerequisite

The Agent must separately build and configure the MCP server from the source repository:

1. Build the checked-out source with the repository's documented command.
2. Configure the current host's local stdio MCP entry to run `node <actual-repository-path>/dist/index.js`.
3. Refresh the host configuration and confirm that `electron_attach`, `electron_status`, `electron_targets`, and `electron_logs` are visible.
4. Start or confirm the Electron application using its existing project command, with dynamic loopback Inspector/CDP endpoints.

The exact host configuration file, repository path, Electron command, ports, and target selector are environment-specific and must be discovered by the Agent.

## Permissions and data boundary

The manifest declares `Read`. The plugin has no credentials, OAuth flow, remote service, install hook, bundled executable, or automatic file write. The Skill may guide the Agent to use the project's existing commands and configure the host's MCP entry. The MCP server accepts only loopback HTTP(S) discovery endpoints and keeps bounded diagnostic events in memory. No project files or logs are uploaded by this plugin.

Do not provide production endpoints, credentials, tokens, or private logs. If the MCP host, application, endpoint, target, expected behavior, or regression operation is unavailable, report `BLOCKED` instead of treating the missing prerequisite as a code failure.

## Verification

The source repository provides unit and contract tests, a real Electron fixture, and an opt-in external Electron E2E harness. The external harness requires a caller-provided project checkout and Electron executable; it is not part of the plugin install and is not a default marketplace test.
