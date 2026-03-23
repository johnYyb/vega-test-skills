---
name: use-vega-white-labeling
description: 'Generates Vega white labeling examples for Vega apps. Use this skill when the user wants to override Vega theme colors for a white label client, use VegaThemeManager.overrideColors, reset custom theme colors with VegaThemeManager.reset, apply custom actionColor or sidebarColor values, or create JavaScript, React, Angular, Vue, Stencil, or Next.js examples for Vega white labeling.'
---

# Use Vega White Labeling

## Purpose

Use this skill when the user wants to apply client-specific color overrides to a Vega app for a white label implementation.

This skill is specifically for:
- Applying client-specific color overrides with `VegaThemeManager.overrideColors(...)`.
- Resetting custom color overrides with `VegaThemeManager.reset()`.
- Configuring `actionColor` and `sidebarColor` for white label clients.
- Explaining when white labeling should and should not be used.
- Returning ready-to-paste usage for JavaScript, React, Angular, Vue, Stencil, or Next.js style projects.
- Recommending where white label theme initialization should live.

## When To Use This Skill

Use this skill when the user:
- Wants to white label a Vega app for a client.
- Wants to override Vega colors with client branding.
- Asks how to use `VegaThemeManager.overrideColors(...)`.
- Wants to customize action button or form accent color.
- Wants to customize the side navigation background color.
- Wants to reset white label overrides back to Vega defaults.
- Wants white label behavior added to an existing Vega app, even if the startup location is not fully specified yet.
- Wants a full page or component example that applies client branding colors.

If the request is specifically about Vega white labeling, always use this skill first. Missing Vega dependencies are not a reason to skip the skill; instead, use the skill to provide the correct integration guidance and startup pattern.

## Required Vega API

Use these APIs exactly:

```ts
VegaThemeManager.overrideColors(customColors: VegaThemeOverrideColors): void
VegaThemeManager.reset(): void
VegaThemeManager.toggleDarkMode(enabled: boolean): void

type VegaThemeOverrideColors = {
  actionColor?: string;
  sidebarColor?: string;
};
```

White labeling overrides are only supported in light mode.

## Inputs To Collect

Ask only for the missing information:
- Target framework or runtime.
- Whether the app is a true white label client implementation or a standard Global Payments-branded product.
- Desired `actionColor` and `sidebarColor` values.
- Whether the user wants startup-only setup or a runtime client-brand switcher.
- Whether the user wants a reset flow back to the Vega defaults.
- Whether the target app is SSR-based, such as Next.js.
- Whether the user wants a full page example or only a snippet.
- Target file or component if the user wants code changes applied.

If the user wants white labeling but does not provide brand colors, ask for the color values or provide a clearly labeled placeholder example that must be replaced.

## Rules

1. Use `VegaThemeManager.overrideColors(...)` only for true white label client scenarios.
2. Do not recommend white labeling for standard Global Payments-branded products.
3. White labeling is only available in light mode, so force or restore light mode before applying overrides.
4. Use `VegaThemeManager.toggleDarkMode(false)` before applying white label overrides when dark mode may already be active.
5. Use `VegaThemeManager.reset()` to remove the override settings and return to the default Vega styles.
6. In module-based apps, import `VegaThemeManager` from `@heartlandone/vega`.
7. In plain script or global usage, `window.VegaThemeManager` may be used after Vega has loaded.
8. Initialize white label overrides once at application startup, module initialization, or another shared bootstrap path.
9. Never place one-time white label setup inside a render body or other frequently executed path.
10. If the user wants runtime switching between clients, centralize the update path so both color overrides and reset behavior are managed in one place.
11. If the user asks for a page, return a complete minimal page or component file, not only isolated snippets.
12. If useful for debugging, include a reset example and a quick note that dark mode must be off for overrides to take effect.
13. If the current project does not yet have Vega installed, still use this skill, but do not install packages as part of the skill output.
14. If Vega is missing, tell the user which package or packages they need before the code will work.
15. If Vega is already installed, do not include package installation messaging.
16. When generating new code for an existing app, follow that app's established conventions for framework usage, file placement, import style, component structure, naming, styling, and state patterns instead of forcing a generic template.
17. Prefer adapting the white label example to the host app's existing branding or tenant configuration architecture. Only fall back to the generic templates in this skill when the app structure is unknown or the user asks for a standalone example.
18. In SSR frameworks such as Next.js, apply the override on the client side only.
19. Remind the user that Vega does not save or fetch client color choices for them; persistence and data retrieval belong to the host application.
20. Encourage accessible color choices because Vega only performs limited contrast protection around custom primary color usage.

## Expected Output

Structure the answer in this order:
1. Short explanation of where white label setup should live and whether the request is a real white label use case.
2. Initialization snippet with light mode enforcement and color override setup.
3. Framework-specific page or render snippet.
4. Optional reset snippet when the user needs to remove the override.
5. Optional accessibility or debugging note when it would help.

Keep the output concrete. If the user asks for a white label flow, return complete code rather than describing the process abstractly.
If the user asks for a page, prefer a complete runnable example that includes theme setup and visible Vega components in the same output.
If required packages are missing, include a short note telling the user what to install. If the packages are already present, omit that note.
When the request targets an existing app, tailor the output to that app's current code style and structure rather than returning a generic sample.

## Framework Templates

### JavaScript Or Web Component Usage

```html
<vega-button id="apply-client-theme">Apply Client Theme</vega-button>
<vega-button id="reset-client-theme" variant="secondary">Reset Theme</vega-button>

<script type="module">
  const customColors = {
    actionColor: '#3c9f55',
    sidebarColor: '#274f1c'
  };

  document.querySelector('#apply-client-theme')?.addEventListener('click', () => {
    window.VegaThemeManager.toggleDarkMode(false);
    window.VegaThemeManager.overrideColors(customColors);
  });

  document.querySelector('#reset-client-theme')?.addEventListener('click', () => {
    window.VegaThemeManager.reset();
  });
</script>
```

### React Usage

```tsx
import { useEffect } from 'react';
import { VegaButton } from '@heartlandone/vega-react';
import { VegaThemeManager } from '@heartlandone/vega';

const clientTheme = {
  actionColor: '#3c9f55',
  sidebarColor: '#274f1c'
};

export function WhiteLabelExample(): JSX.Element {
  useEffect(() => {
    VegaThemeManager.toggleDarkMode(false);
    VegaThemeManager.overrideColors(clientTheme);

    return () => {
      VegaThemeManager.reset();
    };
  }, []);

  function handleResetTheme(): void {
    VegaThemeManager.reset();
  }

  return <VegaButton onClick={handleResetTheme}>Reset White Label Theme</VegaButton>;
}
```

### Angular, Vue, Or Stencil-Style Usage

```ts
import { VegaThemeManager } from '@heartlandone/vega';

export function applyClientTheme(): void {
  VegaThemeManager.toggleDarkMode(false);
  VegaThemeManager.overrideColors({
    actionColor: '#3c9f55',
    sidebarColor: '#274f1c'
  });
}

export function resetClientTheme(): void {
  VegaThemeManager.reset();
}
```

### Next.js Usage Note

```tsx
'use client';

import { VegaThemeManager } from '@heartlandone/vega';
```

### Reset Snippet

```ts
VegaThemeManager.reset();
```

## White Labeling Notes

- White labeling should only be used for white label client scenarios, not for standard Global Payments-branded products.
- White labeling is not supported in dark mode.
- Applying `overrideColors(...)` while dark mode is active will have no effect because Vega dark mode colors remain active.
- Vega does not manage how client color selections are stored or fetched; that belongs to the host application.
- Only action and sidebar colors are configurable through the white labeling API.
- If the user asks for code applied in the repo, place white label setup in the app's branding or tenant bootstrap flow instead of scattering it across components.

## Example Request

`Show me how to apply white label client colors in a Vega app.`

`Create a Vega React example that applies custom actionColor and sidebarColor values for a client.`

`Help me reset Vega white label overrides back to the default theme.`

## Example Behavior

- Import `VegaThemeManager` from `@heartlandone/vega` for module-based apps.
- Force light mode before applying white label color overrides.
- Apply `actionColor` and `sidebarColor` through `VegaThemeManager.overrideColors(...)`.
- Use `VegaThemeManager.reset()` to remove the white label override.
- Keep persistence and tenant lookup logic in the host application.