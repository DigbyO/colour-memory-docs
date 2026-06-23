# Colour Memory

Every other colour API tells you what colours go together. Colour Memory tells you what they mean, where they came from, how strong the evidence is, and what an AI must not claim.

Pass it a hex value. It returns colour science, the nearest named archive evidence, a source, an evidence grade, and explicit constraints on what may safely be asserted. The result is called a **Colour Passport**.

This repository contains runnable integration examples, response schemas, and MCP configuration for the hosted Colour Memory API. The production service and proprietary archive are maintained in private infrastructure.

---

## Try it now

No signup required for the first few calls.

```bash
curl -X POST https://api.colourmemory.com/query/hex \
  -H "Content-Type: application/json" \
  -d '{"hex": "#8B1A1A", "n_results": 1}'
```

Example response:

```json
{
  "name": "Tyrant_Crimson_Proscription_Lists",
  "hex": "#8B1A1A",
  "archive": "DarkHistory",
  "primary_source": "Roman proscription records, 82 BCE, Sulla dictatorship",
  "claim_strength": "C",
  "claim_role": "anchor",
  "do_not_say": [
    "Do not claim this hex is a spectrophotometrically measured historical colour.",
    "Do not assert this shade was universally used to mark proscription documents."
  ]
}
```

The archive association is documented. The stored hex is an editorial estimate from historical description. Colour Memory preserves that distinction so an AI does not turn a historical reference into a false measurement claim.

For full access, get an API key at [colourmemory.com/start](https://colourmemory.com/start).

---

## What the Colour Passport returns

The `/colour/passport` endpoint is the canonical single call. Every other tool derives from the same object.

```bash
curl -X POST https://api.colourmemory.com/colour/passport \
  -H "X-Api-Key: YOUR_KEY" \
  -H "Content-Type: application/json" \
  -d '{"hex": "#4A3728", "include_cultural": true, "include_physics": true}'
```

The response separates four layers that other colour systems conflate:

**Colour science** -- hex, Lab, LCh, LRV, chroma, hue angle, WCAG contrast ratios. Deterministic.

**Archive evidence** -- nearest named colour, primary source, institution, evidence grade A through E. Graded.

**Interpretation** -- cultural associations by market, with scope, period, and counterexample warnings. Labelled as interpretation.

**Generated content** -- copy, strategy, editorial argument. Constrained by layers one through three.

---

## Evidence grades

| Grade | Meaning |
|---|---|
| A | Direct institutional record |
| B | Named documentary source |
| C | Named secondary source |
| D | Cultural or semantic association |
| E | Poetic or literary inference only |

Every endpoint exposes the claim strength of its archive anchor and returns `do_not_say` constraints. Agents must respect these. They are not suggestions.

---

## MCP integration

Colour Memory exposes tools via the Model Context Protocol. Connect any MCP-compatible client using the URL below.

**Claude, Cursor, Cline, and other MCP clients:**

```
https://api.colourmemory.com/mcp?api_key=YOUR_KEY
```

Add that URL as a remote MCP connector. The key goes in the query string -- no separate auth header is needed for MCP connections.

**Verify the connection:**

Call `meta_capabilities`. If `colour_passport` appears in `mcp_tool_names`, the connection is working.

---

## REST API

Base URL: `https://api.colourmemory.com`

Authentication: `X-Api-Key: YOUR_KEY` header on all REST requests.

### Core endpoints

| Endpoint | Method | Description |
|---|---|---|
| `/colour/passport` | POST | Canonical colour truth object |
| `/query/hex` | POST | Nearest archive colours by perceptual distance |
| `/query/conceptual` | POST | Semantic archive search by concept or question |
| `/colour/{name}` | GET | Full data for a named archive colour |
| `/colour/timeline` | POST | Trace a colour concept through history |
| `/compare-colours` | POST | Deep perceptual and cultural comparison |
| `/colour/cultural-risk` | POST | Cultural associations and warnings by market |
| `/colour/strategy` | POST | Commercial colour strategy with archive grounding |
| `/archive/provenance` | POST | Evidence audit for a named archive colour |
| `/archive/evidence-gap` | POST | What source would be needed to support a claim |
| `/palette/concept` | POST | Historically grounded palette from a concept |
| `/palette/gradient` | POST | Perceptually smooth gradient between archive anchors |
| `/accessibility/matrix` | POST | Full WCAG 2.1 matrix for a palette |
| `/interior-specification` | POST | Complete room specification from a concept |
| `/ecommerce/product-copy` | POST | Archive-grounded product copy |
| `/agent/brief` | POST | Colour direction package for image generation |
| `/agent/verify-palette` | POST | Verify a generated image used the specified colours |

Full reference: [colourmemory.com/docs.html](https://colourmemory.com/docs.html)

---

## Hex provenance

Every result includes a `hex_provenance` field with one of four statuses:

- `measured_object` -- physically measured from a known object
- `manufacturer_specification` -- published manufacturer value
- `historical_description_approximation` -- editorial estimate from documented description
- `speculative` -- derived from cultural or semantic context

A perceptual distance of zero means the submitted hex matches the stored archive estimate exactly. It does not mean the colour has been verified against a physical historical object. The passport states this explicitly so agents cannot conflate the two.

---

## Pricing

Two kinds of usage: archive calls (search, match, retrieve) and Intelligence credits (AI-powered analysis, briefs, copy generation). Archive tools continue working even if Intelligence credits run out.

| Plan | Price | Archive calls | Intelligence credits |
|---|---|---|---|
| Free Explorer | Free, no signup | 25 | 3 |
| Free Developer | Free, email required | 500 | 10 |
| Studio | £12/month | 2,000 | 50 |
| Pro | £39/month | 10,000 | 200 |
| Enterprise | £299/month (founder rate) | 50,000 | 500 |

Annual billing available. Credit packs and one-off reports also available without a subscription.

Current pricing: [colourmemory.com/pricing](https://colourmemory.com/pricing)

---

## Repository contents

```
examples/          Runnable integration examples
docs/              Extended documentation
README.md          This file
```

Integration examples in this repository are MIT licensed. The archive, hosted intelligence layer, and API outputs are proprietary. See [colourmemory.com](https://colourmemory.com) for commercial terms.

---

## Who builds with it

**AI agent developers** -- evidence-grounded colour reasoning, hallucination constraints, cultural risk checks before generation.

**Design system teams** -- archive-anchored colour tokens, palette audit and governance, accessibility validation.

**Heritage and museum projects** -- provenance research, period-accurate palette generation, anachronism detection.

**Ecommerce** -- archive-grounded product naming and copy, colour strategy with market-specific cultural reading.

**Image generation workflows** -- colour briefs for Midjourney, Flux and DALL-E, post-generation palette verification.

---

## Contact

hello@colourmemory.com

[colourmemory.com](https://colourmemory.com)

---

*Colour Memory is a product of PR Eye Ltd, Oxfordshire.*
