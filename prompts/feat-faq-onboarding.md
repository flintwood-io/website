# Prompt: FAQ section + onboarding copy improvements

## Context

Flintwood.io homepage (Astro, `src/pages/index.astro`). Ember/dark design system.
Two changes: (1) update Step 01 marketing copy to reflect one-click CloudFormation
onboarding, (2) add an FAQ section addressing the three questions prospects ask before
connecting their AWS account.

---

## Task 1 — Update Step 01 copy

In `#how-it-works`, Step 01:

**Body:** "Deploy a single read-only IAM role via one-click CloudFormation — no CLI, no
local tooling. It opens in your AWS Console, you review the permissions, click Create
Stack. We assume the role once a month, then disconnect."

**Pill:** "One-click CloudFormation · 2 min · No CLI"

---

## Task 2 — Add `#faq` section

Place between `#diff` (Why Flintwood) and `#sample-report`.

**Section eyebrow:** "Common questions"
**Section title:** "Everything you need to know before connecting your AWS account."

5 Q&A items. Use an accordion or flat Q&A layout — flat (always-visible) preferred for
SEO and simplicity. Each item: bold question, muted body text.

### Questions & answers

**1. How does AWS access work? Do I need to install anything?**
No. You deploy a read-only IAM role using a one-click CloudFormation button — it opens
directly in your AWS Console, pre-filled. You review the exact permissions, click
"Create Stack," and it's done in under 2 minutes. No CLI, no credentials to configure,
nothing to install.

**2. What can Flintwood see in my AWS account?**
Configuration metadata only — IAM policies, CloudTrail trails, GuardDuty findings,
Config rules, S3 bucket policies, Lambda function configurations, and Bedrock role
permissions. We do not access the contents of S3 objects, databases, application
secrets, or any customer data stored in your account.

**3. Is it safe to give a third party read-only access?**
You deploy the role — you control it. Every access we make appears in your CloudTrail
logs. You can revoke access instantly by deleting the IAM role or CloudFormation stack —
no support ticket, no offboarding process. This is the same pattern used by Wiz,
Datadog, Lacework, and every major AWS security vendor.

**4. Does this affect my SOC 2 compliance?**
It helps it. Our access is read-only, scoped to specific services, and logged in
CloudTrail — that's CC7 evidence you're generating for free. Auditors treat documented
third-party security assessment access as a positive control signal. We also provide a
Data Processing Agreement on request that describes exactly what we access and how we
handle it.

**5. How do I revoke access?**
Delete the CloudFormation stack (or the IAM role directly) from your AWS Console.
Access is revoked instantly — we can no longer assume the role. Nothing else required.

---

## Design spec

```
#faq {
  padding: 80px 48px;
  background: var(--bg);         /* same as hero bg, alternates with #diff surface */
  border-top: 1px solid var(--border);
}

.faq-grid {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 0;
  margin-top: 48px;
  border: 1px solid var(--border);
  border-radius: 8px;
  overflow: hidden;
}

.faq-item {
  padding: 28px 32px;
  border-right: 1px solid var(--border);
  border-bottom: 1px solid var(--border);
}
/* Remove right border on 2nd column, bottom border on last row */
.faq-item:nth-child(2n) { border-right: none; }
.faq-item:nth-last-child(-n+2) { border-bottom: none; }

.faq-q {
  font-size: 14px;
  font-weight: 600;
  color: var(--white);
  margin-bottom: 10px;
  line-height: 1.4;
  letter-spacing: -0.01em;
}

.faq-a {
  font-size: 13.5px;
  color: var(--muted);
  line-height: 1.68;
}
```

Mobile (<960px): single column, remove grid borders, each item gets a bottom border only.

---

## Deliverables

- `src/pages/index.astro` updated (Step 01 copy + new #faq section)
- Build passes: `npm run build` exits 0
