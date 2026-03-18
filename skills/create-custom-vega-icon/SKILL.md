---
name: create-custom-vega-icon
description: Generates VegaIconManager registration code and framework-specific usage for custom SVG icons in Vega.
---

# Create Custom Vega Icon

## Purpose

Use this skill when the user wants to register a custom SVG icon with `VegaIconManager` and render it through Vega components.

This skill is specifically for:
- Registering one or more custom SVG icons.
- Creating a page or component that displays the custom icon.
- Returning ready-to-paste usage for JavaScript, React, Angular, Vue, or Stencil-style projects.
- Recommending where icon registration should live.
- Explaining when `autoSize` should be used.

## When To Use This Skill

Use this skill when the user:
- Wants to add a custom icon to a Vega app.
- Wants to create a Vega page that uses a custom icon.
- Provides SVG markup and wants working Vega usage.
- Asks how to use `VegaIconManager`.
- Wants a custom icon rendered with `vega-icon` or `VegaIcon`.
- Wants to convert SVG assets into reusable Vega icon registrations.
- Wants a Vega custom icon added to an existing page or component, even if the current app has not installed Vega yet.

If the request is specifically about a Vega custom icon, always use this skill first. Missing Vega dependencies are not a reason to skip the skill; instead, use the skill to provide the integration guidance and startup registration pattern.

## Required Vega API

Use these APIs exactly:

```ts
VegaIconManager.register(icons: Record<string, IconPayload>): void
VegaIconManager.getIcon(name: string): Nullable<IconPayload>
VegaIconManager.hasIcon(name: string): boolean

type IconPayload = {
  icon: string;
  autoSize?: boolean;
};
```

## Inputs To Collect

Ask only for the missing information:
- Icon name.
- SVG markup.
- Target framework or runtime.
- Whether the user wants a full page example or only a component/snippet.
- Whether the icon should preserve intrinsic SVG dimensions.
- Target file or component if the user wants code changes applied.

If the user only describes the icon but does not provide the SVG, ask for the SVG asset or provide a placeholder example and say it needs to be replaced.

## Rules

1. Register the icon once at application startup or module initialization.
2. Never place `VegaIconManager.register(...)` inside a render body or other frequently executed path.
3. Import `VegaIconManager` from `@heartlandone/vega`.
4. In React, render with `VegaIcon` from `@heartlandone/vega-react`.
5. In Angular, Vue, Stencil, or plain web component usage, render with `<vega-icon icon="icon-name"></vega-icon>` unless the project already wraps it differently.
6. Recommend `autoSize: true` only when the SVG width and height should be preserved by `vega-icon`.
7. If the icon name could collide with an existing icon, suggest a more specific name such as `brand-visa-icon`.
8. Explain that `VegaIconManager.register(...)` does not overwrite an existing icon with the same name, so unique names are safer for reusable examples.
9. If the user asks for a page, return a complete minimal page or component file, not only isolated snippets.
10. If useful for debugging, include a quick `VegaIconManager.hasIcon(name)` check.
11. If the current project does not yet have Vega installed, still use this skill, but do not install packages as part of the skill output.
12. If Vega is missing, tell the user which package or packages they need to install before the code will work.
13. If Vega is already installed, do not include package installation messaging.
14. When generating new code for an existing app, follow that app's established conventions for framework usage, file placement, import style, component structure, naming, styling, and state patterns instead of forcing a generic template.
15. Prefer adapting the Vega icon example to the host app's existing architecture. Only fall back to the generic templates in this skill when the app structure is unknown or the user asks for a standalone example.

## Expected Output

Structure the answer in this order:
1. Short explanation of where registration should live.
2. Registration snippet.
3. Framework-specific page or render snippet.
4. Optional verification snippet when debugging would benefit from it.

Keep the output concrete. If the user provides SVG markup, return complete code rather than describing the process abstractly.
If the user asks for a page, prefer a complete runnable example that includes both icon registration and visible usage in the same output.
If required packages are missing, include a short note telling the user what to install. If the packages are already present, omit that note.
When the request targets an existing app, tailor the output to that app's current code style and structure rather than returning a generic sample.

## Framework Templates

### JavaScript Or Web Component Usage

```html
<vega-flex direction="col" gap="size-24">
  <vega-box>
    <vega-font>Custom Icon</vega-font>
    <vega-flex gap="size-8" align-items="center">
      <vega-icon icon="custom-icon"></vega-icon>
      <vega-icon icon="custom-icon" size="size-24"></vega-icon>
      <vega-icon icon="custom-icon" size="size-48"></vega-icon>
    </vega-flex>
  </vega-box>
</vega-flex>

<script type="module">
  window.VegaIconManager.register({
    'custom-icon': {
      icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16"><path d="..." /></svg>'
    }
  });
</script>
```

### React Usage

```tsx
import { VegaIcon } from '@heartlandone/vega-react';
import { VegaIconManager } from '@heartlandone/vega';

VegaIconManager.register({
  'custom-icon': {
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16"><path d="..." /></svg>'
  }
});

export function Example(): JSX.Element {
  return (
    <div>
      <VegaIcon icon="custom-icon"></VegaIcon>
      <VegaIcon icon="custom-icon" size="size-24"></VegaIcon>
    </div>
  );
}
```

### Angular, Vue, Or Stencil-Style Usage

```ts
import { VegaIconManager } from '@heartlandone/vega';

VegaIconManager.register({
  'custom-icon': {
    icon: '<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 16 16"><path d="..." /></svg>'
  }
});
```

```html
<vega-icon icon="custom-icon"></vega-icon>
```

## Usage Notes

- For SVGs with fixed dimensions such as `width="36" height="24"`, consider `autoSize: true` if the icon should retain that aspect and size behavior.
- If the user has multiple icons, register them in a single `VegaIconManager.register({ ... })` call.
- If the user asks for a page, include a visible layout example such as `vega-flex`, `vega-box`, and `vega-font` so the custom icon usage is obvious.
- If the user wants the code applied in the repo, place the registration in the app bootstrap or shared initialization layer instead of the component render path.
- If the user is replacing an icon during development, changing the icon key is safer than assuming `register()` will overwrite the previous one.
- If the skill is used inside an existing app, mirror the app's current coding conventions so the new icon code blends into the codebase naturally.

## Example Request

`Create a custom Vega icon named brand-visa-icon for React using this SVG and preserve its original size.`

`Create a Vega page in plain JavaScript that registers a custom icon and shows it in a header section.`

## Example Behavior

- Import `VegaIconManager` from `@heartlandone/vega`.
- Import `VegaIcon` from `@heartlandone/vega-react` for React.
- Register `brand-visa-icon` with `autoSize: true` when preserving intrinsic dimensions is required.
- Render it with `<VegaIcon icon="brand-visa-icon"></VegaIcon>`.