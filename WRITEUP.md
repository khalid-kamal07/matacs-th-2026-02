# AI Proposal Triage — System Design

## 1. AI vs. Rules

The system will use AI to extract information from uploaded proposal PDFs, including company experience, price, timeline, warranty, licence, insurance, and team details. AI will return structured JSON and identify missing or unclear information. It will not decide the final ranking on its own.

Normal code will validate the extracted data, check mandatory requirements, calculate weighted scores, and sort eligible proposals. Mandatory requirements include an Ontario electrical contractor licence held by the bidder, current WSIB clearance, and $5M liability insurance.

This approach combines AI's ability to understand documents with the consistency of rule-based processing.

## 2. Consistency and Explainability

The system will use admin-defined criteria and weights stored separately from application code. Weighted criteria must total 100%, and mandatory criteria will use pass/fail checks.

For this illustrative ranking, the scoring rules are: experience = qualifying installation count divided by the highest count among proposals being scored, multiplied by 30; price = lowest eligible price divided by the proposal price, multiplied by 25; timeline = fastest stated eligible completion time divided by the proposal's completion time, multiplied by 15; warranty = up to 7.5 points for a panel warranty capped at 25 years and up to 7.5 points for workmanship warranty capped at 12 years; team/approach = 4 points for a named lead, 3 for a stated crew size, 4 for a safety plan, and 4 for a monitoring plan. Scores are rounded to two decimal places after summing. These normalization formulas are design assumptions because the brief gives criterion weights but does not prescribe formulas; an administrator should approve them before production use.

Missing or unclear information will not be treated as a confirmed fact. A proposal with an unknown completion time is flagged instead of assigning an invented timeline score. The scoring code will use fixed rules, validated inputs, and consistent rounding. The same proposal data and criteria version should produce the same results. Each result will include criterion scores, total score, extracted evidence, and reasons for the ranking. Any change in inputs or criteria will trigger a new evaluation.

## 3. Human in the Loop

A reviewer will upload proposals, select the category, and review extracted information. Proposals with missing, conflicting, or uncertain information will be flagged for human verification.

The reviewer can correct extraction errors and request re-evaluation. Any override must include the person responsible, the reason, the previous result, and the updated result. The system will record the proposal identifier, criteria version, extracted data, scores, flags, timestamps, and reviewer actions for auditing.

The system will provide recommendations only. A human will make the final award decision.

## 4. Changing Criteria

Admin-defined criteria and weights will be stored separately from application code. The system will validate changes, including ensuring that weighted criteria total 100% and that mandatory rules are clearly defined.

Each criteria configuration will have a version number. When criteria change, previous evaluations will retain their original configuration for auditing. The reviewer can rerun evaluations using the new version, and the system will clearly identify results produced under different versions.

## 5. Risks and Safeguards

- **Incorrect extraction:** Validate data types and required fields. Flag uncertain information and allow human verification.
- **Hallucinated facts:** Use only information supported by the proposal. Store supporting text or page references and never treat missing information as confirmed.
- **Prompt injection:** Treat proposal contents as untrusted data. Ignore instructions inside documents that attempt to change the system's rules, scores, or ranking.
- **Inconsistent scoring:** Use deterministic code for mandatory checks, score calculation, and ranking rather than relying on AI-generated scores.
- **Bias or unfair ranking:** Apply the same approved criteria to all proposals and retain explanations for each score.
- **Incorrect or incomplete proposals:** Flag missing, conflicting, or ambiguous information for review before a final decision.
- **Privacy and security:** Restrict access to proposals, protect stored documents, and avoid logging unnecessary sensitive information.
- **Auditability:** Keep versioned criteria and evaluation records so results can be reviewed and reproduced.

The system should prioritize reliable evidence, transparent scoring, and human oversight over fully automated decisions.
