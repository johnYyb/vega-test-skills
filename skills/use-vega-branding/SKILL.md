---
name: use-vega-branding
description: 'Generates Vega branding examples for Vega apps. Use this skill when the user wants to switch Vega branding with VegaThemeManager.applyBranding, apply Global Payments or Genius branding, configure branding before the app renders, use the global VegaThemeManagerApplyBranding setting, render official logos with VegaBrandLogo, or create JavaScript, React, Angular, Vue, Stencil, or Next.js examples for Vega branding.'
---

# Use Vega Branding

## Purpose

Use this skill when the user wants to apply an official supported Vega brand configuration such as Global Payments or Genius.

This skill is specifically for:
- Applying official Vega branding with `VegaThemeManager.applyBranding(...)`.
- Configuring branding before the app renders.
- Using the global `window.VegaThemeManagerApplyBranding` setup path when early imports are not practical.
- Rendering official brand logos with `VegaBrandLogo`.
- Returning ready-to-paste usage for JavaScript, React, Angular, Vue, Stencil, or Next.js style projects.
- Recommending where branding initialization should live.

## When To Use This Skill

Use this skill when the user:
- Wants to apply Global Payments branding in Vega.
- Wants to switch to Genius branding in Vega.
- Asks how to use `VegaThemeManager.applyBranding(...)`.
- Wants branding applied before the application renders.
- Wants to know how to use the global branding setting in vanilla JavaScript.
- Wants to render official Global Payments, Genius, or G symbol logos.
- Wants branding behavior added to an existing Vega app, even if the bootstrap location is not fully specified yet.
- Wants a full page or component example that applies branding.

If the request is specifically about Vega branding, always use this skill first. Missing Vega dependencies are not a reason to skip the skill; instead, use the skill to provide the correct integration guidance and startup pattern.

## Required Vega API

Use these APIs exactly:

```ts
VegaThemeManager.applyBranding(brandOptions: 'gp' | 'genius'): void

window.VegaThemeManagerApplyBranding = 'gp' | 'genius'
```

When direct logo rendering is needed, prefer the brand logo component with these supported properties:

```ts
type VegaBrandLogoProps = {
  name?: 'g-symbol' | 'genius' | 'global-payments';
  size?: 'small' | 'medium' | 'large';
  theme?: 'auto' | 'dark' | 'light';
};
```

## Inputs To Collect

Ask only for the missing information:
- Target framework or runtime.
- Which official brand should be applied: `gp` or `genius`.
- Whether the user wants startup-only branding setup or also wants direct logo rendering.
- Whether the user needs a full page example or only a snippet.
- Whether the target app is SSR-based, such as Next.js.
- Whether the user wants module import setup or the global configuration pattern.
- Target file or component if the user wants code changes applied.

If the user wants a logo example but does not specify which logo, ask whether they want `global-payments`, `genius`, or `g-symbol`, or provide a clear placeholder choice.

## Rules

1. Use `VegaThemeManager.applyBranding('gp' | 'genius')` to switch Vega to a supported official brand.
2. Apply branding before the app renders so the UI does not flash with the wrong brand.
3. In module-based apps, import `VegaThemeManager` from `@heartlandone/vega`.
4. In vanilla JavaScript or other early-boot scenarios where import timing is difficult, use `window.VegaThemeManagerApplyBranding` before Vega loads.
5. Use the supported branding values exactly: `gp` or `genius`.
6. Prefer `VegaBrandLogo` when the user needs an official logo rendered directly in the UI.
7. Use supported `VegaBrandLogo` names exactly: `global-payments`, `genius`, or `g-symbol`.
8. If the user asks for a page, return a complete minimal page or component file, not only isolated snippets.
9. If useful for debugging, include a note that branding should be applied before UI render to avoid mixed-brand output.
10. If the current project does not yet have Vega installed, still use this skill, but do not install packages as part of the skill output.
11. If Vega is missing, tell the user which package or packages they need before the code will work.
12. If Vega is already installed, do not include package installation messaging.
13. When generating new code for an existing app, follow that app's established conventions for framework usage, file placement, import style, component structure, naming, styling, and state patterns instead of forcing a generic template.
14. Prefer adapting the branding example to the host app's existing bootstrap and brand configuration architecture. Only fall back to the generic templates in this skill when the app structure is unknown or the user asks for a standalone example.
15. In SSR frameworks such as Next.js, apply branding on the client side unless the host app has a verified early client bootstrap path that runs before Vega renders.
16. Make it clear that this API is for supported official branding migration, not arbitrary client color overrides. Use the white labeling skill for custom client colors instead.

## Expected Output

Structure the answer in this order:
1. Short explanation of where branding setup should live.
2. Initialization snippet with the chosen branding path.
3. Framework-specific page or render snippet.
4. Optional logo snippet when the user also needs direct brand logo rendering.
5. Optional verification or migration note when it would help.

Keep the output concrete. If the user asks for a branding flow, return complete code rather than describing the process abstractly.
If the user asks for a page, prefer a complete runnable example that includes branding setup and visible branded Vega UI in the same output.
If required packages are missing, include a short note telling the user what to install. If the packages are already present, omit that note.
When the request targets an existing app, tailor the output to that app's current code style and structure rather than returning a generic sample.

## Framework Templates

### JavaScript Or Web Component Usage

```html
<script type="module">
  window.VegaThemeManagerApplyBranding = 'gp';
</script>

<script
  type="module"
  src="./node_modules/@heartlandone/vega/dist/vega/vega.esm.js"
></script>
```

```html
<vega-brand-logo name="global-payments" size="small" theme="auto"></vega-brand-logo>
```

### React Usage

```tsx
import { useEffect } from 'react';
import { VegaBrandLogo } from '@heartlandone/vega-react';
import { VegaThemeManager } from '@heartlandone/vega';

export function BrandingExample(): JSX.Element {
  useEffect(() => {
    VegaThemeManager.applyBranding('gp');
  }, []);

  return <VegaBrandLogo name="global-payments" size="medium" theme="auto"></VegaBrandLogo>;
}
```

### Angular, Vue, Or Stencil-Style Usage

```ts
import { VegaThemeManager } from '@heartlandone/vega';

export function applyGpBranding(): void {
  VegaThemeManager.applyBranding('gp');
}
```

```html
<vega-brand-logo name="genius" size="small" theme="auto"></vega-brand-logo>
```

### Next.js Usage Note

```tsx
'use client';

import { VegaThemeManager } from '@heartlandone/vega';
```

### Logo Snippet

```tsx
import { VegaBrandLogo } from '@heartlandone/vega-react';

export function GeniusLogo(): JSX.Element {
  return <VegaBrandLogo name="genius" size="large" theme="auto"></VegaBrandLogo>;
}
```

## Branding Notes

- `VegaThemeManager.applyBranding(...)` is intended for supported official brand migration, such as Global Payments and Genius.
- This branding API is different from white labeling. Use white labeling only for client-specific custom colors.
- Apply branding before the UI renders to avoid visual flashes or mixed-brand output.
- Branding updates action colors, navigation logos, and app footer branding across affected Vega components.
- If the user asks for code applied in the repo, place branding setup in the app's main bootstrap or theme initialization flow instead of spreading it across components.

## Example Request

`Show me how to apply Global Payments branding in a Vega app before render.`

`Create a React example that switches Vega to Genius branding and shows the official logo.`

`Help me use the global branding setup path in a vanilla JavaScript Vega app.`

## Example Behavior

- Import `VegaThemeManager` from `@heartlandone/vega` for module-based apps.
- Apply branding with `VegaThemeManager.applyBranding('gp')` or `VegaThemeManager.applyBranding('genius')`.
- Use `window.VegaThemeManagerApplyBranding` when branding must be set before Vega is imported in a vanilla setup.
- Render official logos with `VegaBrandLogo` using supported `name`, `size`, and `theme` values.
- Distinguish supported branding migration from white label custom color overrides.