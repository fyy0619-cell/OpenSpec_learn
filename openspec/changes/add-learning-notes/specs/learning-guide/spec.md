## ADDED Requirements

### Requirement: Beginner guide explains the OpenSpec workflow
The repository SHALL provide a beginner-friendly guide that explains the OpenSpec workflow from initialization to archive.

#### Scenario: Reader follows the guide
- **WHEN** a beginner reads the guide
- **THEN** they can understand proposal, specs, design, tasks, validation, and archive

### Requirement: Guide includes project operation commands
The repository SHALL include command examples for initializing OpenSpec, creating changes, checking status, validating artifacts, and archiving completed changes.

#### Scenario: Reader practices commands
- **WHEN** a beginner copies the command examples into a terminal
- **THEN** they can operate an OpenSpec project step by step

### Requirement: Example change remains inspectable
The repository SHALL keep an example active change so learners can inspect the OpenSpec artifact structure.

#### Scenario: Reader inspects the example change
- **WHEN** a beginner opens `openspec/changes/add-learning-notes`
- **THEN** they can see README, proposal, design, specs, and tasks artifacts for one change
