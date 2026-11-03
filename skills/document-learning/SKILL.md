---
name: document-learning
description: "Turn project work, study sessions, troubleshooting, and experiment results into durable, linked Markdown learning notes. Use when asked to document lessons, record an experiment, capture what was learned, or add findings to a knowledge library or digital garden, including Indonesian requests such as dokumentasikan pembelajaran or catat hasil eksperimen. Does not start experiments or publish notes unless requested."
---

# Document Learning

Create notes that preserve the reasoning, evidence, and limits of what the user learned. This skill works across subjects, repositories, and agents; it requires only Markdown and access to the relevant source material.

## Choose the source and destination

- Use the current request, conversation, and named artifacts to establish what to capture. Inspect the relevant code, report, notebook, logs, or references before making factual claims. Do not scan unrelated workspaces or chat archives by default, and do not claim access to unavailable conversations.
- Honor an explicit destination and its local instructions. Otherwise inspect workspace guidance for a knowledge-library destination. For Giovan's garden, read [the Knowledge Library profile](references/knowledge-library.md); it gives the default destination, language, topic maps, and publication rules.
- If the configured library is absent or inaccessible, create a local draft in an existing documentation location, or `docs/learning/` when none exists. Report the fallback and any transfer still needed. Never silently clone, push to, or substitute another repository.
- Search existing notes by topic, project, and result before writing. Update a concept note when understanding changes; keep distinct experimental runs traceable. Choose the smallest set of notes that captures the learning without duplicating project documentation.

## Extract the learning

Write a useful explanation rather than a chat transcript or a list of completed tasks. Capture the original question, relevant context, approach and decisions, observed outcome, what changed in understanding, and remaining questions. Preserve the user's actual contribution and avoid inventing first-person experiences.

Keep observed facts, interpretations, and proposed next steps distinguishable. Cite the artifact or source that supports a material claim. If evidence is missing, say what is unknown; do not reconstruct results from an implementation, a plan, or a remembered summary. Describe a passing build, a simulation, a local smoke check, and a deployed result at their actual scope.

For an experiment, retain the question or hypothesis, setup and relevant versions, input/data provenance, baseline or comparison, procedure, observations, failures, limitations, and next decision. Include seeds, sample counts, units, uncertainty, and run/commit identifiers when they affect interpretation and are available. Mark missing details explicitly. A failed, interrupted, or unrun experiment is a valid record; do not invent measurements, drop failed runs, or imply causation from association.

Use [the learning-note template](assets/learning-note.md) or [the experiment template](assets/experiment.md) as a starting point when helpful. Adapt or omit sections that do not serve the material. Set dates from the actual session or artifact, and distinguish an event date from the date the note was written when they differ. Follow the destination's language and style; the Knowledge Library profile uses English.

## Write and connect the note

- Choose one primary topic based on the question being explored. Link secondary topics and the relevant project rather than copying the same note into several categories. Preserve existing URLs when updating notes.
- Write meaningful titles and stable, descriptive filenames. Use dated filenames when separate runs need separate identities. Keep raw logs and bulky outputs in their source system; summarize the important evidence and record useful references.
- Link the note from an appropriate topic or project map when it can be usefully discovered there. Check that internal targets exist and that links from published pages do not lead to excluded drafts.
- Use sources actually inspected. For changing external claims, verify authoritative documentation when tools and access allow; otherwise identify the source version and verification limit. An inaccessible source must not become a fabricated citation.
- A public note needs public-safe evidence: public permalinks or sanitized excerpts, with provenance and limitations preserved. Remove credentials, personal records, private chat excerpts, internal endpoints, and identifying research data. Do not copy screenshots or attachments without reviewing their contents. Keep necessary private provenance in a confirmed ignored or untracked location, and explain any loss of reproducibility caused by redaction.

## Verify and hand off

Review the note against the source: numbers, units, dates, versions, attribution, failed runs, and the distinction between evidence and interpretation. Check frontmatter and links; use the destination's documented build when relevant and available. Do not rerun costly experiments or alter a live system merely to document it. Report unavailable checks honestly.

Default new notes to an unpublished draft unless publication was requested. In a public repository, a draft is still exposed if committed or pushed; publication flags only control site rendering. Inspect the exact diff and attachments before any authorized commit or push. Follow existing session authorization for Git and deployment actions; skill invocation itself grants no additional publication authority. If an external action fails, inspect the cause before retrying; stop on an authorization failure or a repeated unchanged error and report what remains.

Finish with the created or updated paths, a brief account of the learning captured, checks performed, and whether the result is a local draft, committed, pushed, or confirmed deployed. Do not report a note as live without checking the deployed page.
