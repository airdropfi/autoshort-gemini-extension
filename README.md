# AutoShort for Gemini CLI

Plan, draft and render faceless short videos on your own [AutoShort](https://autoshort.org) account without leaving Gemini CLI. The extension connects Gemini to the AutoShort MCP server and adds four slash commands and a context file that keep you in control: you review the script first, and nothing that costs credits runs until you have seen the price AutoShort returns and said yes.

## Before you start

- **You need an AutoShort account** (sign up at https://autoshort.org). The extension is a connector, not a free video generator.
- **Drafts are free, renders cost credits** from your own AutoShort balance. The price comes from AutoShort at the moment you ask; this extension never states a price of its own.
- You need Gemini CLI. A browser must be available once, for sign-in.

## Install

```bash
gemini extensions install https://github.com/airdropfi/autoshort-gemini-extension
```

Restart Gemini CLI, then sign in:

```text
/mcp auth autoshort
```

A browser window opens: sign in to AutoShort and click Allow. Sign-in is OAuth 2.0 with PKCE and dynamic client registration, discovered from the server, so there is no key to paste. Check it with `/mcp` (the `autoshort` server should list its tools) and try `/autoshort:credits`.

Update later with `gemini extensions update autoshort`; remove with `gemini extensions uninstall autoshort`.

### Which Gemini CLI, and Antigravity CLI

Google announced on 2026-05-19 that Gemini CLI stops serving Google AI Pro, Ultra and free individual accounts on 2026-06-18 (source: https://developers.googleblog.com/an-important-update-transitioning-gemini-cli-to-antigravity-cli). Gemini CLI keeps working with a paid Gemini API key or an enterprise license. If you use Antigravity CLI instead, this repository is also an Antigravity plugin (`.antigravity-plugin/plugin.json`, `mcp_config.json`, `rules/AGENTS.md`):

```bash
agy plugin install https://github.com/airdropfi/autoshort-gemini-extension
```

It connects the same MCP server (`https://autoshort.org/api/mcp`, OAuth sign-in opens in your browser when the server connects) with the credit-buying tools switched off (`disabledTools`), and loads the rules from `GEMINI.md` as an always-on rule. The slash commands below are Gemini CLI commands; Antigravity plugins have no commands, so there you just describe what you want. `agy plugin validate .` passes (with warnings that `GEMINI.md` and `commands/` are not loaded by Antigravity); we have not run the plugin end to end in Antigravity yet. Google's migration guide also describes `agy plugin import gemini` (https://antigravity.google/docs/cli/gcli-migration).

## Commands

| Command | What it does |
|---|---|
| `/autoshort:short <idea>` | Idea, script review, free draft, price check, render only after your yes. |
| `/autoshort:series <premise>` | Season plan for a multi-episode story: save, review, confirm, then produce one quoted episode at a time. Plan before produce. |
| `/autoshort:credits` | Credit balance and recent usage. Read-only. |
| `/autoshort:formats [focus]` | Reads your own post performance and the template gallery and suggests formats to test. Read-only. |

You can also just describe what you want ("make a 30-second short about why octopuses have three hearts"); `GEMINI.md` carries the same rules.

## What it will and will not do

- A draft is free and renders nothing. Every paid step (a render, a character sheet, an episode) is priced by an AutoShort tool first and needs your explicit yes to that price.
- The tools that buy credits and turn on auto-recharge are switched off in this extension (`excludeTools` in `gemini-extension.json`). To add credits, use AutoShort's own checkout page.
- Narration is read exactly as written. AutoShort does not rewrite it.
- Real or famous people, protected characters and sexual content are refused.
- Videos use AI-generated images and voice. Switch on the platform's AI-content label when you post.
- No result is promised: nobody can tell you how a video will perform.
- Gemini CLI asks you to confirm MCP tool calls. If you mark the `autoshort` server as trusted in your own settings, that confirmation is skipped, including for render tools; the extension does not set `trust` and we recommend you leave it off.

## Contents

```text
gemini-extension.json        manifest: name, version, remote MCP server https://autoshort.org/api/mcp
.antigravity-plugin/plugin.json  Antigravity plugin manifest
mcp_config.json              Antigravity MCP server definition (same server)
rules/AGENTS.md              Antigravity always-on rule (a copy of GEMINI.md: keep both in sync)
GEMINI.md                    context: what AutoShort is and the rules above
commands/autoshort/*.toml    /autoshort:short, :series, :credits, :formats
assets/icon.png              icon
scripts/validate.mjs         schema checks, no dependencies: node scripts/validate.mjs
```

## Data and privacy

The MCP server receives what the tools need: the scripts, briefs and settings you ask Gemini to send, and your video, credit and series data on your AutoShort account. The extension itself stores nothing and runs no code on your machine; it is a server address, Markdown and prompts. Sign-in tokens are stored by Gemini CLI and can be revoked in your AutoShort dashboard under API keys. See the [privacy policy](https://autoshort.org/en/privacy) and [terms](https://autoshort.org/en/terms).

## Support

https://autoshort.org/en/support or support@autoshort.org. Documentation: https://autoshort.org/en/developers.

## License

MIT for the files in this repository (see `LICENSE`). The AutoShort service is governed by its terms.
