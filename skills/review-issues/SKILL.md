---
name: review-issues
description: Review the development state of a Git project (GitLab or GitHub) and its open issues. Use when I ask to review, clean up or validate the issues of a repository, or to check which issues can be closed. Checks each issue against the current code, classifies it as done, partially done, still open or unclear, drafts closing comments and clarified descriptions, and never writes to the tracker without my explicit confirmation.
---

# Review the issues of a Git project

The goal is an honest picture of where the project stands, and a clean issue tracker:
closed issues explain how they were solved, open issues are clear enough for anyone to pick up.

## Non-negotiable rules

1. **Read-only until I confirm.** Never close, edit, label, assign or comment on an issue without my explicit
   confirmation for that specific issue. A general "ok" covers only the list of actions shown just before it.
2. **Evidence before closing.** Propose to close an issue only if the **current code** shows it is solved,
   backed by a merged MR/PR, a commit, a test, a changelog entry or a release. A merged MR alone is not enough:
   check that the fix is still there on the default branch. No evidence, no closing proposal: classify it as "to check".
3. **Never rewrite someone else's issue silently.** A clarified description is a proposal, shown in full
   before any edit. Keep the author's intent; add structure and missing facts, do not change the request.
4. **Never invent.** Do not make up reproduction steps, versions, error messages or causes.
   Mark missing information as a question to the issue author.
5. **Write like a human maintainer.** Everything posted (comments, descriptions) must read as if I wrote it myself:
   - short, factual, direct; cite the evidence (MR/PR number, commit hash, file);
   - plain sentences, the way the team already writes in this tracker; match its tone and length;
   - no mention of Claude or AI, no signature, no generated-by footer;
   - no AI tells: no "delve", "crucial", "comprehensive", "seamless", "it is worth noting", "I hope this helps",
     no em dashes as connectors, no bold everywhere, no emoji, no closing summary or pleasantries;
   - write in the language the issue is written in.

## Workflow

### 1. Understand the project (read-only)

- Detect the forge from `git remote -v`: GitLab → `glab`, GitHub → `gh`. Check the CLI is authenticated.
- Read the README, CHANGELOG, recent tags/releases, the default branch, and the last ~50 commits (`git log --oneline`).
- Summarise the state of the project in 5 lines max: what it does, current version, recent activity, open MRs/PRs.

### 2. Collect issues and evidence

| | GitLab (`glab`) | GitHub (`gh`) |
|---|---|---|
| Open issues | `glab issue list --per-page 100` (open by default) | `gh issue list --state open --limit 200` |
| One issue + comments | `glab issue view <n> --comments` | `gh issue view <n> --comments` |
| Merged MRs/PRs | `glab mr list --merged --per-page 100` | `gh pr list --state merged --limit 200` |
| Linked commits | `git log --all --oneline --grep "#<n>"` | same |

### 3. Check the code (for every issue)

Work on an up-to-date default branch (`git fetch` then read `origin/<default>`; never commit, push or switch the user's branch).
- Locate the code the issue is about: grep for function names, parameters, messages, file names cited in the issue.
- **Bug:** read the code path and decide whether the faulty behaviour can still happen. If a cheap and safe check exists
  (an existing unit test, `--help`, a dry run, a tiny input), run it and report the output.
  Never run long computations, pipelines on real data, cluster jobs or anything that writes outside a temporary folder without asking me.
- **Feature:** check whether the option, function, parameter or documentation exists and works as requested.
- Give the evidence as file and line (e.g. `modules/align.nf:42`), command output, or test name.
- If the code alone cannot tell (needs real data, external service), say so and classify as "to check".

### 4. Classify

| Status | Meaning |
|---|---|
| **Done** | Solved in the current code, with evidence. Candidate for closing. |
| **Partially done** | Some of it is solved in the code; list precisely what is left. |
| **Open** | Nothing implemented yet, still relevant. |
| **Unclear** | Cannot be acted on as written (vague, no context, no expected result). |
| **Obsolete / to check** | Possibly outdated, duplicate or no longer relevant: needs my decision. |

### 5. Report (in the conversation)

1. **Project state** (5 lines).
2. **Summary table:** issue number, title, status, evidence or reason, proposed action.
3. **Closing proposals:** for each "Done" issue, the comment to post, e.g.
   "Fixed in !42 (commit abc1234): `--min-reads` now defaults to 10 and is documented in docs/usage.md. Closing."
4. **Clarification proposals:** for each "Unclear" or "Partially done" issue whose description is not clear enough,
   a rewritten description. For a short issue, a few plain sentences are enough. For a longer one, use only the
   sections that help (and the repo's issue template if there is one):
   - **Context:** what the user was doing, version, environment.
   - **Problem:** what happens / what is missing.
   - **Expected:** what should happen.
   - **Steps to reproduce** (bugs) or **Scope** (features).
   - **Acceptance criteria:** how we will know it is solved.
   - **Open questions** for the author, when information is missing.
5. **Leads to solve it** (optional, for open issues): one to three concrete leads (where in the code, which approach),
   clearly labelled as suggestions.

Then stop and ask which actions to apply.

### 6. Apply, only what I confirm

| Action | GitLab | GitHub |
|---|---|---|
| Comment + close | `glab issue note <n> -m "<text>"` then `glab issue close <n>` | `gh issue close <n> --comment "<text>"` |
| Comment only | `glab issue note <n> -m "<text>"` | `gh issue comment <n> --body "<text>"` |
| Update description | `glab issue update <n> --description "<text>"` | `gh issue edit <n> --body-file <file>` |

Apply one issue at a time and report the result of each command. If a command fails or is blocked by a permission prompt,
stop and tell me; do not retry with another method.

## Confidentiality

Issue content may contain unpublished results or user data: do not save it to memory and do not search the web with it.
