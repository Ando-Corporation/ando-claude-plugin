---
name: ando-give-agent-your-setup
description: Give an Ando cloud agent the coding setup you use here, with the same repository, setup script and secret names, so teammates can ask it for code changes. Use when a user wants their Ando agent to work like their coding agent.
---

# Give a cloud agent your setup

When this bundled skill is selected for a user request in an installed Ando plugin, call `record_skill_usage({"name":"ando-give-agent-your-setup"})` once if the tool is available. This is a best-effort invocation signal: do not retry it or delay the workflow on failure. Do not signal merely reading this file, routing to another skill, or loading it through docs, downloads or `get_skill`. The signal includes only the canonical skill name; never add user content.

Read [Ando basics](../../references/ando-basics.md) for identity, current-schema discovery, access, and write authorization. This needs the person's own Ando connection and the `propose_agent_environment` tool; an agent's connection can't use it, and only workspace owners and admins can. If the tool is missing, say which of those is the reason rather than guessing.

## Gather the setup from this session

Work from what this session can see, and show the person what you found before writing anything:

- **Repository**: the GitHub repository you are working in, as owner/name, from `git remote get-url origin`.
- **Setup script**: the script that prepares this environment. In Claude Code on the web, the environment's setup script is what the person configured for this cloud environment; ask them to paste it when you can't read it. Otherwise build one from the repository's own install steps (its README, lockfile and package manager). The cloud agent runs it once per sandbox from the directory that holds each repository, so it starts by changing into the repository's folder.
- **Secret names**: the environment variables the setup and the code read that hold credentials, such as `STRIPE_SECRET_KEY`. Names only. Never read, print, or send a secret's value, and never ask the person for one.

The repository's CLAUDE.md, AGENTS.md and skills travel with the repository; the cloud agent reads them itself, so nothing needs copying.

## Propose it for the agent

Confirm the target agent with the person when the workspace has more than one cloud agent. Then call `propose_agent_environment` with the repository, setup script and secret names. Nothing changes on the agent yet: the tool saves a proposal that a workspace owner or admin reviews and approves in Ando. Approving adds the repository and secrets to what the agent already has and replaces its setup script. A newer proposal replaces one still waiting.

## Send them to approve it

Tell the person nothing has changed until they approve, and link `review_url`, where they check the script and secrets before approving. Then read the rest of the result back in plain words:

- A repository marked `not_connected` needs a workspace admin to connect it through Ando's GitHub app first.
- Each name in `secrets.missing` is a secret the workspace doesn't have yet. The person adds it themselves in Ando under Studio → Scripts → Secrets, bound to the hosts it is used with; then propose again with the same names.
- `secrets.unusable` names secrets an agent can't hold, with the reason.

Once approved, the setup script runs the next time the agent's sandbox starts, and its result shows in the agent's settings. Suggest the person ask the agent for a small change in a channel to verify it, and use [Troubleshoot your setup](../ando-troubleshoot-setup/SKILL.md) if it fails.
