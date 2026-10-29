# MATACS — Associate Software Developer Assessment

**Candidate:** Khalid
**Reference:** MATACS-TH-2026-02  
**Time spent:** 25-30 min

## Repository contents

- [System flow diagram](diagram/flowchart.png)
- [System design write-up](WRITEUP.md)
- [AI prompts](PROMPTS.md)
- [Expected proposal ranking](ranking.json)

## Overview

This submission proposes an AI-assisted workflow for extracting facts from contractor proposals and evaluating them against administrator-defined criteria. AI extracts information and identifies uncertainty; deterministic application code validates inputs, checks mandatory requirements, and calculates scores. A human reviewer verifies flagged information and makes the final award decision.

## How AI tools were used

I used ChatGPT to help structure the design, draft prompts, and review edge cases. I checked the proposed approach against the assessment requirements and kept scoring, mandatory checks, and ranking in deterministic application logic rather than delegating final decisions to AI. One important correction was to avoid treating a licensed subcontractor as satisfying a requirement that the bidder itself hold the licence; proposal B is therefore marked ineligible under the stated rule.


