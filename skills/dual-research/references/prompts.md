# Codex Dispatch Templates

Read only the template needed for the current phase. Replace `{workspace}` with
the absolute research workspace path. Replace `{allowed-report-paths}` with an
explicit list of absolute output paths when using the repair template.

Every reporting template below includes the detail-report contract. Keep it in
the actual dispatch so Codex does not need to load the coordinator's skill.

## Preparation

```text
/codex:rescue --fresh --background "Read {workspace}/_agents/qa.md. Prepare for independent research on that question, but do not begin investigating yet. Return only clarification questions whose answers materially affect the scope or recommendation. If there are none, return NO_QUESTIONS. Do not read any Claude findings. Do not edit any files."
```

## Additional Clarification

```text
/codex:rescue --resume --background "Read the updated brief and answers in {workspace}/_agents/qa.md. Return any remaining blocking clarification questions, or NO_QUESTIONS. Do not read any Claude findings. Do not edit any files."
```

## Independent Research — Round 1

```text
/codex:rescue --resume --background "Read {workspace}/_agents/qa.md and independently research the question within its constraints. Do not read any Claude findings. Write a concise report, at most 100 lines, to {workspace}/round1-codex.md and a complete detail report to {workspace}/_agents/round1-codex-detail.md. The detail report must include: each material claim linked to evidence and sources, with source dates or applicable versions when relevant; confidence per claim (high, medium, or low) with a reason; considered but rejected options and rejection reasons; research gaps, unverified assumptions, and unresolved questions. The detail report has no line limit. Edit only these two specified files. Do not modify any other files."
```

## Independent Comparison — Round 2

```text
/codex:rescue --resume --background "Read {workspace}/_agents/qa.md, {workspace}/round1-claude.md, {workspace}/_agents/round1-claude-detail.md, {workspace}/round1-codex.md, and {workspace}/_agents/round1-codex-detail.md. Compare the findings and identify material differences in claims, sources, assumptions, and recommendations. For each difference, choose the better-supported position or propose an alternative, explaining the evidence. Do not assume agreement proves correctness. Record questions for the user or Claude. Do not read Claude's Round 2 evaluation yet. Write {workspace}/round2-codex.md (at most 100 lines) and {workspace}/_agents/round2-codex-detail.md (no line limit), even if no differences are found. The detail report must include: each material claim linked to evidence and sources, with source dates or applicable versions when relevant; confidence per claim (high, medium, or low) with a reason; considered but rejected options and rejection reasons; research gaps, unverified assumptions, and unresolved questions. Edit only these two specified files. Do not modify any other files."
```

## Reconciliation

```text
/codex:rescue --resume --background "Read {workspace}/_agents/qa.md, {workspace}/round2-diff.md, {workspace}/round2-claude.md, {workspace}/_agents/round2-claude-detail.md, and your own Round 2 reports. Reassess the remaining disagreements using evidence and any new answers. Update only {workspace}/round2-codex.md (at most 100 lines) and {workspace}/_agents/round2-codex-detail.md (no line limit). State what changed and why; preserve any disagreement that remains justified. The detail report must include: each material claim linked to evidence and sources, with source dates or applicable versions when relevant; confidence per claim (high, medium, or low) with a reason; considered but rejected options and rejection reasons; research gaps, unverified assumptions, and unresolved questions. Do not modify any other files."
```

## Repair Missing or Incomplete Reports

```text
/codex:rescue --resume --background "The previous task finished without complete required reports. Re-read {workspace}/_agents/qa.md and complete only these assigned outputs: {allowed-report-paths}. Human-facing round reports must be at most 100 lines. Detail reports have no line limit and must include: each material claim linked to evidence and sources, with source dates or applicable versions when relevant; confidence per claim (high, medium, or low) with a reason; considered but rejected options and rejection reasons; research gaps, unverified assumptions, and unresolved questions. Preserve the independence rules of the phase being repaired. Edit only the explicitly listed output files. Do not modify any other files."
```

## Completion and Continuation

Use the plugin's documented job completion mechanism. If background completion
needs inspection, the coordinator can use `/codex:status {job-id}` or
`/codex:result {job-id}` with the identifier returned at dispatch. Do not delegate
polling to the rescue forwarding subagent. Verify completion and the assigned
reports before dispatching the next phase. These are plugin commands; do not
replace them with direct Codex CLI calls.

`--resume` continues the latest rescue thread. If an unrelated task has replaced
that thread, stop rather than continuing in the wrong context.
