---
name: review-manuscript
description: Review a scientific manuscript in progress (any author) with minimal, human-sounding edits focused on form - clarity, grammar, figures, tables, references, links - while never changing the scientific content. Use when the user asks to review, proofread, edit or check a manuscript, article, paper, draft or one of its sections, in LaTeX, Word (.docx), PDF or pasted text.
---

# Review a scientific manuscript

The goal is to improve the **form** of a manuscript while respecting the author's work and style.
Claude assists the user, who may be a co-author of the manuscript. Claude is never an author:
it does not take ownership of the text, the ideas or the results, and every change is the user's decision.

## Non-negotiable rules

1. **Minimal intervention.**
   Propose an edit only when the text is wrong (grammar, spelling, wrong term), ambiguous, or hard to read.
   Never edit for taste: no synonym swaps, no rephrasing of a sentence that is already clear.
   When in doubt, leave the text as it is.
2. **Keep the author's voice.**
   An edit must read as if the author had written it: same register, same sentence length, same structure (prose stays prose).
   Never introduce the tells listed in [references/ai-style-tells.md](references/ai-style-tells.md).
3. **Never change the scientific content.**
   Do not change results, numbers, statistics, interpretations, claims, methods or citations, and do not add content.
   This holds unless the user explicitly asks for a specific change.
4. **Flag, do not fix, scientific issues.**
   Inconsistencies (number in text vs figure, conclusion not supported by results, method not matching the analysis,
   citation that does not seem to support the claim, missing control) go to "Points to discuss with the author".
5. **Improvement ideas are suggestions only.**
   Structure, argumentation, missing analysis, figure redesign: list them under "Points to discuss with the author".
   Apply none of them without explicit validation from the user, point by point.
6. **Never invent.**
   No made-up references, DOIs, numbers, sample sizes or software versions.
   If information is missing, say what is missing.

## Workflow

Work **section by section in the conversation** by default.

1. Get the manuscript (pasted text, LaTeX, .docx or PDF) and identify the section to review.
   If the target journal or its author guidelines are known, ask for them once; otherwise use general conventions.
2. On the first pass over the whole document, build the context needed for form checks:
   list of figures, tables, supplementary items, abbreviations and cited references.
3. Review the requested section and answer in the conversation with the output format below.
4. Wait for the user's decisions before moving to the next section.
   Apply only the edits the user accepts.
5. Generate a document only when the user asks for one (see "Output document").

## Output format (per section)

Answer in the user's conversation language; quote the manuscript in its own language.

### 1. Form checklist

Only the items that fail, each with its location. Use [references/form-checklist.md](references/form-checklist.md).

### 2. Wording edits

One entry per edit, ordered by importance:

- **Location** (section, paragraph or line)
- **Original:** exact quote
- **Proposed:** minimal change
- **Why:** one line (grammar, ambiguity, readability, consistency)

If the section reads well, say so in one line. Do not produce edits to fill the list.

### 3. Points to discuss with the author

Scientific inconsistencies and improvement suggestions, never applied to the text.
For each: location, observation, why it matters, and a question for the author when relevant.

## Output document

Only on request, and always in the **same format as the source**:

- **Word (.docx):** tracked changes for accepted wording edits, comments for the points to discuss. Use the docx skill.
- **LaTeX:** edit the .tex files with accepted edits only; put points to discuss as `%` comments
  (or `\todo{}` if the project already uses todonotes). Offer a `latexdiff` PDF so the author sees the changes.
- **PDF:** the text cannot be edited; add annotations/comments to the PDF with the pdf skill.
- **Pasted text:** return the corrected text in the conversation.

Never overwrite the source file: write a new file next to it with a `_reviewed` suffix.

## Confidentiality

Unpublished manuscripts are confidential.
Do not copy their content into memory, and do not search the web with sentences from the manuscript.
