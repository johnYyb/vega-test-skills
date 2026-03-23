---
applyTo: "skills/**/SKILL.md"
description: "Shared authoring rules for Vega skill files in this repository. Use when creating or updating any SKILL.md so new Vega skills follow the same structure, trigger coverage, API precision, output contract, and framework guidance as the existing skills."
---

# Vega Skill Authoring Rules

These instructions apply to every skill under `skills/**/SKILL.md` in this repository.

## Goal

Every Vega skill should help an AI coding agent produce code that is:

- specific to one Vega task
- based on the correct Vega APIs
- concrete enough to paste into a real project
- adaptable to an existing app instead of forcing a generic template
- consistent with the style and behavior of the other Vega skills in this repo

## Frontmatter Rules

- The `name` value must match the skill folder name exactly.
- The `description` must be explicit, trigger-rich, and written for discovery.
- The `description` should say what the skill generates and include phrases such as `Use this skill when the user wants to...` in natural language form.
- Include likely user intents, important Vega APIs, common framework names, and common aliases the user may say.
- Quote the `description` when it contains colons or other YAML-sensitive content.

## Scope Rules

- Each skill should focus on one Vega capability or one tightly related workflow.
- Do not combine unrelated Vega tasks into one skill.
- If a request is clearly about the skill's topic, instruct the agent to use that skill first.
- Missing dependencies are not a reason to skip the skill. The skill should still provide the correct integration guidance.

## Required Section Pattern

Keep the section structure aligned with the existing skills. New skills should normally include these sections in this order:

1. `# <Skill Title>`
2. `## Purpose`
3. `## When To Use This Skill`
4. `## Required Vega API`
5. `## Inputs To Collect`
6. `## Rules`
7. `## Expected Output`
8. `## Framework Templates`
9. A topic-specific notes section such as `## Dialog Notes`
10. `## Example Request`
11. `## Example Behavior`

Add a section only when it improves the skill. Do not add filler.

## API Rules

- Document the exact Vega APIs the agent should prefer.
- Include function signatures, relevant types, allowed option values, and important return types when they affect usage.
- Use `Use these APIs exactly:` or equivalent language when API precision matters.
- If the skill has an alternative component or pattern, explain when to use the standard API and when to use the alternative.
- Do not describe unsupported options, invented APIs, or guessed behavior.

## Input Collection Rules

- Tell the agent to ask only for missing information.
- Prefer a short, practical input list tied to implementation needs.
- If a required asset is missing, such as SVG markup, translation resources, dialog content, or action behavior, tell the agent either to ask for it or provide a clearly labeled placeholder example.
- If the user wants code changes applied, include a `Target file or component` input item.

## Shared Output Rules

- The skill output must stay concrete and implementation-oriented.
- If the user asks for a page, return a complete minimal page or component file, not isolated fragments.
- If the user provides enough detail, return ready-to-use code instead of abstract explanation.
- Prefer short setup guidance followed by code.
- When helpful, include an optional verification or debugging snippet.

## Existing App Rules

- When the request targets an existing app, instruct the agent to follow that app's established conventions for framework usage, file placement, import style, component structure, naming, styling, and state patterns.
- Prefer adapting examples to the host app's existing architecture and interaction flow.
- Only fall back to a generic template when the app structure is unknown or the user explicitly wants a standalone example.

## Dependency Rules

- Do not tell the agent to install packages as part of the skill output.
- If Vega dependencies are missing, tell the user which package or packages are needed before the code will work.
- If Vega is already installed, omit package installation messaging.

## Framework Coverage Rules

- Include framework-specific guidance only when it adds practical value to the skill.
- Prefer these template buckets when applicable:
  - JavaScript or web component usage
  - React usage
  - Angular, Vue, or Stencil-style usage
  - Next.js usage note for SSR-sensitive behavior
- If the behavior depends on browser APIs, DOM availability, overlays, or runtime globals, state that SSR frameworks such as Next.js must run that logic on the client side only.

## Behavior Rules For Vega Skills

- Put one-time Vega initialization in application startup, module initialization, or another shared bootstrap path.
- Do not place one-time setup in render bodies or other frequently executed code paths.
- Trigger user-facing behaviors such as dialogs or notifications from event handlers, async workflow callbacks, controller logic, or other action paths, not during render.
- If the API returns a value that matters later, such as an ID or element handle, call that out in the rules and examples.
- Include short warnings for behavior that commonly causes integration mistakes.

## Writing Style Rules

- Write in direct instructional language.
- Prefer short rule statements with clear implementation impact.
- Keep examples realistic and minimal.
- Use consistent headings and phrasing across skills so the repository reads like one system.
- Avoid vague statements such as `use best practices` unless the exact practice is stated immediately after.

## Skill Checklist

Before considering a new skill complete, verify that it:

- has frontmatter with an accurate `name` and discovery-focused `description`
- clearly states when the skill should and should not be used
- documents the exact Vega APIs to prefer
- asks only for the missing inputs needed to implement the feature
- includes rules for missing Vega dependencies
- tells the agent how to behave in an existing app versus a standalone example
- returns complete code when the user asks for a page or full component
- includes framework examples only where they materially help
- includes at least one example request and one example behavior summary

## Repository Baseline

New Vega skills should preserve the repository's current baseline:

- one skill per Vega task
- exact API guidance
- reusable framework examples
- explicit integration rules
- concrete output expectations
- consistency across JavaScript, React, and component-based app patterns