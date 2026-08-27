# Demo Script

Everything you need to run the demo, in order. Each act has the **exact prompts** to paste, what should appear on screen, and what to say while it renders.

**Total runtime:** ~20 minutes.

---

## Before you start

- [ ] Signed in as **your own account** on all three surfaces — the personas are narration only
- [ ] Report pinned to **FY2026 Q1**
- [ ] `Operating Cost Ratio by Market` confirmed **absent** from the model
- [ ] Seed content posted and **verified as indexed** (ask Copilot about UK costs and check it answers)
- [ ] Power BI Desktop open with the model loaded, for Act 3
- [ ] Semantic model ID to hand, for the remote MCP connection in Act 3
- [ ] Reference numbers on a sticky note:

  > Revenue ~$130.7M flat · Gross margin ~62% · **UK −51.0% operating margin, 113.1% cost ratio (worst)** · US −42.4% · China −27.0% · Blended −4.95%, weakest Q1 in four years

> [!IMPORTANT]
> **The split to keep in your head all the way through:** the model gives you the numbers, the work graph gives you the reasons. When you get to Act 1 Scene 3, read the figures off the screen and let the chats supply the *why*. Don't let them blur together — the separation is the whole point.

---

## Cast

| On screen you'll say | Actually signed in as | Why |
| --- | --- | --- |
| "Maya, a finance director" | You | Act 1, in BizChat |
| "Priya, an analyst" | You | Act 2, in Cowork |
| "Tom, an analytics engineer" | You | Act 3, in GitHub Copilot |

Two supporting accounts appear inside the seeded chats and the recorded call. Rename all of these to suit your own tenant.

---

## Cold open · 45 seconds

**Say:**

> "One finance question — *how did Q1 land?* — asked by three different people, in three different tools. A finance director in Teams. An analyst in Cowork. An engineer in GitHub. Same data, same skills underneath. Watch the answer stay identical while the canvas gets richer and the work actually gets done."

---

## Act 1 · Microsoft 365 Copilot (BizChat) · 4–5 min

*Set-up line: "She lives in Teams and Outlook. She never opens Power BI."*

### 1.1 — Find the problem

```
Give me a brief executive summary of the Finance State of the Nation Power BI report with visuals. Let me know how Q1 landed across the P&L compared with recent quarters, and flag anything that's out of step with where we've been?
```

**On screen:** Revenue ~$130M flat, gross margin ~62%, operating income the weakest Q1 in four years.

**Say:** "Notice she didn't ask for a dashboard — she asked a question. The problem surfaces on its own."

### 1.2 — Narrow it down

```
Focusing on the operating cost issue, break operating margin down by market so we can use the data to identify where the issue is exaggerated.
```

**On screen:** UK stands out at ~−51%, the worst market. US ~−42%. Growth markets positive.

**Say:** "The UK is dragging the headline. That's where it's exaggerated — and that's the thread we pull."

### ★ 1.3 — The work graph reveal

```
Are there any insights we can take from recent meetings, communications or documents with the operations department that can help provide more context on what's happening in the UK?
```

**On screen:** Copilot surfaces the seeded Teams chats, the call transcript, and the three finance documents.

**Say — this is the moment that matters:**

> "This is the work graph. The model gave us −51%. The chats and documents tell us **why** — a supplier walked away weeks before signing, a new site failed its fire sign-off so we're paying double rent, a marketing push didn't convert. Almost all of it one-off. Only the sustainability-reporting hire is permanent. This isn't the core business failing, and you cannot get that from a P&L."

> [!TIP]
> Let this one breathe. If you rush it, the whole demo is just three chatbots agreeing with each other.

### 1.4 — Delegate

```
Draft a Teams message to Priya Nair asking her to dig into the UK operating cost ratio, and set up a 30-min review with the relevant people.
```

**On screen:** Draft Teams message plus a meeting invite, attendees pulled from the org graph.

**Say:** "She delegates without leaving the chat. That's the handoff into Act 2."

---

## Act 2 · Copilot Cowork · 6–7 min

*Set-up line: "Priya picks this up in Cowork — same engine, an analyst's canvas."*

### ★ 2.1 — Same question, same answer

```
Give me a brief executive summary of the Finance State of the Nation report with visuals — how Q1 landed across the P&L versus recent quarters.
```

**On screen:** Identical numbers to Act 1.

**Say:** "Same question, same answer, different surface. That consistency *is* the product."

### 2.2 — Find the ungoverned metric

```
What do recent meetings, emails, and chats say about UK operating cost by market?
```

**On screen:** The ops chats and call transcript, revealing that "operating cost ratio by market" is something ops already track — by hand, and inconsistently with finance.

**Say:** "The metric already exists. Informally. Ops calculate it one way, finance another, and the last board pack had two different figures for the same market. Nobody owns it, so it drifts."

### ★ 2.3 — Four artefacts, one prompt

```
Ok. 1) draft an email for me to share my findings, 2) book a 30-min follow-up with the Operations department on Monday if available, 3) create a rich html dashboard tracking the story around operating cost ratio by market and schedule it to run every week and give me next best action suggestions, 4) build a short slide deck on the UK operating-income story
```

**On screen:** An email draft, a booked meeting, a scheduled HTML dashboard with recommended actions, and a slide deck.

**Say:** "Four things, one prompt. And the dashboard isn't a one-off — it's scheduled, and it suggests what to do next."

> [!TIP]
> This takes a while to render. Don't fill the silence with apologies — narrate *why* it matters: governance, reuse, the fact that nobody has to rebuild this next quarter.

### 2.4 — Hand off to engineering

```
Send a Teams message to Tom Brennan: Operating Cost Ratio by Market is getting a lot more focus so we need to make sure the model is governed in a way that ensures consistency, plus any related metrics. It also seems to be drifting towards analysing by region rather than market (country).
```

**On screen:** A drafted Teams message framing the metric as ungoverned and drifting.

**Say:** "The metric needs to become real. Handoff to Act 3."

> [!NOTE]
> That last line about region-versus-market isn't padding — it sets up the AI instruction in Act 3.

---

## Act 3 · GitHub Copilot + Power BI MCP · 6–7 min

*Set-up line: "Tom makes it real — in VS Code, with the model wired in over MCP."*

### 3.1 — Pick up the context

```
Use the Work IQ MCP to check my most recent Teams message from Priya to help set the context for the session
```

**On screen:** The message from Act 2 arrives as context.

**Say:** "Same work graph — now in a developer's tooling."

### 3.2 — Connect to the published model

```
Connect to the Finance State of the Nation Power BI model (YOUR-SEMANTIC-MODEL-ID) using the remote MCP server and show me how 2026 Q1 actuals compare to the previous quarter. Give me a broad picture across the key financials
```

> [!IMPORTANT]
> Replace `YOUR-SEMANTIC-MODEL-ID` with the GUID from your published model's URL.

**On screen:** The same Q1 figures, this time straight from the published model.

**Say:** "Same numbers the finance director and the analyst saw — from the raw model this time."

### 3.3 — Confirm the gap

```
List the measures and tables in this model, is there an existing operating-cost-ratio measure?
```

**On screen:** 30 measures, none of them a cost ratio.

**Say:** "Confirmed net-new. We're not duplicating something that already exists — we're filling a real gap."

### ★ 3.4 — Generate and govern

```
Ok, using the local semantic modelling MCP, connect to Finance State of the Nation in Power BI Desktop. Recommend and generate the DAX for a governed "Operating Cost Ratio by Market" measure, plus supporting measures (OpEx growth QoQ, cost-to-serve). Then add a semantic model AI instruction that helps the AI understand that end users use the terminology 'market' when they're referring to country, not region.
```

**On screen:** DAX generated and written into the live model, plus an AI instruction added to the semantic model.

**Say:**

> "Two things happened there. It recommended the *related* metrics, not just the one I asked for — that's Copilot as an analytics partner rather than a code generator. And it wrote an AI instruction into the model itself, so the next person who asks about 'market' gets country, not region. We just fixed the ambiguity that caused the drift in the first place."

### 3.5 — Close the governance loop

```
Generate an updated data dictionary that I can copy into our CoE portal: every measure, its definition, and dependencies, as markdown. Save it as a session artifact, then open that file in the side panel.
```

**On screen:** A markdown data dictionary covering every measure and its dependencies.

**Say:** "Discoverable, documented, maintained. The metric went from a number in someone's spreadsheet to a governed asset in about four minutes."

---

## Close · 1 min

**Do:** re-ask the summary prompt on whichever surface is still on screen.

**Say:**

> "We started with one question in Teams. It travelled through Cowork and into the model, and the answer never changed. Same data, same definition, same answer — everywhere. The difference is that now the metric is governed, the work is done, and the next person who asks gets the same truth."

---

## The five moments that must land

If you're running short, protect these and cut the connective tissue:

1. **Act 1.3** — the work graph explains what the P&L can't
2. **Act 2.1** — same answer, different surface
3. **Act 2.3** — four artefacts from one prompt
4. **Act 3.4** — Copilot recommends related measures *and* writes the AI instruction
5. **The close** — same data, same definition, same answer

---

## If someone asks…

**"Is this real data?"**
No — entirely synthetic, generic brands and countries. The point is the pattern, not the numbers.

**"Could Copilot have just made the −51% up?"**
No. It comes from the semantic model via Fabric IQ. That's exactly why the seeded chats and documents deliberately contain no figures — so you can see which layer is doing what.

**"What if our metric definitions already exist?"**
Even better. The Act 3 point isn't creating a measure from nothing, it's that an ambiguous metric got a single governed definition, an AI instruction to stop it drifting, and documentation — in one pass.

**"How long did the seeding take?"**
A call, five chat threads and three documents. Under an hour of work, plus overnight for indexing.
