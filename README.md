# opendiavgeia MCP server

Ask Claude, ChatGPT, Cursor or any MCP client what Greek public bodies paid and to whom, and which public tenders are open. The server reads the decisions posted on [Diavgeia](https://diavgeia.gov.gr) since 2010, with totals per body, per place and per payee by tax number (ΑΦΜ), and the open tenders from [ΚΗΜΔΗΣ](https://cerpp.eprocurement.gov.gr) and [TED](https://ted.europa.eu). Every record it returns links to its page on [opendiavgeia.com](https://opendiavgeia.com), where you can check it against the original decision or notice.

```
https://opendiavgeia.com/mcp
```

[![Install in Cursor](https://cursor.com/deeplink/mcp-install-dark.svg)](https://cursor.com/install-mcp?name=opendiavgeia&config=eyJ1cmwiOiJodHRwczovL29wZW5kaWF2Z2VpYS5jb20vbWNwIn0%3D)
[![Install in VS Code](https://img.shields.io/badge/VS_Code-Install_server-0098FF?logo=visualstudiocode&logoColor=white)](https://insiders.vscode.dev/redirect/mcp/install?name=opendiavgeia&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fopendiavgeia.com%2Fmcp%22%7D)

It needs a free opendiavgeia account. The first time you connect, you sign in with Google or a code sent to your email, and the account is made then.

## Connect

**Claude** (web and desktop): open Customize → Connectors, press Add → Add custom connector, give it a name such as opendiavgeia, paste the address and press Add. Then press Connect and sign in.

**Claude Code:** install the plugin, which adds the server and the [skill](#skill):

```
/plugin install opendiavgeia --marketplace opendiavgeia/mcp
```

or only the server:

```bash
claude mcp add --transport http opendiavgeia https://opendiavgeia.com/mcp
```

**Gemini CLI:**

```bash
gemini extensions install https://github.com/opendiavgeia/mcp
```

**Cursor and VS Code:** use the buttons above, or add it to `mcp.json`:

```json
{
  "mcpServers": {
    "opendiavgeia": { "url": "https://opendiavgeia.com/mcp" }
  }
}
```

VS Code's file uses `"servers"` in place of `"mcpServers"` and wants `"type": "http"` next to the address.

**ChatGPT and other clients:** add the same address as a remote MCP server (Streamable HTTP). The client opens a sign-in page; there is no key to copy.

## What you can ask

- How much did the Municipality of Athens pay ΔΕΗ in 2019?
- Which bodies paid COSMOTE the most in 2024?
- Which municipalities in Crete spent the most in 2024?
- Which decisions of the Municipality of Patras are about street lighting?
- Which IT tenders in Crete close this week?
- Which open tenders for road works have a budget over €1 million?

Questions work in Greek or English, and names in Greek or Latin letters.

## Tools

All eleven only read.

| Tool | What it returns |
| --- | --- |
| `find_body_or_place` | Public bodies (ministries, municipalities, hospitals, universities) and places (regions, municipalities) by name, with their uid |
| `find_payee` | A person, company or organisation that was paid or awarded something, by name or ΑΦΜ, most paid first |
| `body_spending` | What a body paid per year; for one year, its largest payees and its monthly totals |
| `place_spending` | What a municipality or region paid per year; for one year, its bodies and payees that paid or were paid most |
| `rank_places` | Regions or municipalities ranked by what they paid in a year |
| `payee_profile` | What one ΑΦΜ was paid per year, which bodies paid it most, and the contracts it won |
| `search_decisions` | Decisions by words, ΑΔΑ, type and date, within one body or across all of them |
| `decisions_between` | The decisions one body issued that name one ΑΦΜ, newest first |
| `get_decision` | One decision by its ΑΔΑ: subject, type, body, amount, parties and the decisions it follows from |
| `direct_award_threshold` | Direct awards counted in €200 bands up to and past the €37,200 limit above which a tender is required |
| `find_tenders` | Open tenders from ΚΗΜΔΗΣ and TED, closing first, by words, kind of work (CPV), region, budget, procedure and deadline, with buyer, budget and deadline |

## Skill

[`skills/greek-public-spending`](skills/greek-public-spending/SKILL.md) tells an agent how to answer from this data: names before figures, payments and awards kept apart, a link on every record, and care with private people. It works with the MCP tools or, without them, from the site's own pages. The Claude Code plugin and the Gemini extension include it; for other agents:

```bash
npx skills add opendiavgeia/mcp
```

## About the data

- Amounts are in euros. Payments (money paid out) and contract awards (money agreed) are kept apart and never added together.
- Totals run to the end of the latest month held in full. A year still running is partial, and each result says what it covers.
- Open tenders come from ΚΗΜΔΗΣ, the Greek public procurement register, and TED, the EU's tenders journal. Each tender's page on the site shows what its buyer paid for similar work since 2010.
- Decisions come from Diavgeia's open API and, back to 2010, from the [glossAPI/diavgeia](https://huggingface.co/datasets/glossAPI/diavgeia) archive (CC BY 4.0). [How amounts are counted](https://opendiavgeia.com/about).
- Patterns such as awards just under the tender limit are worth a look, not proof of wrongdoing.

## Limits and privacy

- 60 calls a minute per account, across all your clients.
- The server keeps which tools ran, not your conversation. [Privacy](https://opendiavgeia.com/privacy) · [Terms](https://opendiavgeia.com/terms)

## Feedback

Open an [issue](https://github.com/opendiavgeia/mcp/issues) for a wrong figure, a missing tender, a name the tools can't find or a client that won't connect, or write to contact@opendiavgeia.com.

This repository holds the server's documentation and its [MCP Registry](https://registry.modelcontextprotocol.io) entry, `server.json` (`com.opendiavgeia/diavgeia`). opendiavgeia is not affiliated with diavgeia.gov.gr or any public authority.
