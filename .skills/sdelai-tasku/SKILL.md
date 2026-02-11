---
name: sdelai-tasku
description: >-
  Use when the user asks "сделай задачу N/давай сделаем задачу N/сделай
  PREFIX-N/сделай следующую задачу/давай след задачку/го след задачку/го след
  задачу/го след таску/давай след задачу" to produce a detailed task brief:
  pull requirements and business criteria from specs, read task status/log from
  tasks files, and output a technical execution plan plus expected business
  benefit before any implementation.
---

# Sdelai Tasku

## Overview

Prepare a detailed task brief before implementation. The skill must work even if filenames differ across repositories.

Always resolve and require 4 artifacts:
1. dev-plan,
2. project/feature spec,
3. arch-patterns,
4. task file.

If arch-patterns is missing, create a draft file in `specs/` and continue.

Do not implement code until user explicitly says `делай`.

## Workflow

### 1) Resolve the 4 mandatory artifacts

#### 1.1 Dev-plan
- Search under `specs/` recursively for markdown files with probable names:
  - `dev-plan.md`, `dev_plan.md`, `development-plan.md`, `plan.md`.
- Prefer file containing both keys: `task_prefix` and `tasks_dir`.
- If multiple candidates remain, choose deterministically (sorted path, first).
- If none found, ask user for explicit path.

#### 1.2 Project/feature spec
- Prefer file in same folder as resolved dev-plan.
- Name priority:
  - `TECH_SPEC.md`
  - `tech-spec.md`
  - `SPEC.md`
  - `spec.md`
  - any `*spec*.md` excluding dev-plan and arch-patterns files.
- If none found, ask user for explicit path.

#### 1.3 Arch-patterns
- Search under `specs/` recursively for probable names:
  - `arch-patterns.md`, `arch_patterns.md`, `architecture-patterns.md`.
- If missing:
  - create `specs/arch-patterns.md` with draft template;
  - then use this new file.

Draft template:
```md
# Architecture Patterns

## Active Rules

### AP-001 Modular Boundaries
- status: active
- rule: Feature/business modules depend on ports/interfaces, not concrete integrations.

### AP-002 Composition Root Only
- status: active
- rule: Wiring of concrete adapters is allowed only in bootstrap/composition root.

### AP-003 Error Contracts
- status: active
- rule: External integration errors must be normalized to explicit domain/application errors.
```

#### 1.4 Task file
- Read resolved dev-plan and extract:
  - `task_prefix`
  - `tasks_dir`
- Parse user request:
  - if user gave number `N` -> `{task_prefix}-N`;
  - if user gave explicit `{PREFIX}-N` and prefix differs from dev-plan -> ask confirmation;
  - if user asked "next task" alias:
    - scan `{tasks_dir}/{task_prefix}-*.md`;
    - parse `Статус` and `## Зависимости`;
    - valid next: `Статус=todo` and all dependencies are `done` or `нет`;
    - if multiple valid, pick first in sorted order;
    - if none valid, report first blocked todo and blockers.
- Resolve task file path. If missing, ask to create task.

### 2) Load task status/history only

From task file read only:
- `Статус` / `Status`
- `## Зависимости` / `Dependencies`
- `## Лог` / `Log` (last 3 entries)

Do not use task description as source of requirements.

### 3) Load requirements and rules

Read:
- resolved project/feature spec,
- resolved arch-patterns file.

All requirement details and business criteria must come from spec.
Only active architecture rules are applied.

### 4) Build task brief

Output sections:
1. **Task key** (plus short title from task heading).
2. **Resolved artifact paths** (all 4 files).
3. **Current status + last logs**.
4. **Detailed requirements (from spec)**:
   - data/model,
   - API contracts,
   - validation/permissions,
   - runtime/integration behavior.
5. **Technical execution plan** (1-7 concrete steps, file-level when possible).
6. **Expected business benefit** (strictly from spec; list assumptions if unclear).
7. **Applied architecture rules** (rule IDs).
8. **Risks / open questions**.
9. Confirmation prompt: **"делай?"**

### 5) No implementation before approval

Before explicit `делай`:
- no code/spec changes,
- no task status updates.

### 6) Post-approval logging (mandatory)

After user says `делай`, before any code/spec change:
- set task status to `in_progress`;
- append start log entry with timestamp.

After each meaningful change block:
- append log entry.

Before handoff:
- set status `review`;
- append summary log entry.

### 7) Review-change rule

If task is in `review` and changes are requested:
- set status to `need_changes` with reason (`review_feedback` or `requirements_changed`);
- append log entry with reason;
- only then continue changes.

If task was `done` and requirements changed:
- reopen to `need_changes` (`requirements_changed`),
- preserve prior requirements/history,
- continue `in_progress` -> `review` -> `done`.
