# CCM-36674 — SP Layering & SP Options: Context File

**Jira:** [CCM-36674](https://harness.atlassian.net/browse/CCM-36674)  
**Parent epic:** [CCM-34580](https://harness.atlassian.net/browse/CCM-34580) — Improve Net New SP recommendations  
**Confluence brief (Faizan):** [Net-New SP features docs brief](https://harness.atlassian.net/wiki/spaces/LWG/pages/24325357732)  
**Assignee:** Arpit Agrawal | **Reporter:** Karan Shetty | **Priority:** P1  
**Sources:** Jira ticket + meeting transcript (Faizan walkthrough) + screen recording + Faizan's Confluence write-up

---

## Product area

**Cloud Cost Management → Cost Optimization → Commitment Orchestration (AutoCUD)**

The Commitment Orchestrator automatically recommends, purchases, and exchanges Reserved Instances (RIs) and Savings Plans (SPs) toward a shared coverage target per cloud account and service. This release adds new controls at two points in the product:

1. **Setup wizard → Step 3: Preferences** — two new purchase preference fields and one new commitment strategy toggle
2. **Actions tab → SP approval dialog** — new term and payment strategy overrides before confirming a purchase

---

## Background: how RIs and SPs relate

- Both RIs and SPs offer AWS discounts in exchange for committing to usage for 1 or 3 years.
- Both can cover the same compute usage (EC2). They share **one coverage budget** per orchestrator.
- Recommendations are sized using **historical usage over a full year**, not a short lookback window.
- Net-new Compute SP recommendations are sized from real SP-coverable on-demand demand, complementary to what RI already covers.
- Renewals follow their own path and are separate from net-new recommendations.

---

## Features to document (4 total)

### 1. Preferred commitment type (SP vs RI preference)

**UI label:** "Preferred commitment type"  
**Setup field:** `preferred_commitment_mode`  
**Location in UI:** Setup wizard → Step 3: Preferences → Purchase preferences section

| Value | UI card label | What it does |
|---|---|---|
| `ri_preferred` | Reserved Instances | RI first; SPs cover only what RI leaves uncovered (the residual). **Default.** |
| `sp_preferred` | Savings Plans | SP first; RIs cover the residual. |

**What changed:** Before this release, the orchestrator always used RI-preferred and this was not configurable.

**Key docs angle:** The preference controls *which commitment type spends the shared coverage budget first*, not whether the other type is turned off. Both may still be purchased — preference just sets the ordering.

**UI copy (verbatim from screen):**
- Reserved Instances card: "The orchestrator prioritizes reserved instances. Savings plans are used only when reserved instances aren't a good fit for the usage pattern."
- Savings Plans card: "The orchestrator prioritizes savings plans. Reserved instances are used only when savings plans aren't a good fit for the usage pattern."

**⚠ Open question before publishing:** The `sp_preferred` option is stored and validated in the backend but SP-first ordering *may not be fully live* in the current release. Confirm with Faizan whether to document it as generally available or as "available in an upcoming release."

---

### 2. Maximum commitment per Savings Plan

**UI label:** "Maximum commitment per savings plan ($/hr)"  
**Setup field:** `max_sp_hourly_commitment`  
**Location in UI:** Setup wizard → Step 3: Preferences → Purchase preferences section (below Preferred commitment type)

| Setting | Behavior |
|---|---|
| Unset or 0 | No customer ceiling; platform defaults may still apply |
| > 0 | Caps each net-new recommendation (and each staggered chunk) at this hourly commitment value |
| Below platform minimum | Rejected at setup validation |

**What changed:** This field did not exist before.

**Key docs angle:** This is a safety rail — it limits how large any single SP purchase can be. Useful for large accounts where the orchestrator might otherwise recommend a very large commitment in one go. Pairs naturally with SP Staggering (each staggered step is also capped by this value). Renewals of existing SPs are **not** governed by this ceiling the same way net-new recommendations are.

**Example (from transcript):** If eligible on-demand spend is $100/hr and you set the cap to $25/hr, the orchestrator recommends $25/hr in the first run. After that SP is approved and purchased, the next run recommends another $25/hr — and so on.

**Note:** Per-connector, per-recommendation — not per team or per account.

---

### 3. Harness SP Staggering

**Customer-facing name:** **Harness SP Staggering** — use this exact name in all docs. Do not use "layering" or "laddering."  
**UI label:** "Savings Plan staggering"  
**Setup field:** `savings_plan_layering.enabled` (do not surface the field name in customer docs)  
**Default:** Off  
**Location in UI:** Setup wizard → Step 3: Preferences → Commitment strategy section

**UI copy (verbatim):** "Build your Savings Plan coverage gradually through small purchases instead of one large commitment."

**What changed:** This toggle did not exist before. The UI is a simple on/off — no sub-options. The complexity is entirely in the backend.

#### Why it reduces risk (lead with this in docs)

A single large Savings Plan locks the customer into a large hourly commitment for 1 or 3 years. If usage drops after purchase, the unused commitment is wasted spend (overcommitment risk). Harness SP Staggering reduces this by:

- **Spreading purchases over time** instead of one monolithic commitment
- Buying only a **fraction of remaining uncovered demand** per step — early steps are modest, later steps shrink naturally as coverage grows
- Creating **decision points over time** — if usage falls, later steps size smaller or skip, so the portfolio adapts instead of overshooting upfront
- Working alongside **max hourly commitment** so no single purchase can be oversized

#### How it works

Each orchestrator run recomputes remaining uncovered SP-coverable demand (active and pending commitments already subtracted). It then purchases a fraction of that remaining demand rather than the full amount.

**Default pace (balanced):**
- ~8.3% of remaining coverable demand per step
- Several steps per month → ~50% of total coverable usage covered in about one month
- Chunks below ~$1/hr are skipped
- Recommendations expire after a few days; the next step is spaced so an in-flight approval or recent purchase does not stack another recommendation on top

**Why this pace:** It front-loads savings (50% coverage quickly) while keeping individual commitments small enough to adapt if usage changes. The fraction decays over time because the remaining uncovered pool shrinks with each step.

#### Future: stagger pace options (do not document as current)

A future release will let customers choose a pace:

| Pace | Intent | Effect |
|---|---|---|
| Aggressive | Faster coverage in the first month | Higher fraction → larger early chunks |
| Balanced | ~50% of coverable demand in ~1 month | Default today (~8.3% per step) |
| Slow | More conservative ramp | Lower fraction → smaller chunks, longer time to same coverage |

Document **Balanced** as the current behavior only. Do not imply Aggressive or Slow are available today.

#### Industry context (for writer background only — do not copy into docs)

ProsperOps describes the same FinOps concept as "Adaptive Laddering" (small commitments staggered over time to reduce lock-in risk). Use as conceptual inspiration only. In all Harness docs always use **Harness SP Staggering**.

---

### 4. Term and payment options — Compute Savings Plans only

**Location in UI:** Commitment orchestration → [Service] orchestrator → Actions tab → pending SP purchase → approve (checkmark icon)  
**Dialog title:** "Approve SP purchase?"

**Scope:** **Compute Savings Plans only** — covers EC2, Lambda, and Fargate. This does **not** apply to EC2 Instance SPs or Database SPs (not in this release).

**What changed:** Before this release, the orchestrator always purchased at 1-year, No Upfront. This was not overridable at approval time.

**New fields in the approval dialog:**

| Field | Options | Default |
|---|---|---|
| Purchase term | 1-year, 3-year | 1-year |
| Payment strategy | No Upfront, Partial Upfront, All Upfront | No Upfront |

Six combinations total (2 terms × 3 payment options).

**Read-only details shown in dialog:** Savings plan type, AWS service, Hourly spend, current Purchase term (updates as you select), Potential savings per month.

**Warning shown in dialog:** "This will execute the purchase on AWS immediately. The commitment is binding for the full term."

**Key docs angle:**
- Present this as a Compute SP approval-time choice, not a general setup override.
- A better discount (longer term or more upfront) usually means a **lower hourly commitment for the same coverage** — this is not "same $/hr, different discount."
- The selected term and payment are what actually gets purchased on AWS.

---

## What is out of scope for this docs pass

- RI payment-option or seed-term setup changes
- Term/payment matrix for EC2 Instance SP or Database SP
- Internal transaction/plumbing/observability-only fixes
- Platform-only knobs not visible in customer setup
- Aggressive/Slow stagger pace options (future release)

---

## Where docs live in the repo

Existing CO docs:
- `docs/operations/cloud-ai-cost-management/cost-optimization/commitment-orchestrator/`
  - `overview.md`
  - `README.md`
  - `ai-features.md`
  - `managing-commitments/ec2-managing-commitments.md`
  - `managing-commitments/rds-managing-commitments.md`
  - `managing-commitments/elasticache-managing-commitments.md`
  - `legacy/README.md`
- `3k-docs/operations/cloud-ai-cost-management/cost-optimization/commitment-orchestrator/` (3.0 parallel tree)

The new features most naturally slot into:
- Setup preferences content → wherever the setup wizard is documented (check `overview.md` or `README.md`)
- SP approval override → `ec2-managing-commitments.md` (since this is EC2/Compute SP only)

---

## Approval workflow

Per the transcript: raise a draft PR → Faizan reviews first → Karan Shetty gives final sign-off.

---

## Open questions before writing

1. Is `sp_preferred` fully live or should it be noted as "coming soon"?
2. Has Faizan sent the detailed technical write-up on the staggering algorithm? (He said he would during the call.) The Confluence brief covers it sufficiently for customer docs — confirm no additional detail is needed.
3. Does the max hourly commitment field need a note about what the platform minimum is (the brief says "below platform minimum → rejected" but doesn't give the value)?
