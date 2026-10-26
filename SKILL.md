---
name: paep-research-agent
description: Review supplied research papers, code and experiment evidence for reproducible PAEP research decisions.
metadata:
  version: 1.1.0
  owner: paep-user
  category: research
---

# paep-research-agent

Use only supplied local sources and task context. Separate hypotheses, reported findings and independently reproduced evidence. Check dataset availability, leakage, baselines, ablations, metrics and compute constraints. Cite local source locations and list missing evidence. Recommend a bounded validation experiment without launching training or making network/model calls. Never fabricate citations, measurements or statistical significance.

## Inputs

- `repository_path` (required, path): absolute directory containing the supplied papers, code and experiment records. Read-only.
- `brief` (optional, string): the research question and which decision the review supports.

## Output

`artifacts/research-report.md` — the review in the structure below.

## Preparation checklists

Run all three checklists before writing conclusions; report each item as `present (path)` / `absent` / `partial`:

Papers checklist:

- [ ] Claimed contributions identified with the exact quoted sentences that state them.
- [ ] Method reproducibility: hyperparameters, data splits, seeds, compute stated or missing.
- [ ] Evaluation: baselines listed, metrics defined, statistical treatment stated or absent.
- [ ] Citation graph: which local files cite/support each claim.

Code checklist:

- [ ] Entry points and how results are produced (exact commands or their absence).
- [ ] Data loading: dataset paths, preprocessing, and whether leakage checks exist.
- [ ] Result artifacts: which numbers come from which script/output, and whether reruns are deterministic.
- [ ] Dependency/runtime pins recorded or missing.

Experiment checklist:

- [ ] What was actually run vs what was only planned (records, logs, result files).
- [ ] Controls/ablations present vs claimed.
- [ ] Environment (hardware, versions) recorded or missing.
- [ ] Any experiment whose numbers exist only in prose (no artifact backing).

## Citation and uncertainty format

- Cite as `path:line` or `path#section` for every factual claim about a source; a claim without a local citation is marked `UNCITED` and excluded from conclusions.
- Uncertainty is written inline as one of:
  - `(verbatim, p. N)` — directly quoted or transcribed from the source.
  - `(inferred)` — your derivation from local sources; show the derivation.
  - `(unverified)` — reported by the source, not independently checked locally.
  - `(not found locally)` — the source asserts it; nothing in the supplied material confirms it.
- Never merge `(verbatim)` and `(inferred)` in one sentence without labeling each part.

## Report structure

```text
# Research review — <question> — <UTC date>
## Question and decision context   — from the brief, [recorded]
## Checklist results               — three checklists with present/absent/partial + paths
## Findings                        — each: claim + citation + uncertainty label
## Reproduction status             — original / inferred / reproduced, per key claim
## Missing evidence                — what is absent locally and what would fill it
## Bounded validation experiment   — one proposal: inputs, expected evidence, cost bound, no execution
## Sources                         — every local file used, with UTC read time
```

## Example invocation

```markdown
# Goal
Assess whether the supplied method's claims are supported by the local
paper, code and experiment records before committing to a replication.

# Context
All supplied material is under the repository; nothing is fetched from the network.

# Constraints
Read-only; no training, no model calls; citations must resolve to local paths.

# Acceptance criteria
- All three checklists completed with paths.
- Every finding carries citation and uncertainty labels.
- Validation experiment is bounded and not executed.
```

## Combination with professional skills

Use `literature-review` for broad source discovery before this focused review, `scientific-critical-thinking` for argument-strength assessment, and `experimental-design` when the bounded validation experiment is later planned for real. This skill supplies the evidence-discipline review; it does not replace survey or design work.

## Handoff convention

The report plus the cited local paths are the complete handoff. A follow-up replication task starts from the `Bounded validation experiment` section; anything not locally reproducible stays listed under Missing evidence, not silently dropped.

## Execution boundary

Use the PAEP task/result protocol and the declared Markdown output in skill.yaml.
The runtime uses a reviewed external codex handoff. Writes, network,
shell commands, credentials and production mutations are outside this skill's
permissions. If evidence is missing, report it and stop; do not broaden access.
