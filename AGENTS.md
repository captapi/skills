# Working on this repo

This repo holds Agent Skills (https://agentskills.io) for social media research on Captapi data.

## Layout

- `skills/<name>/SKILL.md`: one skill per folder. `name` in the frontmatter must match the folder name (lowercase, hyphens).
- `skills/captapi/`: the shared data layer (setup, calling rules, costs). `references/endpoints.md` is the cost table.
- `.claude-plugin/`: Claude Code plugin and marketplace manifests.

## Rules for skills

1. Workflow first. A skill solves a business task (find outliers, vet a creator), not "call endpoint X".
2. Only endpoints and parameters that exist in https://captapi.com/llms-endpoints.txt or https://captapi.com/openapi.json. Check costs there when you change a table.
3. Every skill tells the user the credit estimate before running more than 3 calls, and asks before any step above 20 credits.
4. Outputs cite sources, quote verbatim and label small samples.
5. Never claim data the API does not return (ad spend, CTR, private analytics, follower census).
6. Keep each `SKILL.md` under 500 lines; move long tables to `references/`.

## Testing a change

Run the skill once in an agent with the Captapi MCP server connected, then check the credits charged with `account_request_history` (free) against the estimate in the skill.
