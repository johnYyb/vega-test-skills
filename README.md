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

### Asset And Identity

| Skill | Description |
|---|---|
| [use-vega-custom-icon](skills/use-vega-custom-icon/SKILL.md) | Generate Vega icon registration and usage examples for custom SVG icons across JavaScript, React, Angular, Vue, and similar app setups. |

### Lifecycle And Readiness

| Skill | Description |
|---|---|
| [use-vega-render-status](skills/use-vega-render-status/SKILL.md) | Generate Vega render-status examples, including waitForVega usage, post-render flow coordination, and framework-specific integration guidance. |

### Interaction And Feedback

| Skill | Description |
|---|---|
| [use-vega-dialog](skills/use-vega-dialog/SKILL.md) | Generate Vega dialog and modal examples, including confirmations, destructive actions, and framework-specific integration guidance. |
| [use-vega-loading-indicator](skills/use-vega-loading-indicator/SKILL.md) | Generate Vega loading indicator examples, including page overlays, container-scoped loaders, progress configuration, and framework-specific integration guidance. |
| [use-vega-skeleton-loader](skills/use-vega-skeleton-loader/SKILL.md) | Generate Vega skeleton loader examples, including placeholder layouts, managed removal flows, and framework-specific integration guidance. |
| [use-vega-notification](skills/use-vega-notification/SKILL.md) | Generate Vega notification examples, including toast display, dismiss behavior, action buttons, and framework-specific integration guidance. |

### Theme, Brand, And Localization

| Skill | Description |
|---|---|
| [use-vega-branding](skills/use-vega-branding/SKILL.md) | Generate Vega branding examples, including supported brand switching, official logo usage, and framework-specific integration guidance. |
| [use-vega-dark-mode](skills/use-vega-dark-mode/SKILL.md) | Generate Vega dark mode examples, including theme toggling, saved preference restoration, and framework-specific integration guidance. |
| [use-vega-translation](skills/use-vega-translation/SKILL.md) | Generate Vega translation examples, including initialization, language switching, and framework-specific integration guidance. |
| [use-vega-white-labeling](skills/use-vega-white-labeling/SKILL.md) | Generate Vega white labeling examples, including client-specific color overrides, reset behavior, and framework-specific integration guidance. |

## What These Skills Are For

Use this repository when you want an AI agent to help create or extend a Vega app. The skills in this repo guide the agent to:

- choose the correct Vega APIs for the task
- place Vega initialization code in the right application layer
- return ready-to-use examples instead of vague instructions
- adapt examples for common frameworks and runtime environments

Current coverage focuses on ten common Vega app tasks:

- registering and rendering custom Vega icons
- checking when Vega components have finished rendering
- opening standard dialogs or custom modal flows
- applying supported Vega branding
- enabling and managing dark mode
- showing loading overlays or container-scoped loading indicators
- showing skeleton placeholder layouts while content loads
- showing Vega notifications and dismiss flows
- initializing Vega translation resources and switching languages
- applying white label client color overrides

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
	use-vega-custom-icon/
		SKILL.md
	use-vega-render-status/
		SKILL.md
	use-vega-dialog/
		SKILL.md
	use-vega-branding/
		SKILL.md
	use-vega-dark-mode/
		SKILL.md
	use-vega-loading-indicator/
		SKILL.md
	use-vega-skeleton-loader/
		SKILL.md
	use-vega-notification/
		SKILL.md
	use-vega-translation/
		SKILL.md
	use-vega-white-labeling/
		SKILL.md
```
