## Context

The repository now has a beginner README, but a learner still benefits from a concrete exercise that can be repeated independently.

## Goals / Non-Goals

**Goals:**

- Provide a hands-on practice lab for creating and validating an OpenSpec change.
- Use a low-risk documentation scenario so beginners can focus on the workflow.
- Link the lab from README for easy discovery.

**Non-Goals:**

- Do not implement application source code.
- Do not archive the practice-lab change immediately, because keeping it active helps learners inspect the artifact structure.

## Decisions

- Put the lab in `docs/openspec-practice-lab.md` because it is supporting learning material.
- Use a glossary-document exercise because it is small, safe, and demonstrates the whole workflow.
- Keep commands copyable and explain the expected result after each step.

## Risks / Trade-offs

- The lab adds more reading material -> It is separated from README to keep the entry page manageable.
- Command behavior may change in future OpenSpec versions -> The document records the checked version and uses common stable CLI commands.
