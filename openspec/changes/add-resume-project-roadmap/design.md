## Context

The learner has embedded and IoT-related project background around WS63, LiteOS, MPU6050, fall detection, wireless alerting, and OpenSpec learning. A strong portfolio project should combine these existing strengths with edge AI and IoT backend capabilities.

## Goals / Non-Goals

**Goals:**

- Recommend one highly suitable resume project.
- Provide enough detail for the learner to execute without needing to redesign the plan.
- Connect the project to embedded systems, edge AI, IoT communication, backend visualization, and OpenSpec.
- Include resume bullets and interview talking points.

**Non-Goals:**

- Do not implement the actual firmware or backend in this learning repository.
- Do not claim medical certification or production readiness.

## Decisions

- Choose an edge AI fall detection and emergency alert system because it matches the learner's existing codebase direction and has strong career relevance.
- Use a 12-week roadmap to make execution realistic.
- Include MVP, advanced ML, and future productization stages so the project can grow over time.

## Risks / Trade-offs

- The full project is larger than a simple demo -> The roadmap is split into weekly milestones and MVP/advanced stages.
- TinyML deployment may be difficult on the target board -> The plan allows gateway-side edge inference as an honest intermediate step.
- Safety-sensitive domain -> The document clearly marks the project as a learning prototype, not a medical device.
