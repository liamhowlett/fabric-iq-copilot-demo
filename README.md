# Fabric IQ + Microsoft 365 Copilot — One Question, Three Surfaces

**Summary**: A demo showing Fabric IQ and Work IQ working together across Microsoft 365 Copilot (BizChat), Copilot Cowork, and GitHub Copilot — where the same finance question gets the same answer on every surface, and ends up creating a governed metric that didn't exist before.

> [!NOTE]
> **You don't have to build all of it.** Recreating the full demo end to end — including the seeded meetings, chats and documents — takes real effort and a properly licensed tenant. But you don't need all of that to see the value.
>
> **Publish the semantic model and report, upload the three documents, and you already have a compelling demo.** You'll get grounded data answers across Copilot Chat and Cowork, and the document layer gives Copilot enough context to explain the numbers. The Teams call and chat threads are what make the work-graph reveal land hardest, so add them when you have time.
>
> Microsoft employees: the [internal setup guide](https://microsofteur-my.sharepoint.com/:w:/g/personal/lihowlet_microsoft_com/IQAYSCc086h_Q6S8p6K5lXijAQrStoJJe-HnU-eDlUKk6vo?e=SeGSP3) walks through standing up an MCAPS tenant from scratch.

---

## 💡 What Did I Create and Why?

I created this demo (alongside Product Group — thanks to Carly Newsome and Sara Vredevoogd for the direction) because I genuinely think this is one of the most important BI developments I've seen in a long time. Every single person in every organisation that has Power BI and M365 Copilot — which is a lot of organisations — should be aware of this. Here are some of the reasons I think it's so powerful:

- **The analysis is incredible.** Not just running queries, pulling numbers and creating stunning visuals, but the way it tells the story too.
- **Work IQ integration means you have more than just numbers.** You have all the business context in your M365 ecosystem to augment those insights. Anecdotal information captured in a 1:1 chat, not a data platform, is now an easily accessible data point.
- **What's the use in doing BI if you're not going to act on it?** In Cowork, you can do so from the same environment.
- **The semantic model.** The story below shows how a non-deterministic engine gets the same answers across multiple interfaces. This would not be possible without the most mature semantic layer in the market.

The demo pivots around a single question: **how did Q1 land?**

A finance director asks it in Teams. An analyst picks it up in Cowork. An analytics engineer finishes it in GitHub Copilot. The number is identical every time. What changes is what each person can *do* with it — and by the end, a metric that only existed in people's heads ("operating cost ratio by market") has become a governed, documented measure in the model.

The design decision that makes this work is worth calling out, because it's the thing most demos get wrong:

> **Fabric IQ owns every number. Work IQ owns the story.**
>
> The seeded emails, chats and documents in this demo contain **no figures at all** — no percentages, no currency, no ratios. They only carry the human context: *why* a supplier negotiation collapsed, *why* a building sat empty, *why* one cost line is permanent and the rest aren't.
>
> If you seed your work graph with the same numbers the model already computes, the big reveal is just Copilot repeating itself. Keep them separate and the two halves genuinely add up to something neither could give you alone.

https://github.com/user-attachments/assets/78fd9363-857a-4c54-9223-de4bfe6e245a

_Demo: the same question answered across BizChat, Cowork and GitHub Copilot_

---

## 🧠 Architecture and Components

1. **Fabric IQ / semantic model** — a synthetic consumer-goods P&L ("Finance State of the Nation"). Five years of revenue, OpEx, budget and cash flow across 18 countries. This layer answers every *numeric* question.
2. **Work IQ / the work graph** — a recorded Teams call with transcription on, five Teams chat threads, and three Word documents seeded into the tenant. This layer answers every *why* question.
3. **Three Copilot surfaces** consuming both:
   - **Microsoft 365 Copilot (BizChat)** — the finance director spots the problem
   - **Copilot Cowork** — the analyst investigates it and produces artefacts
   - **GitHub Copilot** + Power BI MCP servers — the engineer governs the metric

The story hinges on one deliberate gap: **`Operating Cost Ratio by Market` does not exist in the model.** Ops calculate it by hand, finance calculate it differently, and the last board pack carried two different figures for the same market. Act 3 fixes that live.

---

## ✅ Prerequisites

### The tenant

Run this in a tenant where **Microsoft 365 Copilot is genuinely licensed and provisioned**. Copilot needs to be properly set up with real mailbox and Teams data behind it — a tenant that has never had Copilot enabled won't return anything, however many times you re-authenticate.

> [!NOTE]
> **Microsoft employees:** there's a companion guide covering how to stand this environment up from scratch in an MCAPS demo tenant — requesting the tenant, the DARSy licence request, Cowork billing, and the tenant settings that need switching on. **[Internal setup guide →](https://microsofteur-my.sharepoint.com/:w:/g/personal/lihowlet_microsoft_com/IQAYSCc086h_Q6S8p6K5lXijAQrStoJJe-HnU-eDlUKk6vo?e=SeGSP3)**

### Accounts

You need **three user accounts** in that tenant:

| Role in the demo | Who they are | Notes |
| --- | --- | --- |
| **You** (the presenter) | Your own account | The **only account you sign into**. You appear in every call and chat, so Act 1 surfaces the whole set |
| **Supporting account 1** | An ops/PMO colleague | Sends the key chat that explains the problem |
| **Supporting account 2** | A second ops voice | Sends the pre-read and the post-mortem |

Three on-screen personas (a finance director, an analyst, an engineer) are **narration labels only** — you stay signed in as yourself and describe the handoffs. Don't create accounts for them.

### Software and licensing

- Microsoft 365 Copilot licences
- Access to **Microsoft Copilot Cowork**
- **GitHub Copilot** (CLI or VS Code) with the Power BI MCP servers — both the **remote** server (queries a published model) and the **local** server (writes to a model open in Desktop)
- **Power BI Desktop** (latest)
- A **Power BI / Fabric workspace** to publish into

---

## 🔹 Step 1: Load the data and the model

The model reads from CSV files in this repo, so it will work from any folder — but you have to tell it where they are once.

- Clone or download this repo somewhere sensible
- Open **`Finance State of the Nation Demo Template.pbit`** in Power BI Desktop
- It will prompt you for **`csvFilePath`**. Paste the full path to the `data` folder in your clone, for example:

  ```
  C:\repos\fabric-iq-copilot-integration-repo\data
  ```

- Let it refresh. You should end up with 11 tables and 30 measures

> [!NOTE]
> The data is synthetic — generic brands, generic countries, no customer reference. It's fixed, so everyone sees the same numbers, and the figures quoted in the demo script will match what's on your screen.

### Check the numbers before you go further

Filter to **FY2026 Q1** and confirm you see roughly this:

| Market | Operating margin | Operating cost ratio |
| --- | --- | --- |
| **UK** | **−51.0%** | **113.1%** ← the outlier the whole demo hangs on |
| US | −42.4% | 104.6% |
| China | −27.0% | 89.4% |
| India | −6.9% | 69.1% |
| **Blended** | **−4.95%** | Revenue ~$130.7M, gross margin ~62% |

If the UK isn't the worst market, something's wrong with your refresh — stop and fix it, because Act 1 doesn't land without it.

> [!WARNING]
> **Pin your report pages to FY2026 Q1.** Unfiltered, the model shows operating income at **+$36.5M** across all five years, which completely undercuts the story. The problem quarter only appears when you filter.

---

## 🔹 Step 2: Confirm the metric gap

This is the payoff of Act 3, so verify it's genuinely missing before you record anything.

- In Power BI Desktop, search the measures list for *"cost ratio"*
- **You should find nothing.** The model has `Operating Margin %` but no cost-ratio measure

Here's the DAX you'll create live in Act 3 — keep it handy as an answer key, but **do not add it to the model yet**:

```dax
Operating Cost Ratio by Market =
DIVIDE (
    [Total OpEx],
    [Total Revenue]
)
```

Supporting measures Copilot will typically suggest alongside it:

```dax
OpEx Growth QoQ % =
VAR CurrentOpEx = [Total OpEx]
VAR PriorOpEx =
    CALCULATE ( [Total OpEx], DATEADD ( 'Date'[Date], -1, QUARTER ) )
RETURN
    DIVIDE ( CurrentOpEx - PriorOpEx, PriorOpEx )
```

```dax
Cost to Serve =
DIVIDE (
    [Total OpEx],
    [Units Sold]
)
```

---

## 🔹 Step 3: Publish the model and report

- Publish to your Fabric/Power BI workspace
- Note the **semantic model ID** from the URL — you'll need it for the GitHub Copilot act, which connects to the published model by ID over the remote MCP server
- Confirm Copilot can see the report in Microsoft 365 (this is what Act 1's first prompt queries)

> [!TIP]
> **The report needs time to index too, but you can work around it.** Copilot's search for Power BI content isn't instant — a freshly published report may not be findable by name, and it may not appear in the **+** attachment menu straight away.
>
> To test before it's indexed, **give Copilot the URL directly**. Open the report in the browser and copy the full address from the address bar, then paste it into your prompt. Report *share* links don't work — you need the resolved, long-form URL.
>
> The first query against a report is also slower than the ones that follow, while Copilot indexes the content. Don't read that as a failure.

---

## 🔹 Step 4: Seed the work graph

This is the step people underestimate. It's also the step that makes the demo worth watching.

Open **`seed-content\START HERE - Seeding Guide and Call Script.docx`** — it walks you through this step in detail, and contains the call transcript and chat threads you'll stage.

The folder is split so it's obvious what goes where:

| | What it is | What to do with it |
| --- | --- | --- |
| `START HERE - Seeding Guide and Call Script.docx` | The setup guide, plus the call transcript and four Teams chat threads | **Read and perform it.** Don't upload it |
| `upload-to-tenant\` | Three finance documents — EMEA cost commentary, OpEx drivers, and the Project Helena closeout memo | **Upload all three** to SharePoint or OneDrive |

**What to do:**

1. **Pick your three accounts.** You present as one of them; two colleagues play Operations. The guide explains which names to swap and why only the presenter slot is load-bearing.
2. **Hold the call.** Read the transcript aloud on a Teams call with **transcription switched on** — the transcript is what Copilot actually retrieves. Natural delivery beats a polished one.
3. **Post the chat threads.** Send each from the right account, as ordinary Teams messages. Your presenting account is a participant in all of them.
4. **Upload everything in `upload-to-tenant\`** to SharePoint or OneDrive somewhere your account can reach.
5. **Wait.** Indexing is not instant — give it **a few hours, ideally overnight**, before you test.

> [!NOTE]
> Teams messages can't be backdated. Everything will be timestamped when you post it, which is fine — it's indexing that matters, not the date on screen.

**Test before you rely on it.** In Microsoft 365 Copilot, ask something like *"what have operations said recently about UK costs?"* If nothing comes back, wait longer. If it still doesn't work the following day, the cause is more likely the tenant than the content — see the prerequisites.

---

## 🔹 Step 5: Run the demo

Everything you need is in **[DEMO-SCRIPT.md](DEMO-SCRIPT.md)** — the prompts in order, what should appear on screen, and what to say while it renders.

Rough shape, about 20 minutes:

| Act | Surface | Runs |
| --- | --- | --- |
| Cold open | — | 45s |
| **Act 1** | Microsoft 365 Copilot (BizChat) | 4–5 min |
| **Act 2** | Copilot Cowork | 6–7 min |
| **Act 3** | GitHub Copilot + Power BI MCP | 6–7 min |
| Close | Any surface | 1 min |

---

## 📁 What's in this repo

```
├── README.md                                             You are here
├── DEMO-SCRIPT.md                                        Prompts, screen checks and talk track
├── model/
│   └── Finance State of the Nation.pbit                  Model + report, prompts for the CSV path
├── data/
│   └── *.csv                                             10 dimension and fact tables
└── seed-content/
    ├── START HERE - Seeding Guide and Call Script.docx   Read this — setup guide + call script
    └── upload-to-tenant/                                 Upload all three to SharePoint/OneDrive
        └── *.docx                                        Cost commentary, OpEx drivers, Helena memo
```

---

## 🧯 Troubleshooting

| Problem | Cause | Fix |
| --- | --- | --- |
| `AADSTS650052` on Copilot sign-in | Copilot isn't provisioned in the tenant | Use a tenant where Microsoft 365 Copilot is properly licensed and in use |
| Copilot finds no meetings, chats or docs | Not indexed yet, or you're not a participant | Wait longer. Confirm your account is in every conversation |
| Copilot can't find the report by name | Report not indexed yet, or it's in an org app | Paste the full report URL from the browser address bar. Share links don't work |
| Operating income is **positive** | Report isn't filtered | Pin pages to FY2026 Q1 |
| `Revenue YoY %` shows ~26% everywhere | Time intelligence across the unfiltered five-year range | Apply a period filter |
| Template won't refresh | `csvFilePath` wrong | Re-enter under *Transform data → Edit parameters* |
| UK isn't the worst market | Report isn't filtered to FY2026 Q1 | Check the filter and re-check against the Step 1 table |
| Cash Flow visuals look alarming | Cash Flow is negative by design and not demo-safe | Leave it off screen |

---

## 🙏 Notes

The data is entirely synthetic — generic brands, generic countries, no customer reference. The finance narrative (a failed supplier renegotiation, a building that failed its fire sign-off, a sustainability-reporting hire) is invented, but deliberately mundane, because that's what makes it land with a finance audience.

If you rebuild this with your own story, the one rule worth keeping is the split: **let the model own the numbers and let the work graph own the reasons.** Everything else is negotiable.
