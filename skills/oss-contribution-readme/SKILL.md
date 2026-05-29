---
name: oss-contribution-readme
description: Create or update a polished README that showcases a contributor's open-source work with an OSS contribution matrix, linked project names, official logos, verified PR or patch counts, open and merged status, optional featured-project details, and LinkedIn-ready intro copy. Use when the user wants a GitHub profile README, standalone OSS portfolio repo, contribution table with logos, public PR counts, tracker patch submissions, featured project table, or reusable open-source contribution showcase.
---

# OSS Contribution README Skill

## Purpose

Create a public-facing README that accurately presents a contributor's
open-source history. Optimize for correctness, clean rendering on GitHub,
durable links, visible logos, and text that can be reused in LinkedIn or a
personal portfolio.

## Execution Guardrails

- **Verify before claiming.** Confirm PR, patch, star, fork, and tenure numbers
  from public sources or the user's supplied tracker output before writing them.
- **Keep public artifacts clean.** Do not include private tool names,
  generated-by notes, hidden workflow details, or agent provenance in the
  README, commit messages, branch names, or public prose.
- **Use durable links.** Prefer official project URLs, GitHub repos, Apache logo
  service assets, project docs, or official organization avatars. Avoid fragile
  image search URLs and expiring assets.
- **Design for screenshots.** Keep table labels short, use compact logos,
  avoid crowded prose inside a single cell, and switch to HTML tables when
  Markdown wrapping makes logo/name pairs look bad.
- **Do not inflate status.** Count merged PRs only when GitHub or the upstream
  tracker shows acceptance. Closed is not merged.
- **Respect mixed contribution types.** If GitHub PRs and tracker patches are
  combined, use neutral wording such as "Contributions" in prose and explain
  the mix only when needed.

## Inputs, Outputs, and Preconditions

| Type | Content |
| --- | --- |
| Inputs | Contributor name, GitHub username, target README path, project list, optional tracker URLs, preferred intro wording, optional featured projects |
| Outputs | `README.md` with intro, contribution matrix, linked logos, verified counts, totals, and optional featured contribution table |
| Evidence | Count commands or source URLs, earliest contribution date for tenure claims, logo source links, `git diff --check` result |

Preconditions:

- `gh` is authenticated when GitHub counts need live verification.
- The user supplies non-GitHub tracker URLs or enough detail to find them.
- Network access is available for current counts, logos, stars, forks, or docs.

## Workflow Overview

Use this ordered workflow unless the user asks for a narrow edit:

1. Gather identity and scope.
2. Verify GitHub and tracker contribution counts.
3. Verify tenure or impact claims.
4. Select project links and logos.
5. Build or update the README tables.
6. Check rendering risks, totals, and link targets.
7. Validate, commit, and push when requested.

## Step 1: Gather Scope

Capture or infer:

- Display name and GitHub username.
- README target, usually a profile repo or standalone showcase repo.
- Projects to include in the main matrix.
- Non-GitHub contribution systems such as JIRA, Bugzilla, mailing-list patches,
  or project-specific trackers.
- Featured projects that need more context than counts, such as creator role,
  ecosystem listing, tech stack, stars/forks, or problem solved.
- Sorting preference: project name by default, or count descending when the user
  wants an impact-ranked table.

If the target repo should contain only a README, do not add generated data files,
screenshots, caches, or scripts unless the user explicitly asks.

## Step 2: Verify Counts

Do not rely on visual GitHub page counts alone. Use `gh` or the GitHub app.

For a GitHub repo:

```bash
gh pr list --repo OWNER/REPO --author USERNAME --state all --limit 1000 \
  --json number,state,mergedAt,url,title,createdAt
```

Count:

- `PRs Created`: all returned PRs for that author in that repo.
- `Open PRs`: PRs with `state == "OPEN"`.
- `Merged PRs`: PRs with non-null `mergedAt`.

For a contributor-wide earliest public PR:

```bash
gh search prs --author USERNAME --sort created --order asc --limit 20 \
  --json repository,number,title,createdAt,state,url
```

Use this to verify claims such as "more than a decade." As of the current date,
the earliest verified public contribution must be at least 10 years old. If the
evidence is exactly near the boundary, state the exact date instead of rounding.

For non-GitHub trackers:

- Use the tracker query URL supplied by the user when possible.
- Count visible submitted tickets or patches from that query.
- Link the count to the query or issue list.
- Track open and merged/accepted status only when the tracker exposes it.
- If a tracker uses terms like `OPEN`, `RESOLVED`, `FIXED`, or `CLOSED`, map
  them carefully and say when a status is not equivalent to GitHub merged.

## Step 3: Choose Logos and Links

Use this priority order:

1. Official project site or docs logo.
2. Official repo asset on the default branch.
3. Foundation logo service, especially Apache `https://apache.org/logos/res/...`.
4. Official GitHub organization avatar.
5. Text-only project link when no clear logo is available.

Rules:

- Link the project name to the official project site or repository.
- Link the count cell to the contributor PR list or tracker query.
- Use `height="18"` for logos in tables unless the user asks for larger visuals.
- Add descriptive `alt` text, for example `Apache Hadoop logo`.
- If a white logo disappears on GitHub's white background, use a dark variant,
  icon-only variant, official avatar, or text-only fallback.
- Avoid underlined-looking custom CSS; GitHub strips most CSS anyway. Use normal
  links and let the platform render them.

Compact Markdown table cell:

```html
<a href="PROJECT_URL"><img src="LOGO_URL" alt="Project logo" height="18"></a> <a href="PROJECT_URL">Project</a>
```

No-wrap HTML table cell when logo and text split across lines:

```html
<td nowrap="nowrap" width="180"><a href="PROJECT_URL"><img src="LOGO_URL" alt="Project logo" height="18"></a>&nbsp;<a href="PROJECT_URL">project-name</a></td>
```

## Step 4: Write the README

Recommended intro:

```markdown
# Open Source Contributions

I have been contributing to open source for more than a decade. Every
contribution is a chance to learn something new and collaborate with amazing
communities.

Name's GitHub handle: [username](https://github.com/username)
```

Main matrix:

```markdown
## Contribution Matrix

| Project | PRs Created | Open PRs | Merged PRs |
|---------|-------------|----------|------------|
| <a href="https://github.com/apache/hadoop"><img src="LOGO" alt="Apache Hadoop logo" height="18"></a> <a href="https://github.com/apache/hadoop">Apache Hadoop</a> | [13](https://github.com/apache/hadoop/pulls/username) | 10 | 3 |
| **Total PRs** | **13** | **10** | **3** |
```

When the matrix includes tracker patch submissions, either:

- Rename the section to `Open Source Contributions`, or
- Add one short sentence before the table that says the matrix includes GitHub
  PRs and tracker-based patch submissions.

Do not over-explain obvious columns. Avoid long definitions such as "PRs Created
means PRs created" in the README.

## Step 5: Add Featured Contributions

Use a separate featured/additional table for projects where the story matters
more than PR counts. This avoids crowding the main matrix.

Preferred columns:

| Column | Use |
| --- | --- |
| Project | Logo plus linked project name |
| Ecosystem | Parent ecosystem, community listing, marketplace, docs, or announcement |
| Role | Creator, maintainer, contributor, architect, reviewer |
| Problem Solved | One concrete problem the project or contribution addressed |
| Tech Stack | Short comma-separated list |
| Metrics | Stars, forks, downloads, adoption, merged status, or other public metrics |

Use an HTML table for this section when it has many columns:

```html
<table>
  <thead>
    <tr>
      <th>Project</th>
      <th>Ecosystem</th>
      <th>Role</th>
      <th>Problem Solved</th>
      <th>Tech Stack</th>
      <th>Metrics</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td nowrap="nowrap" width="180"><a href="PROJECT_URL"><img src="LOGO_URL" alt="Project logo" height="18"></a>&nbsp;<a href="PROJECT_URL">project-name</a></td>
      <td><a href="ECOSYSTEM_URL"><img src="ECOSYSTEM_LOGO" alt="Ecosystem logo" height="18"></a>&nbsp;<a href="ECOSYSTEM_URL">Ecosystem</a></td>
      <td>Creator, architect, maintainer</td>
      <td>NVIDIA GPU observability through <code>nvidia-smi</code> / NVML metrics shipped into Elasticsearch.</td>
      <td>Go, Elastic Beats, Elasticsearch, NVIDIA SMI/NVML, Python</td>
      <td>56 stars, 18 forks</td>
    </tr>
  </tbody>
</table>
```

Keep featured rows factual. If a role ended at a previous employer, use wording
like "creator, architect, and maintainer while at Company" only when the user
wants the employment context included.

## Step 6: LinkedIn Copy

When the user asks for a LinkedIn caption, write short, warm prose. Mention
current focus areas only when relevant:

```text
I have been contributing to open source for more than a decade. Every
contribution is a chance to learn something new and collaborate with amazing
communities.

Lately, my focus has been around the AI ecosystem, including OpenClaw, NemoClaw,
Inspect AI, Inspect Petri, Hermes Agent, and PyTorch.

#OpenSource #AI #MachineLearning #PyTorch #InspectAI #OpenClaw
```

Do not add inflated claims, unverifiable impact, or a long project dump unless
the user asks.

## Step 7: Validate and Finish

Before finalizing:

```bash
git diff --check -- README.md
```

Also check:

- Counts match the latest verified data.
- Totals equal the visible rows.
- Project names link to official sites or repos.
- Count cells link to contributor PR lists or tracker queries.
- Logos render on a white background.
- Logo/name pairs stay readable and do not wrap awkwardly.
- Featured project cells are split into columns instead of one crowded sentence.
- Public text contains no private tool attribution or generated-by language.

If working in a git repo and the user wants the update published, commit only
the README and directly related skill/template files. Leave unrelated dirty
files untouched.

## Quick Reference

| Task | Command or Rule |
| --- | --- |
| List author PRs in one repo | `gh pr list --repo OWNER/REPO --author USER --state all --limit 1000 --json number,state,mergedAt,url` |
| Find earliest public PR | `gh search prs --author USER --sort created --order asc --limit 20 --json repository,number,title,createdAt,state,url` |
| GitHub contributor link | `https://github.com/OWNER/REPO/pulls/USER` |
| Logo size | `height="18"` |
| Default sort | Project name ascending |
| Validation | `git diff --check -- README.md` plus visual/render inspection |
