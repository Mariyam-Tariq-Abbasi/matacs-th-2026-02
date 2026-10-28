# Write-Up: AI-Assisted Proposal Triage

**Name:** Mariyam Tariq Abbasi
**Reference:** MATACS-TH-2026-02

**Assumption:** The budget ceiling (CAD 260k) and the 16-week limit are treated as eligibility checks, because the brief gives them as project limits.

## 1. AI vs. rules
- **AI is used only to read the proposals.** It pulls out facts (price, weeks, licence, warranty, experience, team details) and quotes the sentence each fact came from.
- **Code does everything else:** the pass/fail checks, the scoring and the ranking.
- **Why here:** AI is good at reading text written in many styles, but it can make small mistakes and can be fooled by instructions hidden in a document. Pass/fail checks and maths must give the same, provable answer every time, so they are done by code.

## 2. Consistency and explainability
- **Same result twice:** the AI reads each proposal once and the extracted facts are saved. Scoring always uses the saved facts, so ranking the same proposals twice gives the same result. If a proposal is re-read and a fact changes, the reviewer is shown the change.
- **Why a proposal ranked where it did:** the reviewer sees the pass/fail results, the points for each criterion, the exact formula and numbers used, and the quoted sentence behind every extracted fact.

## 3. Human in the loop and audit
- **Human review:** the tool only recommends. A person makes the final award decision. Proposals that are ineligible or flagged go to a review queue. A reviewer can override a result only by writing a reason.
- **Audit log:** every step is saved: upload, extracted facts and quotes, criteria version, scores, reviewer actions and overrides, and the final decision.

## 4. Changing criteria
- The admin's criteria and weights are saved with a version number.
- If the admin edits them after scoring, old results stay saved under the old version, and the reviewer is told the results are out of date.
- Proposals are then re-scored with the new version from the saved facts. The AI does not need to run again.
- Price and timeline scores compare proposals with each other, so adding or removing a proposal also needs a re-score.

## 5. Risks
- **Wrong extraction:** every fact comes with its quote, so the reviewer can check it quickly. Facts the AI is unsure about are marked for review.
- **Made-up facts:** the AI must use "not stated" (null) when information is missing. It is never allowed to guess.
- **Hidden instructions (manipulation):** proposal text is treated as untrusted data. Any instruction found in it is not followed. It is recorded and the proposal is flagged for a human.
- **Missing information:** a proposal with a missing key fact is flagged, not guessed.
- **Bias:** the AI never scores or ranks. Scores come from admin-set rules applied the same way to every proposal. Humans review the final list.
- **Cost:** the AI reads each proposal only once. Re-ranking and criteria changes use the saved facts and need no new AI calls.

## Scoring working
Formulas (the weights come from the admin's criteria, not fixed in code):
- Experience (30): 30 x min(projects, 10) / 10
- Price (25): 25 x (lowest price among scored proposals / this price)
- Timeline (15): 15 x (shortest weeks among scored proposals / this proposal's weeks)
- Warranty (15): 15 x (0.5 x panel years / 25 + 0.5 x min(workmanship years, 12) / 12)
- Team and approach (15): named lead 4, crew stated 3, safety plan 4, monitoring plan 4

Totals are added before rounding.

**A (rank 1, 98.8):** Experience 30.00 (14 installs, capped at 10), Price 25.00, Timeline 15.00 (12 weeks), Warranty 13.75, Team 15.00.

**E (rank 2, 92.4):** Experience 24.00 (8 installs), Price 24.54 (238,000 / 242,500), Timeline 13.85 (12 / 13 weeks), Warranty 15.00, Team 15.00.

**B (ineligible):** the bidder does not hold the electrical licence; the work is left to a subcontractor.

**C (flagged):** a hidden instruction to rank it first was ignored and reported for human review.

**D (flagged):** no committed schedule, so it cannot be scored until a person confirms it.
