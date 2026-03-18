# Vega Skill Repository Introduction

## Purpose Of This Repository

This repository is a Vega skill repository for AI coding assistants. Its goal is to help AI generate Vega-related code with clearer rules, correct Vega API usage, and outputs that better match real project structures.

It is not a runnable Vega application. Instead, it is a set of capability definitions for AI. Each skill tells the AI:

- when the skill should be used
- which Vega APIs should be preferred
- where the code should live in an application
- which implementation rules must be followed
- how examples should be returned for different frameworks

The current repository covers these Vega scenarios:

- custom icon registration and rendering
- dialog and modal display flows
- translation initialization and language switching

## Why This Repository Is Needed

Without skill-based guidance, AI can still generate code, but the results often have a few recurring problems:

- the output stays too conceptual and is not ready to use
- Vega APIs may be used inconsistently or incorrectly
- initialization logic, event handling, and rendering logic may be placed in the wrong location
- integration patterns vary too much across frameworks
- repeated prompts for similar tasks may produce unstable results

The value of this repository is that it turns implicit engineering knowledge into explicit instructions, so the AI has a consistent basis for implementation.

More specifically, it helps solve these problems:

### 1. Standardize The AI's Understanding Of Vega

By defining fixed APIs, usage rules, and output structures in each skill, the repository reduces ambiguity and helps the AI reason about Vega more accurately.

### 2. Improve The Practicality Of Generated Code

The skills require the AI to return more complete examples instead of loose fragments. That matters when the output needs to be integrated into a real project.

### 3. Better Fit Real Engineering Workflows

The skills emphasize practical patterns such as putting initialization at startup, triggering dialogs from user actions, and centralizing translation resources. That is more useful than API notes alone.

### 4. Reduce Repeated Clarification Work

When a team asks AI to solve similar Vega tasks repeatedly, the skills make the answers more consistent in style, implementation, and technical decisions.

## Typical Use Cases

This repository is useful when you want to:

- use AI to create new Vega pages or features
- integrate Vega capabilities into an existing project
- get Vega examples that are more stable and closer to project conventions
- capture Vega best practices and reuse them across team members and projects

If your goal is to make AI behave more like an engineer who understands Vega constraints and integration patterns, this repository is designed for that purpose.

## Repository Contents

The current structure is:

```text
skills/
  create-custom-vega-icon/
    SKILL.md
  display-vega-dialog/
    SKILL.md
  use-vega-translation/
    SKILL.md
```

Each `SKILL.md` file is an independent capability unit.

For example:

- `create-custom-vega-icon`: guides the AI to register custom SVG icons correctly and render them in React, JavaScript, or other frameworks
- `display-vega-dialog`: guides the AI to use `VegaDialog.open(...)` or `vega-modal` for standard dialogs and custom modal flows
- `use-vega-translation`: guides the AI to initialize `VegaTranslation`, manage translation resources, and switch languages

## How To Use This Repository

### 1. Install The Skill Repository

Run this in the target project:

```bash
npx skills add johnYyb/vega-test-skills
```

After installation, the AI coding assistant can use the skills in this repository when handling relevant Vega tasks.

### 2. Trigger Skills With The Right Requests

When you ask the AI to perform a Vega-related task, more specific prompts usually produce better results.

For example:

```text
Help me register a custom Vega icon in a React page and show a usage example.
```

```text
Help me add a delete confirmation dialog in a Vega app and trigger it from a button click.
```

```text
Help me initialize VegaTranslation and implement English and Chinese language switching.
```

Requests like these are more likely to match the correct skill and produce accurate code.

### 3. Provide Project Context For Better Output

The goal of this repository is not only to produce generic examples, but also to help the AI generate output that fits the actual project structure.

For that reason, it helps to provide:

- the framework in use, such as React, Vue, Angular, or plain JavaScript
- the target file or component
- whether you want a full page, a component, or just a snippet
- whether Vega dependencies are already installed

The more context you provide, the closer the result will be to directly usable code.

### 4. Update Skills When Needed

If the skills in this repository are updated, run:

```bash
npx skills update johnYyb/vega-test-skills
```

That allows the AI to use the latest rules and examples in future tasks.

## Recommended Usage Pattern

To get the most value from this repository, use it as follows:

- treat it as the shared knowledge entry point for Vega-related tasks
- when adding new Vega features, let AI generate the first draft based on these skills
- keep turning repeated Vega implementation patterns into new skills
- let team members collaborate around the same skill set to reduce implementation drift

## Future Todo List

The repository still has a few next-step items that should be tracked explicitly:

1. Move this repository to an official repository.
2. Add more skills that cover additional Vega usage scenarios.
3. Define a clear update communication strategy so users know whether they should update their installed skills when the skills change or when a new Vega version introduces breaking changes.

This third item matters because skill behavior may need to change when Vega APIs, patterns, or recommended integrations change. Users should be told when an update is optional, when it is recommended, and when it is required because of a breaking change.

## Summary

The main goal of this skill repository is not just to tell AI what Vega is. It is to help AI handle Vega development tasks in a way that:

- uses the right APIs
- puts logic in the right place
- follows implementation constraints
- returns usable code
- stays consistent across frameworks

If you are using AI to help build Vega applications, this repository can act as a stable engineering layer that helps the AI move from simply answering questions to becoming a more reliable development collaborator.