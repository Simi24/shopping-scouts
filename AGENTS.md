# AGENTS.md

Instructions for AI agents (Claude Code, Codex, Gemini CLI, Cursor, Copilot...) asked to set up these shopping tools for a person.

## Set up everything
Ask before installing anything. Needs [uv](https://docs.astral.sh/uv/).

1. Install the CLIs:
   ```bash
   uv tool install git+https://github.com/Simi24/ali-scout
   uv tool install git+https://github.com/Simi24/amazon-scout
   uv tool install git+https://github.com/Simi24/price-history
   ```
2. Install the skills in your skills folder (Agent Skills format): `skills/shopping-advisor` from this repo, and `skills/<tool>` from each tool's repo. If you do not support skills, read the `SKILL.md` files and follow them.
3. SerpApi key, needed by amazon-scout and price-history: the **person** must create a free account at https://serpapi.com/users/sign_up (email verification) and then run `amazon-scout setup` **in their own terminal**: it reads the key without echoing it and saves it to `~/.config/serpapi/key`, shared by both tools. Never ask for the key in the chat; never print or commit it. Skip this step if they only want AliExpress.
4. Ask which country they shop in and set the defaults in their shell profile, e.g. for Italy: `ALI_SCOUT_LOCALE=it`, `AMAZON_SCOUT_DOMAIN=amazon.it`, `PRICE_HISTORY_COUNTRY=it`.
5. Check: `ali-scout search "usb c cable" --limit 1` and `amazon-scout credits`.

If the agent cannot run a shell, configure the MCP servers instead (see each tool's README).
