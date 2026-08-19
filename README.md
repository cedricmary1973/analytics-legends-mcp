<!-- mcp-name: ai.analyticslegends/sap-analytics -->

# Analytics Legends — SAP Analytics MCP Server

A **read-only** [Model Context Protocol](https://modelcontextprotocol.io) server over a
curated corpus of the **SAP analytics services market**: who delivers SAP Datasphere,
Business Data Cloud, SAP Analytics Cloud and BW/4HANA work, what the contract market
looks like, what the day rates are, what the vocabulary means, and what has just
happened.

It exists so that an assistant answering an SAP analytics question can **cite a record
instead of guessing**. Every row carries a `citation_url` on analyticslegends.ai, and
every response carries the size of the population it was drawn from.

**Nothing to install.** The server is hosted:

```
https://analyticslegends.ai/mcp        streamable-http
```

The public tools need no credential.

---

## Connect

<details open>
<summary><b>Claude Code</b></summary>

```bash
claude mcp add --transport http analytics-legends https://analyticslegends.ai/mcp
# with a subscriber key, to open the six gated tools:
claude mcp add --transport http analytics-legends https://analyticslegends.ai/mcp \
  --header "Authorization: Bearer alk_…"
```
</details>

<details>
<summary><b>Claude web, Desktop and mobile</b> (custom connector)</summary>

Add a custom connector pointing at `https://analyticslegends.ai/mcp`. The public tranche
answers immediately, with no credential.

To open the paid tranche, use the connector's **request-header authentication** and set
`Authorization` to `Bearer alk_…`. The credential is entered once, by whoever adds the
connector, and is shared by that workspace — one key, one daily quota. There is no OAuth
consent flow to walk: this server's unit of access is a personal API key minted on an
analyticslegends.ai account, not a delegated identity.
</details>

<details>
<summary><b>Claude Desktop / any client reading a JSON config</b></summary>

```json
{
  "mcpServers": {
    "analytics-legends": {
      "type": "streamable-http",
      "url": "https://analyticslegends.ai/mcp"
    }
  }
}
```
</details>

<details>
<summary><b>With a subscriber key</b> (opens the six gated tools)</summary>

```json
{
  "mcpServers": {
    "analytics-legends": {
      "type": "streamable-http",
      "url": "https://analyticslegends.ai/mcp",
      "headers": { "Authorization": "Bearer alk_…" }
    }
  }
}
```
</details>

The manifest is served at
[`/.well-known/mcp.json`](https://analyticslegends.ai/.well-known/mcp.json) and mirrored
in this repository as [`mcp.json`](./mcp.json). `tools/list` is the authority for the
surface — this README is a copy, and a copy can drift.

---

## The 20 tools

**Fourteen are public and unauthenticated.** Six need a subscriber key; they are
**listed and refuse by name**, never hidden — an agent can see what it is missing.
Three open at Consultant, three at Legend, and the MCP Pass opens all six.

| Tool | Answers |
|---|---|
| `search_firms` | Which SAP analytics providers match a country, kind, free text |
| `count_firms_by` | A counting question in one call, instead of a page of rows |
| `get_firm` | One organisation from the published directory |
| `list_firm_kinds` | The directory's breakdown by organisation kind, with live counts |
| `find_opportunities` | The live contract and permanent-role radar |
| `search_news` | The curated SAP analytics news corpus |
| `search_concepts` | The concept encyclopaedia — the vocabulary of the stack |
| `get_concept` | One concept: title, category, level, tags, editor's summary |
| `list_studies` | The deep-research study editions and their metadata |
| `get_day_rate_benchmark` | The public day-rate aggregate by country, stack, seniority |
| `list_sap_modules` | The canonical SAP module/product taxonomy |
| `list_freelance_platforms` | The CV/profile platforms a consultant can sign up on |
| `find_academy_modules` | The Academy training catalogue, searchable by module |
| `query_knowledge_graph` | A traversal of the learning knowledge graph |
| 🔒 `get_concept_card` | The full encyclopaedia card — body, key points, cheat sheet |
| 🔒 `get_study` | One study **body** |
| 🔒 `get_academy_module` | One Academy module, in full |
| 🔒 `find_sap_clients` | The SAP end-customer corpus — who *runs* SAP |
| 🔒 `get_sap_client_profile` | One end-customer's SAP footprint and analytics estate |
| 🔒 `get_firm_intel` | A firm's practice size, partner level, delivery flags |

Every tool is annotated `readOnlyHint: true`. The server writes nothing, anywhere.

---

## Two rules it holds itself to

**A count is read at query time, and it comes with its population.** Responses carry
`_meta.match_count` (what your filters matched) and `_meta.tranche_total_row_count` (the
whole tranche). Quote those rather than any number written in a description — including
the ones in this file.

**A citation says what it is.** Every item carries `citation_url` **and**
`citation_scope`. `record` means the URL is that item's own page; `section_hub` means
this platform publishes no page for that item and the URL is the section index — say so,
or cite the section, but do not present a hub URL as the item's page. News items also
carry `source_url` for the upstream publisher: cite both.

---

## What is deliberately **not** served, at any tier

- **People.** Individual professionals are held as initials only. No tool returns
  anyone's identity or contact details.
- **The matchers.** Opportunity-to-consultant scoring stays on the platform.
- **SAP's own documentation.** Not ours to restate — cite SAP.

Retrieval with attribution is permitted. **Training-data use is not.**

---

## Subscriptions

The six gated tools open with an
[analyticslegends.ai](https://analyticslegends.ai/pricing/) subscription (Consultant or
above), or with the **MCP Pass** (€39.90/month) — the machine-access subscription: this
server's entire paid tranche, no web subscriber screens.

---

## Where the source lives

The platform's application repository is private: it holds a paid corpus and personal
data. **This repository is the server's public face** — its manifest, its registry
descriptor, its client configuration and this documentation. The server itself answers
at `https://analyticslegends.ai/mcp`, and the honest way to inspect it is to call
`tools/list` against it.

Health, with its own timestamp:
[`/api/mcp-health.json`](https://analyticslegends.ai/api/mcp-health.json) — read its
`generated_at` rather than assuming a cadence.

Published in the official MCP registry as **`ai.analyticslegends/sap-analytics`**.

## Licence

[MIT](./LICENSE) for the contents of this repository. The licence does not grant rights
over the data served by the endpoint, which stays under the platform's terms.
