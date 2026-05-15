# Prompt: Handoff-3 Homepage Refresh + PDF Regen

## Context

Flintwood.io is an Astro static site (`src/pages/index.astro`) deployed on Netlify. Design system: Instrument Serif + DM Sans, ember palette (`#c97030` primary), dark background (`#0d0c0a`). A new design handoff (v3) updates homepage copy and delivers a high-fidelity multi-page PDF report template.

---

## Task 1 — Homepage: update `src/pages/index.astro`

Apply the following changes. **Do not remove** the `#soc2-explainer` or `#sample-report` sections added in the previous PR — keep them in place between `#how-it-works` and `#contact`.

### Nav
No change.

### Hero

**H1:** "Your next audit will ask about your<br><em>AI stack.</em>"

**Subtitle** (`.hero-sub`, Instrument Serif italic 34px `#a8a298`): "We already checked."

**Body:** "Flintwood delivers automated monthly SOC 2 readiness reports for AWS — with a dedicated AI and agentic risk layer your auditor hasn't mapped yet."

**CTAs:**
- Primary: "Request free pilot report" → `#contact`
- Outline: "See how it works →" → `#how-it-works`

**Report card mockup (`.report-card`):**
- Score: **74**, progress bar `width: 74%`
- Score note: "↑ +6 pts since March" (green)
- Footer: "14 findings · 3 new this month" | "Human-reviewed ✓"
- Finding 1 Critical: `bedrock-agent-prod` has `bedrock:InvokeModel` on `*` with S3 write access and no MFA condition.
- Finding 2 High: Bedrock invocation logging disabled in production. No audit trail for model calls.
- Finding 3 Medium: 3 Lambda functions with AI permissions not tagged as AI workloads.

### Signal bar (keep 4 columns, update label text)

| Number | Label |
|---|---|
| Monthly | Continuous readiness — not a one-time snapshot |
| 5 days | First report delivered, free, no commitment |
| Read-only | Single IAM role — nothing runs in your account |
| AI-native | Bedrock, SageMaker & agentic Lambda risk layer |

### How it works

**Section title:** "Three steps.<br>No agents. No ongoing access."

| Step | Title | Body | Pill |
|---|---|---|---|
| 01 | Deploy a read-only IAM role | Deploy a single read-only IAM role via CloudFormation or Terraform in under five minutes. Nothing runs in your account — we assume the role monthly, then disconnect. | CloudFormation · Terraform · 5 min |
| 02 | We scan and map to SOC 2 TSC | Every month we scan your environment and map every finding to Trust Service Criteria — CC6, CC7, CC8 — plus a dedicated layer covering your AI and agentic workloads. | TSC-mapped · AI risk layer · Month-over-month drift |
| 03 | Receive a human-reviewed report | Every report is reviewed by a human before it reaches you. No raw scanner output. You get context, severity, and prioritized remediation — ready to share with your auditor. | Auditor-ready · Prioritized · No noise |

### AI Risk

**Eyebrow:** "Flintwood exclusive"

**Title:** "The AI risk layer your auditor doesn't have a checklist for yet."

**Body:** "As your AI stack grows, so does your audit surface. Bedrock roles, autonomous Lambda chains, unlogged model invocations — none of this is in the standard SOC 2 playbook. We track it before your auditor asks about it."

**Bullet list (6 items):**
1. Bedrock and SageMaker access control gaps
2. Agentic IAM role over-permission detection
3. Shadow AI usage via CloudTrail analysis
4. Missing human-in-the-loop controls
5. AI training data access logging gaps
6. Autonomous Lambda chain risk mapping

**Finding cards (unchanged structure, update descriptions):**
1. Critical · CC6.1 · "Over-permissioned Bedrock agent role" — `bedrock-agent-prod` has `bedrock:InvokeModel` on `*` with S3 write access and no MFA condition. Direct path to production data exfiltration.
2. High · CC7.2 · "Bedrock invocation logging disabled" — Model invocation logging is off in production. There is no audit trail for model calls — a gap that will surface in your SOC 2 review under CC7.2.
3. High · CC8.1 · "Autonomous Lambda chain without approval gate" — 2 Lambda functions form an autonomous chain that can invoke Bedrock and write to S3 with no human-in-the-loop control or change approval record.
4. Medium · CC6.7 · "Shadow AI workloads detected" — CloudTrail shows 3 untagged resources making Bedrock API calls outside your inventoried AI workload scope and current access control policy.

### Why Flintwood

**Section title:** "Built for mid-market AWS teams that move fast and audit annually."

| Label | Title | Body |
|---|---|---|
| Independent | No cloud vendor agenda | Flintwood is cloud-neutral and AWS-only by design — not a feature of a larger platform with a quarterly roadmap you can't influence. Your readiness report is ours to obsess over. |
| Narrow scope | SOC 2 + AWS + AI. That's it. | We don't sell you a 400-page CNAPP platform you'll use 10% of. We do one thing: monthly SOC 2 readiness for AWS, with an AI risk layer that no other tool has mapped yet. |
| Human reviewed | Not a raw scanner dump | Every report is reviewed before it reaches your inbox. Context, severity, and prioritized remediation — not a wall of alerts. Your engineering team will actually read it. |

### CTA section

**Eyebrow:** "Free pilot"

**Title:** "First report free.<br>No commitment."

**Body:** "We'll scan your AWS environment and deliver a full SOC 2 readiness report — including AI risk findings — within 5 business days. No strings."

**Single button:** "Request pilot report →" → `mailto:hello@flintwood.io?subject=Pilot report request`

**Note:** "AWS only · Read-only IAM role · SOC 2 TSC mapping included"

### Footer

Remove "Sample report" link. Footer links: How it works · AI Risk · Get free report

---

## Task 2 — PDF: regenerate `public/sample-report.pdf`

The new sample report is a 7-page document with a dark cover and light-themed interior pages. Regenerate using WeasyPrint from a self-contained HTML file at `/tmp/sample-report-v3.html`.

**Light theme (body pages):**
```css
--bg: #faf8f5; --bg2: #f3ede7; --surface: #ffffff;
--border: #e2dbd3; --border2: #cec6bc;
--ember: #c97030; --ember-lt: #e08c4a; --ember-bg: rgba(201,112,48,.07);
--crit-bg: rgba(192,80,64,.08); --crit: #c04838;
--high-bg: rgba(201,112,48,.1); --high: #b86020;
--med-bg: rgba(184,152,42,.1); --med: #906c10;
--low-bg: rgba(80,120,180,.08); --low: #3a6898;
--pass-bg: rgba(80,160,100,.08); --pass: #2e7a46;
--text: #1a1814; --muted: #6b6560; --faint: #9a9490;
```

**Cover page (dark, A4 794×1123px):**
- Background `#0d0c0a`
- Top bar: Flintwood logo + "Sample Report — Fictional Data" pill
- Company: "Acme Corp" in Instrument Serif 56px `#f7f4ef`
- Report type eyebrow: "SOC 2 Readiness Report"
- Meta grid (3 cells): Date "May 14, 2026" · Environment "AWS · [REDACTED]" · Regions "us-east-1 · us-west-2"
- 3 score cards: SOC 2 Readiness "72" (↓ −4 vs last month) · AI Risk "78" (4 AI/agentic findings) · Critical Findings "2" in red
- Disclaimer text bottom

**Page 2 — Executive Summary:**
- Score cards 2-up: SOC 2 72 (bar 72%, ↓−4 regression in CC7) · AI Risk 78 (bar 78%, 4 findings)
- Stat row 5-col: 2 Critical · 4 High · 4 Medium · 1 Critical AI · 10 Total Open
- Changes row 3-col: 12 new gaps · 5 resolved · 0 regressions
- Control Family table: AI_RISK (4 checks, 0 pass, 4 fail, 1 crit, 0%) · CC6 (5, 2, 3, 1 crit, 40%) · CC7 (5, 1, 4, —, 20%) · CC8 (2, 1, 1, —, 50%)

**Page 3 — AI & Agentic Risk** (badge: "Flintwood Exclusive"):
- Callout: "Immediate attention required. 1 critical and 2 high AI risk findings require remediation."
- 4 findings: AI_RISK.1 Critical (bedrock-agent-prod) · AI_RISK.2 High (logging) · AI_RISK.3 High (Lambda chain) · AI_RISK.4 Medium (shadow AI)
- Each finding: severity badge + AI Risk badge + title + ARN + Remediation label + remediation text

**Page 4 — Critical & High Findings:**
- Critical subsection (2 findings): AI_RISK.1 + CC6.3 (root access keys)
- High subsection (4 findings): AI_RISK.2 + AI_RISK.3 + CC6.2 (MFA) + CC7.1 (CloudTrail validation)

**Page 5 — Remediation Roadmap:**
- P1 (fix immediately): bedrock-agent-prod role + root access keys
- P2 (30 days): Bedrock logging + CloudTrail validation + IAM MFA + Lambda approval gate
- P3 (90 days): S3 access logging + Config recorder + shadow AI + GuardDuty TorIPCaller

**Page 6 — All Findings by Control Family:**
- CC6 (5 checks, 2 passing, 3 failing): CC6.3 Critical + CC6.2 High + CC6.5 Low
- CC7 (5 checks, 1 passing, 4 failing): CC7.1 High + CC7.4 Medium (S3) + CC7.3 Medium (GuardDuty) + CC7.2 Low
- CC8 (2 checks, 1 passing, 1 failing): CC8.2 Medium (Config recorder)

**WeasyPrint notes:**
- Remove `backdrop-filter`, `text-wrap: pretty`, `overflow-x: hidden` — not supported
- Replace Google Fonts `<link>` with local font stack or inline `@import` (WeasyPrint fetches remote fonts)
- Use `@page { size: A4; margin: 0; }` and `page-break-after: always` on `.cover` and each `.page`
- The ambient glow `radial-gradient` on the cover is fine — WeasyPrint supports gradients
- Output: `weasyprint /tmp/sample-report-v3.html /tmp/flintwood-init/website/public/sample-report.pdf`

---

## Deliverables

1. `src/pages/index.astro` — updated per Task 1
2. `public/sample-report.pdf` — regenerated per Task 2
3. Build passes: `npm run build` exits 0

Branch: `feat/handoff-3-redesign`
