# Agent Skills for Vega

**NOTE: This repository is currently in development and may change as the Vega skill set expands.**

This repository contains agent skills for building Vega apps with AI. The skills are designed to help an AI coding agent generate accurate Vega integration code, explain recommended usage patterns, and return framework-specific examples for common Vega tasks.

## Installation

To install these skills into your project, run:

```bash
npx skills add johnYyb/vega-test-skills
```

## Updating Skills

To update the installed skills, run:

```bash
npx skills update johnYyb/vega-test-skills
```

## Available Skills

| Skill | Description |
|---|---|
| [create-custom-vega-icon](skills/create-custom-vega-icon/SKILL.md) | Generate Vega icon registration and usage examples for custom SVG icons across JavaScript, React, Angular, Vue, and similar app setups. |
| [display-vega-dialog](skills/display-vega-dialog/SKILL.md) | Generate Vega dialog and modal examples, including confirmations, destructive actions, and framework-specific integration guidance. |
| [display-vega-notification](skills/display-vega-notification/SKILL.md) | Generate Vega notification examples, including toast display, dismiss behavior, action buttons, and framework-specific integration guidance. |
| [use-vega-translation](skills/use-vega-translation/SKILL.md) | Generate Vega translation initialization and language-switching examples for localized Vega apps. |

## What These Skills Are For

Use this repository when you want an AI agent to help create or extend a Vega app. The skills in this repo guide the agent to:

- choose the correct Vega APIs for the task
- place Vega initialization code in the right application layer
- return ready-to-use examples instead of vague instructions
- adapt examples for common frameworks and runtime environments

Current coverage focuses on four common Vega app tasks:

- registering and rendering custom Vega icons
- opening standard dialogs or custom modal flows
- showing Vega notifications and dismiss flows
- initializing Vega translation resources and switching languages

## Contributing

Contributions are easiest when each skill stays focused on one Vega task and includes concrete API guidance, usage rules, and runnable examples.

When adding or updating a skill:

- keep the frontmatter `name` and `description` accurate
- describe when the skill should and should not be used
- document the exact Vega APIs the agent should prefer
- include practical examples for the frameworks you want the agent to support

## Repository Structure

```text
skills/
	create-custom-vega-icon/
		SKILL.md
	display-vega-dialog/
		SKILL.md
	display-vega-notification/
		SKILL.md
	use-vega-translation/
		SKILL.md
```
