# Seed prompt — bootstrap a personal Career Wiki (LLM-maintained career knowledge base)

> Paste everything below the line into Claude (Claude Code at the root of a private repo, or a
> Claude Project with access to your career material). It is person-independent: the first thing it
> does is discover *your* material and adapt to it. It pauses for confirmation before scaffolding,
> then builds a populated, provenance-backed wiki and keeps it current from then on.

---

You are going to create and bootstrap a **persistent, LLM-maintained career knowledge base** (a
"Career Wiki") for **the person you are working with**, following Andrej Karpathy's LLM Wiki pattern
(https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f).

The wiki captures everything a CV alone doesn't show: the context of each role, what the person
actually did and why it mattered, evidence for every claim, case studies, interview stories, career
decisions and their rationale, the history of applications and what was learned from them, and the
vocabulary of the domains they worked in. Every career output — CVs, cover letters, a personal
website, interview preparation — is **compiled from the wiki**, never written from scratch again.
The reasoning that today dies in old CV versions, e-mails and chat history gets written down once
and updated incrementally.

This prompt is **person-independent**. Do **not** assume a profession, seniority, country, language
or industry. Discover the person's material and adapt every choice below to what you actually find.

## The pattern — three strictly separated layers

1. **Sources (read-only ground truth).** You read from these, never write to them: CVs and their
   older versions, application archives (job descriptions, homework, case studies, cover letters),
   portfolios, certificates, performance reviews, talk slides, e-mails the person points you to, and
   an `assets/` folder inside the wiki for raw material the person drops in (notes, voice-memo
   transcripts, pasted job ads, feedback). **The person's own answers in a working session are a
   source too** — record them with the date they were given.
2. **The wiki (writable, compiled artifact).** A folder of cross-linked Markdown pages you create,
   edit and delete. Cross-page links use Obsidian-style `[[wiki-link]]` syntax so the graph is
   navigable (in Obsidian or any compatible viewer; it still works as plain Markdown).
3. **The schema.** A `CLAUDE.md` at the wiki root: the single source of truth for *when* to capture
   knowledge, *how* to write pages, which categories exist, the privacy rules and the format
   conventions. Every future session consults it.

## Privacy is load-bearing

A career wiki is personal data. Treat privacy as part of the schema, not an afterthought:

- Every page carries a `visibility:` field in its frontmatter:
  - `public` — may appear on a public website or a CV,
  - `shareable` — may go to a specific employer or recruiter on request, but not be published,
  - `private` — never leaves the wiki.
- **Public outputs are compiled only from `public` content.** A fact never moves to a more open
  visibility without the person's explicit approval.
- **Off-limits by default** (do not read or extract unless the person asks for that specific file):
  contracts, non-compete agreements, salary and bonus records, ID documents, HR admin, health or
  family matters. If such files are in the sources, list them in the schema as excluded.
- Third parties (colleagues, managers, referees) appear only by role ("the CTO"), unless the person
  explicitly provides a name *and* says it may be used.
- Employer confidentiality: no client names, internal numbers or unreleased products in `public`
  pages unless they are already public. When in doubt, generalise.
- If the wiki lives in a git repository, that repository is **private**. A public template (this
  seed) is fine to share; the wiki instance is not.

## Phase 0 — Discover the material, then confirm parameters with me

Before creating anything, investigate what you can reach and report back a short proposal:

- **Where the material lives.** Folders you can read, connected apps (cloud drive, e-mail), uploaded
  files. List what you found by type: CV versions (with dates), application folders, portfolios,
  certificates, other evidence. **Do not open the off-limits categories above.**
- **The shape of the career.** From file and folder names and the most recent CV only: roughly how
  many roles, which domains, which languages the material is in. Do not build the timeline yet.
- **Gaps visible already.** E.g. the most recent role missing from every CV, years without
  material, contradicting dates between CV versions.
- **Where the wiki should live.** A private git repo (best: versioned, diffable, links checkable),
  a cloud-drive folder, or the documents of a Claude Project. Recommend one based on what you see.
- **Output targets.** Which outputs the person wants compiled from the wiki (CV, website content,
  cover letters, interview prep), and in which languages.

Then propose, and **ask me to confirm before proceeding**:

- the **wiki root** location,
- the **initial category set** (start from the default below; add or drop based on this career —
  e.g. a researcher may want `publications/`, a freelancer `clients/`, a career changer
  `transitions/`),
- the **languages** (wiki language vs. output languages),
- the **instruction file** to wire in Phase 3 (root `CLAUDE.md` / `AGENTS.md`, or the Project's
  custom instructions),
- whether to set up the **optional periodic review** (Phase 4).

Ask only where the choice is genuinely mine; otherwise pick the sensible default and state it. Do
not start scaffolding until I confirm.

## Phase 1 — Scaffold the structure and write the schema

Create the wiki root with this **default starting taxonomy** (adapt it to what Phase 0 found):

```
<wiki-root>/
├── CLAUDE.md            # the schema — maintenance rules, privacy rules, page formats
├── index.md             # map of content — human-facing landing page
├── positioning.md       # who I am professionally: headline, target roles, core strengths
├── timeline.md          # the master chronology: every role, education, gap, with dates
├── glossary.md          # domain vocabulary (industries, products, methods) I can speak to
├── faq.md               # recurring questions (incl. hard interview questions) + agreed answers
├── open-questions.md    # contradictions and gaps waiting for my input
├── _log.md              # chronological changelog of wiki updates (append-only)
├── roles/               # one page per position: <yyyy-mm>-<org>-<title>.md
├── projects/            # one case study per notable piece of work
├── stories/             # interview stories in STAR form, tagged by competency
├── skills/              # one page per competency, linking to its evidence
├── credentials/         # education, certificates, talks, publications, awards
├── applications/        # one page per application: <yyyy-mm>-<org>-<role>.md
├── decisions/           # career decisions: <yyyy-mm-dd>-<slug>.md
├── outputs/             # registry of compiled outputs (CV variants, site copy, letters)
└── assets/              # READ-ONLY raw drops from me (sources, not wiki pages)
```

Give each folder an `_index.md` describing its purpose and page format. `applications/` and
`decisions/` use an **Active / Closed** split so items move rather than get deleted.

Write `<wiki-root>/CLAUDE.md` as the schema. It must contain, **adapted to this person's material**:

- **Layers.** The three layers, with the concrete list of *this* person's sources (which folders and
  apps are read-only ground truth, where `assets/` lives) and the explicit list of excluded files.
- **Privacy rules.** The visibility levels, the off-limits list, and the rule that outputs compile
  only from content at or below the output's visibility.
- **When to update the wiki** — the maintenance contract, treated as a checklist every session:
  - new role, promotion, title or scope change → `roles/` + `timeline.md`,
  - achievement, shipped project, measurable result → `projects/` and/or the role page,
  - a situation worth retelling (conflict, failure, leadership, hard decision) → `stories/`,
  - new competency evidenced → `skills/` (link the evidence; no unevidenced skills),
  - certificate, course, talk, publication, award → `credentials/`,
  - application sent, stage reached, homework done, offer or rejection → `applications/`,
  - interview held → the application page (questions asked, what landed, what didn't) and, if a
    question recurs, `faq.md`; a new good story → `stories/`,
  - career decision made (take/leave a job, sabbatical, specialisation) → `decisions/`,
  - feedback received (review, interviewer, mentor) → the relevant role/skill page, at `private`
    unless I say otherwise,
  - domain term used → `glossary.md`,
  - contradiction or gap found → `open-questions.md`,
  - files dropped in `assets/` → compile them into the right pages,
  - output generated → `outputs/` (what, when, for whom, which pages it was compiled from).
  - Always append a one-line entry to `_log.md`.
- **When NOT to update.** Wording-only changes; restating what a source already says verbatim (link
  it instead); speculation about the future (the wiki describes what *is* — plans live only in
  `positioning.md` target roles and in active `applications/`); anything in the off-limits list.
- **How to write pages.** One concept per page (split past ~200 lines); lead with the answer, then
  provenance; cross-reference liberally with `[[page-name]]`; absolute dates (`YYYY-MM`,
  `YYYY-MM-DD`), never "recently" or "currently".
- **Provenance and confidence.** Every non-obvious claim cites its source — a file path with its
  date or version, or `owner, YYYY-MM-DD` for something I stated in a session. Numbers carry a
  confidence marker: `verified` (document), `stated` (I said so), `estimate` (I estimated). **Never
  invent or round up a result.** A missing number is a question for `open-questions.md`, not a
  guess.
- **Contradictions.** Never silently overwrite. If CV versions or sources disagree (dates, titles,
  scope), record both with their sources, ask me, and log the resolution.
- **Page skeletons:**
  - `roles/`: Org (one-line description) / Title / Dates / Context (team, product, market) /
    Responsibilities / Results (with evidence and confidence) / Skills evidenced / Links /
    `visibility`.
  - `projects/`: Problem / My role / Approach / Result / Evidence / Confidentiality / Reusable as
    (CV bullet, case study, story).
  - `stories/`: Situation / Task / Action / Result / Lesson / Competency tags / Length (60s, 2min) /
    Last used.
  - `applications/`: Org and role / Source / Dates / Job description summary (link the original in
    `assets/`) / CV variant used / Stages / Homework / Outcome / Lessons.
  - `decisions/`: Context / Options considered / Decision / **Why** / Consequences — the Why is
    load-bearing.
  - `_log.md`: append-only, newest first, **one self-contained line per entry**.
- **Outputs are compiled, not authored.** A CV, website text or letter is generated from wiki pages.
  If I edit an output by hand, propagate the change back into the wiki (or ask whether it should
  be), so the wiki stays the single source of truth.
- **Evolving the schema.** The taxonomy is a starting point. When content fits no existing
  category, the schema may grow: create the folder and `_index.md`, add it to the tree here, add a
  "When to update" trigger, link it from `index.md`, log it. Default to *not* adding a category if
  an existing one absorbs the content. Evolution is explicit and rare.

Write `index.md` as the human-facing map of content and seed `_log.md` with the bootstrap entries.

## Phase 2 — Bootstrap real content

Don't leave empty stubs and don't write from imagination. Treat the bootstrap as **explicit research
tasks**, each worth doing on its own (run them in parallel if you can dispatch subagents). Each task
reads sources and writes real, provenance-backed pages:

1. **Timeline from all CV versions.** Read every CV version, oldest to newest. Build `timeline.md`
   and one `roles/` page per position. Where versions disagree, record both and add the conflict to
   `open-questions.md`. Note how the person's self-description evolved; it feeds `positioning.md`.
2. **Application archive.** Walk the application folders. One `applications/` page per application
   (closed ones in the Closed section). Extract patterns: which roles were targeted when, which
   homework and case studies exist — those are evidence and often become `projects/`.
3. **Portfolio and evidence.** Portfolios, talks, certificates and published work → `projects/` and
   `credentials/`, each with its source.
4. **Skills map.** From the pages above, derive `skills/` pages. Every skill links at least one piece
   of evidence; a skill without evidence goes to `open-questions.md`, not into `skills/`.
5. **Glossary.** Domain terms the career touches (industries, products, standards, methods), with a
   pointer to the role or project where each was used.
6. **The interview with the owner.** Last, and only for what the sources can't answer: walk
   `open-questions.md` with me **one question at a time** — missing roles, missing results,
   unresolved contradictions, the context behind decisions. Write my answers into the pages as
   `stated` with today's date.

Keep a **conservative bias**: a smaller true wiki beats a padded one. Every non-obvious claim cites
its source. No result without evidence or my explicit statement.

## Phase 3 — Wire the instruction file so sessions self-maintain the wiki

Add a section to the **top** of the confirmed instruction file (root `CLAUDE.md` / `AGENTS.md`, or the
Project's custom instructions). Adapt the paths:

```markdown
## Career knowledge base — `<wiki-root>/`

`<wiki-root>/` is an LLM-maintained knowledge base of my career: roles, projects, evidence,
interview stories, applications, decisions and positioning. Every career output (CV, cover letter,
website, interview prep) is compiled from it.

**Before any non-trivial career task:**

- Read `<wiki-root>/CLAUDE.md` — the schema, including the privacy rules. Treat its "When to
  update" list as a checklist for the current task.
- Skim `<wiki-root>/index.md` and read the pages relevant to the task.

**While working:**

- If something changes or is learned — a new role or result, an application step, an interview, a
  decision, feedback, a new story — update the relevant page per the schema and append a one-line
  entry to `<wiki-root>/_log.md`, in the same session (and the same commit, if the wiki is in git).
- Compile outputs only from pages whose `visibility` allows it. Never invent a result.
```

The load-bearing instruction is *"update the wiki in the same session as the change that triggered
it."* The discipline lives in the instructions, not in anyone's memory.

## Phase 4 — (Optional, if I confirmed it) the periodic review

Careers change slowly and then suddenly; small wins are forgotten within months. If I opted in, set
up a recurring review (a scheduled task, a calendar reminder, or a documented ritual — whatever this
environment supports), monthly or quarterly, that:

1. **Asks me** a short, fixed set of questions: what changed in my role, what shipped, what I'm
   proud of, what went wrong, any feedback, any applications or recruiter contact.
2. **Updates the wiki** from my answers per the schema.
3. **Runs a lint pass:** dangling `[[links]]`, stale `timeline.md`, results without evidence,
   pages with missing or inconsistent `visibility`, stories not used in a long time, open questions
   that were answered elsewhere.

Bake in: **"when in doubt, don't write — a noisy review is worse than a quiet one."** The review only
writes inside the wiki root.

## Phase 5 — Verification

- Every `[[link]]` resolves; every `index.md` link is live.
- Every page has a `visibility` field; nothing from the off-limits list was extracted.
- Every result on a `roles/` or `projects/` page has a source and a confidence marker.
- `open-questions.md` lists every unresolved contradiction and gap.
- If the wiki is in git: the repository is private, and the wiki and the instruction-file change are
  committed together.
- Report a summary: wiki root, categories created, page counts per category, what each bootstrap
  task produced, the open questions left, and the instruction-file changes made.

Proceed through the phases in order. Pause for my confirmation at the end of Phase 0. Keep a
conservative bias throughout: a smaller true wiki beats a padded one.
