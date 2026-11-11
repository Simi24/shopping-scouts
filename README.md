# shopping-scouts

Tools that let an AI agent help you buy things online — and tell you when a "deal" is not one.

| | What it does |
|---|---|
| [ali-scout](https://github.com/Simi24/ali-scout) | Search AliExpress, read reviews, judge prices against the 30-day low |
| [amazon-scout](https://github.com/Simi24/amazon-scout) | Search Amazon, read its review summary, build a ready-to-checkout cart link |
| [price-history](https://github.com/Simi24/price-history) | Price history by store from Google Shopping, with a deal verdict |
| **shopping-advisor** (this repo) | The skill that ties them together: is the price good, are there better-value alternatives, what does the extra money buy |

## Why

A robot vacuum on sale at 499.99, "−67%" from 1,499.99. The history says the market price was 760-910 and the strikethrough was the launch price: a real but much smaller discount. And a 299.99 model from the same brand, with better reviews, did everything that person needed — the flagship's extras solved problems they did not have. shopping-advisor asks those questions every time.

## Works with any agent

Nothing here is tied to one agent:

- **CLIs** — any agent with a shell (Claude Code, Codex, Gemini CLI, Cursor, Copilot...) can run them.
- **Skills** — in the open [Agent Skills](https://agentskills.io) format (`SKILL.md`), read by most agents.
- **MCP servers** — for agents without a shell; they run straight from GitHub with `uvx`.
- **AGENTS.md** in every repo — point your agent at a repo and ask it to set it up.

## Install

You need [uv](https://docs.astral.sh/uv/). Amazon and price history go through [SerpApi](https://serpapi.com/): a free account gives 250 searches a month.

```bash
uv tool install git+https://github.com/Simi24/ali-scout
uv tool install git+https://github.com/Simi24/amazon-scout
uv tool install git+https://github.com/Simi24/price-history
amazon-scout setup        # opens SerpApi, asks for the key without echoing it, saves it for both tools
```

Then add the skills to your agent: copy or symlink `skills/*` from each repo into its skills folder (e.g. `~/.claude/skills/`). Or just tell your agent: *"set up the tools in github.com/Simi24/shopping-scouts"* — the [AGENTS.md](AGENTS.md) files tell it how.

### Claude Code

All four come as plugins from this marketplace (skills and MCP servers included; the CLIs still need `uv tool install` for shell use):

```text
/plugin marketplace add Simi24/shopping-scouts
/plugin install shopping-scouts@shopping-scouts
/plugin install ali-scout@shopping-scouts
/plugin install amazon-scout@shopping-scouts
/plugin install price-history@shopping-scouts
```

### Other agents (MCP)

Each tool's README has ready-made MCP configurations for Claude Desktop, Cursor, Gemini CLI, VS Code and Codex.

## Example

> **You**: is this a good deal? amazon.it/dp/B0DJ3284BS
>
> **Agent**: 499.99 is an all-time low in Italy, but the real discount is ~30%, not 67%. For your flat (tile floors, 10+ cm under the furniture) the Qrevo S Pro at 299.99 does the same job with better reviews (★4.5 from 5,401 vs ★4.1 from 3,022); the Saros' extras are for furniture under 8 cm and high thresholds. Cart: https://www.amazon.it/gp/aws/cart/add.html?ASIN.1=B0GHN9DR1C&Quantity.1=1

## License

MIT
