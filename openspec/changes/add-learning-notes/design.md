## Context

This repository is a learning repository for OpenSpec. A beginner needs both conceptual explanation and real files to inspect.

## Goals / Non-Goals

**Goals:**

- Explain OpenSpec in Chinese for a beginner.
- Show the full spec-driven workflow from initialization to archive.
- Provide a concrete active change that can be validated by the OpenSpec CLI.

**Non-Goals:**

- Do not implement an application feature.
- Do not archive the example change yet, because keeping it active helps learners inspect the change structure.
- Do not introduce project-specific build dependencies.

## Decisions

- Use `README.md` as the primary learning entry because GitHub renders it by default.
- Use one capability named `learning-guide` so the example stays focused.
- Keep the OpenSpec change active so `openspec status --change add-learning-notes` remains useful for practice.

## Risks / Trade-offs

- Long README may feel large to beginners -> It is organized by numbered sections and includes a quick command cheat sheet.
- The example is documentation-focused rather than code-focused -> This avoids unrelated complexity while still showing the OpenSpec workflow.
