# CLAUDE.md — role-engineering-lead

You are in the source repo of **`rasa.module.role.engineering-lead`**: the `engineering-lead` role as a RasaOS role module. It is not the tenant and not a codebase. The role runs in sessions elsewhere.

## Provisional layout

Canon has no `kind: role` yet (tenant ADR 2026-10-10-roles-are-element-modules D2), so this repo is a `module`. Three things follow galt ADR 0064's role layout rather than canon, until canon adds the kind:

- The repo is named `role-engineering-lead`: the ADR 0064 form, and canon's form once `kind: role` lands. It is not `module-role-engineering-lead`.
- The payload is at the root (`role.json`, `prompt.md`, `.claude-plugin/`), because galt's loader passes the repo root as `--plugin-dir`. There is no `content/`.
- `rasa.json` carries the role's id and version under `rasa`.

## Rules

1. **This repo is the source.** Change `role.json` and `prompt.md` here, by PR. Until galt reads crew roles from Elements, mirror the change into galt's in-code copy (`packages/router/src/crew/`).
2. **Every change to the role bumps two versions.**
   - The role version: `role.json` `version` and `rasa.json` `rasa.role_version`. Bump it past the live version, because galt refuses a role version that is not greater.
   - The Element version: a minor bump for a role change, a patch for docs. It appears in `VERSION`, `rasa.json`, `.claude-plugin/plugin.json`, the README `**Version:**` line and a new `CHANGELOG.md` heading.
3. **Nothing that widens.** No `settings.json`, `.mcp.json` or `hooks/`, and `rasa.json` `permissions` stays `[]`. Only a person may load a widening Element (ADR D5). Gate grants belong in `role.json` and act only through a role assignment a person publishes.
4. **Gate.** `RASA_SCHEMA_DIR=~/rAI/rasa-os/schema bin/check-role` must print GREEN before every commit.
5. **Release.** Commit, tag `v<VERSION>` and push the tag. Never move a published tag; fix forward.
6. **Merged is not live.** A role goes live only when an admin runs DOS ⚡ Publish roles from the tenant.
7. **Public.** The repo is open. Never put a secret, a credential, customer data or anything read from production into it. Check 8 catches only the obvious token shapes.
