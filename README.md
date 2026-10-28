# MATACS-TH-2026-02: AI Proposal Triage Design

**Name:** Mariyam Tariq Abbasi
**Reference:** MATACS-TH-2026-02
**Role:** Associate Software Developer
**Time actually spent:** About 1.5 hours in total, including setting up GitHub for the first time.

## How I used AI tools
I used Claude to understand the brief and check my work, and NotebookLM to draft the write-up, prompt, ranking file and diagram. I wrote my own answer to Question 1 first (AI extracts facts, code does the checks and scoring) and used it as the base for the design. I disagreed with NotebookLM's first draft, which had a wrong price-score calculation (18.2 instead of 7.8), claimed I spent 60 minutes and said I had built a security layer, so I rejected it; in the redo I also corrected its wrong sub-scores for Proposal E (Experience 30 instead of 24, Team 9.1 instead of 15).

## Files
- [WRITEUP.md](./WRITEUP.md): answers to the five questions and the scoring working
- [PROMPTS.md](./PROMPTS.md): the fact-extraction prompt
- [ranking.json](./ranking.json): expected ranking of proposals A to E
- [diagram/architecture-diagram.png](./diagram/architecture-diagram.png): process flow from upload to ranked output
