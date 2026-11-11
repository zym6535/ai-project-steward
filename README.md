[简体中文](./README.zh-CN.md) | **English**

# Design Rationale for `AGENTS.md` and the Knowledge Project Steward Skill

## 1. Background

This approach is designed for knowledge-work projects that evolve over time.

Consider a product launch project. It may involve:

- defining the target audience and project scope;
- collecting interviews, market data, and competitor research;
- extracting needs and key insights;
- comparing pricing, channel, and promotion strategies;
- making important decisions;
- producing launch plans, presentations, and execution templates;
- continuously revising deliverables based on feedback.

Projects like this are often advanced through natural conversation. When working with AI, we might say:

- “Let’s focus on small and midsize teams first.”
- “We won’t invest in Channel B for now.”
- “Use this launch plan as the current version.”
- “We still need more interviews before confirming this conclusion.”
- “Keep the old proposal for reference, but do not use it as an execution guide.”

These statements may look like ordinary conversation, but they can represent:

- a change in project scope;
- a deferred option;
- the designation of an authoritative version;
- a conclusion that is still pending validation;
- an older version that should move into the archive.

If this information remains only in chat, the project can easily suffer from lost context, repeated discussions, unclear version ownership, and broken continuity.

I therefore want AI to do more than answer the immediate question. It should also take on part of the role of a project knowledge steward:

> The user advances the work through natural conversation, while AI identifies tasks, conclusions, decisions, deliverables, and open questions, then maintains them in the appropriate places.

## 2. Why Use Both `AGENTS.md` and a Skill?

They operate at different levels.

I think of them this way:

- `AGENTS.md` is the project’s local collaboration charter.
- The Skill is AI’s working method for managing a knowledge project.

### `AGENTS.md`: Project-Level Rules

`AGENTS.md` answers the question:

> What conventions should AI follow in this particular project?

For example, it can define:

- which top-level directories the project uses;
- where current knowledge, working materials, and deliverables belong;
- how task directories should be named;
- how daily iterations should be recorded;
- which maintenance actions AI may perform directly;
- which significant changes require user confirmation;
- how AI should report file changes back to the user.

These rules are specific to the project. Another project may use a different directory structure, naming convention, or permission boundary.

### The Skill: A General Decision-Making Framework

The Skill answers the question:

> How should AI identify and maintain project information with lasting value from natural conversation?

It helps AI determine:

- whether the current exchange is temporary or has become an ongoing task;
- whether the user is still exploring or has made a decision;
- whether a conclusion is stable;
- whether a work record should be created;
- which information deserves to become established project knowledge;
- which information should remain tentative;
- when a task is entering its closing phase;
- which materials should be retained and which no longer have lasting value.

The Skill should not impose one directory structure or naming convention on every project. It follows the project’s own `AGENTS.md` first.

The resulting order of authority is:

```text
The user’s current explicit request
        ↓
The project’s AGENTS.md
        ↓
The project’s established knowledge
        ↓
The general Skill workflow
```

## 3. Core Design Principles

### Use Natural Conversation as the Entry Point

The user should not have to explicitly declare things such as:

- “This is a new task.”
- “This should be recorded now.”
- “This phase is complete.”
- “Please invoke a particular Skill.”

AI evaluates the conversation together with the project’s current state and determines which phase the work is in.

It should notify the user or request confirmation only when its judgment would affect the project’s direction, established conclusions, important files, or directory structure.

This allows project management to support the user’s thinking instead of interrupting it.

### Organize Content by Task Purpose

AI first identifies the task purpose served by a piece of material.

For example, a product launch project may contain tasks such as:

```text
Target audience research
Pricing strategy
Launch channel selection
Launch planning
Post-launch evaluation
```

Under this model:

- user interviews, audience profiles, and needs analysis belong to “Target audience research”;
- price modeling, option comparisons, and pricing decisions belong to “Pricing strategy”;
- channel data, channel comparisons, and media plans belong to “Launch channel selection.”

This keeps the context, analysis, decisions, and deliverables for the same task connected.

### Separate Materials by Their Current Role

After identifying the task, AI determines the role each material currently plays within that task.

The same task may appear across several top-level directories. For example, “Pricing strategy” may include:

```text
docs/pricing-strategy/          Current pricing principles
work/pricing-strategy/          Pricing validation still in progress
outputs/pricing-strategy/       A ready-to-use pricing proposal
references/pricing-strategy/    Market data and interview evidence
archive/pricing-strategy/       Older pricing proposals that were rejected
```

These are not five different tasks. They are five material layers of the same task.

### Give Content a Clear Lifecycle

In real work, ideas usually move gradually from exploration to stability. The project structure should reflect that lifecycle:

```text
Natural conversation
   ↓
Ongoing exploration
   ↓
work: an active workstream takes shape
   ↓
Confirmation, validation, or convergence
   ├── docs: established knowledge
   ├── outputs: formal deliverables
   ├── references: sources and evidence
   └── archive: superseded versions and completed process materials
```

Suppose we say:

> “We may want to use a subscription model, but we still need to validate customer acceptance.”

This is still a tentative option and should remain in the work record.

After interviews and pricing tests, we may confirm:

> “We will formally adopt a two-tier subscription model: Basic and Professional.”

AI can then:

- promote the pricing principles into `docs/`;
- place the final price sheet in `outputs/`;
- retain interviews and test results in `references/`;
- move rejected alternatives into `archive/`.

### Keep One Current Authoritative Version per Topic

A project may retain historical versions, but it should clearly identify which version is currently in use.

For a product launch plan:

- the current launch plan belongs in `outputs/`;
- the adopted strategy and key decisions belong in `docs/`;
- market research and budget evidence belong in `references/`;
- older plans and completed design work belong in `archive/`;
- the daily iteration log records what was confirmed or changed that day.

Other locations should refer to the authoritative file through concise summaries and links, rather than storing competing full copies.

### Provide Both Task and Timeline Views

Task-oriented directories answer:

> Where can I find everything related to target audience research or the product launch plan?

The `iteration/` directory answers:

> What exactly moved forward on a particular day?

The project therefore provides two complementary views:

```text
Task view: what purpose does this content serve?
Timeline view: what changed in the project each day?
```

A daily iteration log may record:

- which tasks were involved;
- what was completed;
- which decisions were made;
- which important files changed;
- the current result;
- remaining open questions;
- the next step.

The log stores summaries and links only. It does not duplicate established project content.

### Balance Automatic Maintenance with User Control

AI may perform lightweight, reversible maintenance directly, such as:

- updating the current work notes;
- recording decisions already confirmed by the user;
- repairing related links;
- updating the daily iteration log;
- promoting confirmed content into established project knowledge.

For changes with greater impact, AI should explain the consequences and request user confirmation first. These include:

- large-scale directory restructuring;
- deleting or archiving important files;
- overwriting formal deliverables;
- resolving conflicts between authoritative documents;
- changing the project’s direction;
- treating a conclusion as confirmed when the user’s position is unclear.

The goal is for AI to take responsibility for routine maintenance without taking over the user’s key decisions.

## 4. Overall Workflow

A typical collaboration flow can be summarized as follows:

```text
The user expresses ideas naturally
      ↓
The Skill evaluates the current state and information value
      ↓
AGENTS.md provides the project-specific rules
      ↓
AI updates the relevant work, knowledge, deliverables, or evidence
      ↓
AI records version relationships and daily progress when needed
      ↓
AI reports the substantive file changes to the user
```

The Skill determines *how to reason about the situation*. `AGENTS.md` determines *how that reasoning should be applied in this project*.

## 5. Expected Outcomes

### For the User

The user can focus on:

- understanding the problem;
- comparing options;
- expressing preferences;
- making decisions;
- reviewing deliverables.

The user does not need to take on extensive file-management work or learn a complex project-management process.

### For AI

AI can restore project context more reliably, including:

- the project’s current objective;
- which conclusions have been confirmed;
- which tasks are still active;
- where the current authoritative version lives;
- which sources support the conclusions;
- what changed recently;
- what should happen next.

This reduces forgotten context, accidental use of outdated versions, and repeated discussions.

### For the Project

The project should ultimately support four capabilities:

1. **Findability:** Relevant materials can be located quickly.
2. **Authority:** The current valid version can be identified with confidence.
3. **Traceability:** The sources of conclusions and the evolution of versions remain understandable.
4. **Continuity:** Work can continue across conversations, project phases, and even changes in collaborators.

The goal is not merely to keep folders tidy. It is to create a sustainable way for humans and AI to work together:

> The user advances knowledge work through natural conversation, while AI continuously turns the conversation into clear, reliable, traceable, and reusable project knowledge without adding management overhead.
