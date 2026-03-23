---
name: use-vega-dark-mode
description: 'Generates Vega dark mode examples for Vega apps. Use this skill when the user wants to enable dark mode, disable dark mode, toggle dark mode with VegaThemeManager.toggleDarkMode, check the current theme with VegaThemeManager.isDarkMode, create a theme switcher, persist a light or dark preference, or create JavaScript, React, Angular, Vue, Stencil, or Next.js examples for Vega dark mode.'
---

# Use Vega Dark Mode

## Purpose

Use this skill when the user wants to enable, disable, or manage dark mode in a Vega app.

This skill is specifically for:
- Enabling dark mode with `VegaThemeManager.toggleDarkMode(true)`.
- Disabling dark mode with `VegaThemeManager.toggleDarkMode(false)`.
- Reading the current dark mode state with `VegaThemeManager.isDarkMode()`.
- Creating a user-facing light or dark mode toggle.
- Explaining where theme initialization should live.
- Returning ready-to-paste usage for JavaScript, React, Angular, Vue, Stencil, or Next.js style projects.
- Explaining how Vega components and Vega CSS classes respond to dark mode automatically.

## When To Use This Skill

Use this skill when the user:
- Wants to use dark mode in Vega.
- Asks how to enable or disable Vega dark mode.
- Asks how to use `VegaThemeManager.toggleDarkMode(...)`.
- Wants a theme switcher for light mode and dark mode.
- Wants to check whether dark mode is currently enabled.
- Wants theme preference restored when the app starts.
- Wants dark mode behavior added to an existing Vega app, even if the bootstrap location is not fully specified yet.
- Wants a full page or component example that toggles between themes.

If the request is specifically about Vega dark mode, always use this skill first. Missing Vega dependencies are not a reason to skip the skill; instead, use the skill to provide the correct integration guidance and startup pattern.

## Required Vega API

Use these APIs exactly:

```ts
VegaThemeManager.toggleDarkMode(enabled: boolean): void
VegaThemeManager.isDarkMode(): boolean
```

Dark mode support is automatic for Vega components and Vega CSS classes when the theme is toggled through `VegaThemeManager`.

## Inputs To Collect

Ask only for the missing information:
- Target framework or runtime.
- Whether the user wants startup-only theme initialization or a visible theme switcher.
- Whether the app should default to light mode or dark mode.
- Whether the user wants the theme preference persisted across sessions.
- Whether the target app is SSR-based, such as Next.js.
- Whether the user wants a full page example or only a snippet.
- Target file or component if the user wants code changes applied.

If the user wants persistence but does not specify the storage mechanism, provide a minimal example such as `localStorage` and say it can be replaced with the app's preferred persistence layer.

## Rules

1. Use `VegaThemeManager.toggleDarkMode(true)` to enable dark mode and `VegaThemeManager.toggleDarkMode(false)` to disable it.
2. Use `VegaThemeManager.isDarkMode()` when the current theme state must be read for initialization, toggle UI state, or debugging.
3. Initialize the desired theme once at application startup, module initialization, or another shared bootstrap path.
4. Never place one-time theme initialization inside a render body or other frequently executed path.
5. In module-based apps, import `VegaThemeManager` from `@heartlandone/vega`.
6. In plain script or global usage, `window.VegaThemeManager` may be used after Vega has loaded.
7. Trigger visible theme changes from user actions, startup restoration logic, or client-side app initialization, not during server rendering.
8. If the user wants a toggle UI, keep the toggle state synchronized with `VegaThemeManager.isDarkMode()`.
9. If the user wants preference persistence, restore the saved value before or as the app initializes its visible theme.
10. If the user asks for a page, return a complete minimal page or component file, not only isolated snippets.
11. If useful for debugging, include a quick `VegaThemeManager.isDarkMode()` verification step.
12. If the current project does not yet have Vega installed, still use this skill, but do not install packages as part of the skill output.
13. If Vega is missing, tell the user which package or packages they need before the code will work.
14. If Vega is already installed, do not include package installation messaging.
15. When generating new code for an existing app, follow that app's established conventions for framework usage, file placement, import style, component structure, naming, styling, and state patterns instead of forcing a generic template.
16. Prefer adapting the dark mode example to the host app's existing theme architecture. Only fall back to the generic templates in this skill when the app structure is unknown or the user asks for a standalone example.
17. In SSR frameworks such as Next.js, apply theme changes on the client side only.

## Expected Output

Structure the answer in this order:
1. Short explanation of where theme initialization or toggle logic should live.
2. Initialization or toggle setup snippet.
3. Framework-specific page or render snippet.
4. Optional persistence snippet when the user wants theme restoration.
5. Optional verification snippet when debugging would benefit from it.

Keep the output concrete. If the user asks for a theme toggle flow, return complete code rather than describing the process abstractly.
If the user asks for a page, prefer a complete runnable example that includes theme setup and a visible way to switch themes in the same output.
If required packages are missing, include a short note telling the user what to install. If the packages are already present, omit that note.
When the request targets an existing app, tailor the output to that app's current code style and structure rather than returning a generic sample.

## Framework Templates

### JavaScript Or Web Component Usage

```html
<vega-button id="toggle-theme">Toggle Theme</vega-button>

<script type="module">
  const savedTheme = localStorage.getItem('vega-theme');
  const initialDarkMode = savedTheme === 'dark';

  window.VegaThemeManager.toggleDarkMode(initialDarkMode);

  document.querySelector('#toggle-theme')?.addEventListener('click', () => {
    const nextDarkMode = !window.VegaThemeManager.isDarkMode();
    window.VegaThemeManager.toggleDarkMode(nextDarkMode);
    localStorage.setItem('vega-theme', nextDarkMode ? 'dark' : 'light');
  });
</script>
```

### React Usage

```tsx
import { useEffect, useState } from 'react';
import { VegaButton } from '@heartlandone/vega-react';
import { VegaThemeManager } from '@heartlandone/vega';

export function DarkModeExample(): JSX.Element {
  const [darkMode, setDarkMode] = useState(false);

  useEffect(() => {
    const savedTheme = localStorage.getItem('vega-theme');
    const initialDarkMode = savedTheme === 'dark';

    VegaThemeManager.toggleDarkMode(initialDarkMode);
    setDarkMode(VegaThemeManager.isDarkMode());
  }, []);

  function handleToggleTheme(): void {
    const nextDarkMode = !VegaThemeManager.isDarkMode();
    VegaThemeManager.toggleDarkMode(nextDarkMode);
    localStorage.setItem('vega-theme', nextDarkMode ? 'dark' : 'light');
    setDarkMode(VegaThemeManager.isDarkMode());
  }

  return (
    <div>
      <p>Current theme: {darkMode ? 'Dark' : 'Light'}</p>
      <VegaButton onClick={handleToggleTheme}>Toggle Theme</VegaButton>
    </div>
  );
}
```

### Angular, Vue, Or Stencil-Style Usage

```ts
import { VegaThemeManager } from '@heartlandone/vega';

export function applySavedTheme(): void {
  const savedTheme = localStorage.getItem('vega-theme');
  VegaThemeManager.toggleDarkMode(savedTheme === 'dark');
}

export function toggleTheme(): void {
  const nextDarkMode = !VegaThemeManager.isDarkMode();
  VegaThemeManager.toggleDarkMode(nextDarkMode);
  localStorage.setItem('vega-theme', nextDarkMode ? 'dark' : 'light');
}
```

### Next.js Usage Note

```tsx
'use client';

import { VegaThemeManager } from '@heartlandone/vega';
```

### Verification Snippet

```ts
console.log(VegaThemeManager.isDarkMode());
```

## Dark Mode Notes

- Vega components and Vega CSS classes support dark mode automatically because Vega colors are driven by design tokens.
- Theme initialization belongs in the app bootstrap or shared theme setup path, not inside frequently rerun render logic.
- A persisted theme example can use `localStorage`, but the skill should adapt to the host app's preferred persistence mechanism when one already exists.
- If the user asks for code applied in the repo, place the dark mode setup in the app's existing theme or bootstrap flow instead of scattering it across components.
- For global or playground-style pages, using `window.VegaThemeManager` is acceptable after Vega has loaded. For module-based apps, prefer direct imports.

## Example Request

`Show me how to enable dark mode in a Vega React app and let the user toggle it.`

`Create a Vega page that restores a saved dark mode preference on startup.`

`Help me check whether dark mode is currently enabled in Vega.`

## Example Behavior

- Import `VegaThemeManager` from `@heartlandone/vega` for module-based apps.
- Enable or disable dark mode with `VegaThemeManager.toggleDarkMode(...)`.
- Read the current theme with `VegaThemeManager.isDarkMode()`.
- Restore saved theme preference during client-side startup when persistence is needed.
- Keep toggle UI state synchronized with the actual Vega theme state.