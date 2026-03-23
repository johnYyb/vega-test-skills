---
name: use-vega-loading-indicator
description: 'Generates Vega loading indicator examples for Vega apps. Use this skill when the user wants to show a loading spinner, show a loading bar, use VegaLoader.load, close a loader with VegaLoader.close, display a page loading overlay, display a loading indicator inside a container, show determinate or indeterminate loading progress, or choose between VegaLoader and a direct VegaLoadingIndicator component in JavaScript, React, Angular, Vue, Stencil, or Next.js.'
---

# Use Vega Loading Indicator

## Purpose

Use this skill when the user wants to show loading state in a Vega app.

This skill is specifically for:
- Showing a page-level loading overlay with `VegaLoader.load(...)`.
- Showing a loading indicator inside a specific container.
- Closing an active loader with `VegaLoader.close(...)` after work completes.
- Configuring loading indicator shape, size, mode, percent, label, hint, and status.
- Choosing between the `VegaLoader` API and a directly rendered `VegaLoadingIndicator` component.
- Returning ready-to-paste usage for JavaScript, React, Angular, Vue, Stencil, or Next.js style projects.
- Recommending where loading logic should live in the application flow.

## When To Use This Skill

Use this skill when the user:
- Wants to show a loading indicator in Vega.
- Asks how to use `VegaLoader.load(...)`.
- Wants to block a page while data or a workflow is loading.
- Wants to show loading progress inside a panel, card, or other container.
- Wants a spinner or loading bar for async work.
- Wants a determinate progress indicator with `percent`.
- Wants to know when to use `VegaLoader` versus a directly rendered `VegaLoadingIndicator`.
- Wants a full page or component example that triggers loading behavior.
- Wants loading behavior added to an existing Vega app, even if the trigger location is not fully specified yet.

If the request is specifically about Vega loading indicators, always use this skill first. Missing Vega dependencies are not a reason to skip the skill; instead, use the skill to provide the correct integration guidance and usage pattern.

## Required Vega API

Use these APIs exactly for overlay-style loading indicators:

```ts
VegaLoader.load(options?: VegaLoaderOptions): string | null
VegaLoader.close(id: string): void

type VegaLoaderOptions = {
  containerSelector?: string;
  indicatorOptions?: VegaLoaderIndicatorOptions;
};

type VegaLoaderIndicatorOptions = {
  shape?: 'circle' | 'bar';
  size?: ResponsiveType<'small' | 'default' | 'large'>;
  mode?: 'indeterminate' | 'determinate';
  percent?: number;
  label?: string;
  hint?: string;
  status?: 'default' | 'success' | 'error';
};
```

Also use a directly rendered `VegaLoadingIndicator` component when the user needs an inline or persistent loading indicator instead of an overlay created through `VegaLoader`.

## Inputs To Collect

Ask only for the missing information:
- Target framework or runtime.
- Whether the user wants a page-level overlay or an inline/container-scoped loading indicator.
- Whether the loading state is indeterminate or determinate.
- Desired shape, size, label, hint, and status.
- Whether the loader should target the whole page or a specific container.
- What async work should trigger the loader and when it should close.
- Whether the user wants a full page example or only a trigger/snippet.
- Whether the target app is SSR-based, such as Next.js.
- Target file or component if the user wants code changes applied.

If the user wants a determinate progress example but does not provide the progress source, ask what process drives the percentage or provide a placeholder progress update flow and say it should be replaced with the real progress source.

## Rules

1. Use `VegaLoader.load(...)` for temporary loading overlays that should appear during async work and then be removed.
2. Prefer a directly rendered `VegaLoadingIndicator` component when the user needs a persistent inline loading state inside the normal layout.
3. In module-based apps, import `VegaLoader` from `@heartlandone/vega`.
4. In plain script or global usage, `window.VegaLoader` may be used after Vega has loaded.
5. Trigger `VegaLoader.load(...)` from event handlers, async workflow callbacks, controller functions, route-transition logic, or lifecycle code that runs on the client. Never call it directly during render.
6. Store the return value from `VegaLoader.load(...)` and close the loader only when a non-null `id` is returned.
7. Close loaders in a `finally` path when they wrap async work so the overlay is removed on both success and failure.
8. Use `containerSelector` only when the selector resolves to the intended container element.
9. Use `mode: 'determinate'` only when the app can provide meaningful `percent` updates.
10. Use `mode: 'indeterminate'` when the completion percentage is unknown.
11. Use `shape: 'bar'` when the user wants progress-bar style feedback and `shape: 'circle'` for spinner-style feedback.
12. Keep `percent` within a valid percentage range and pair it with a clear label or hint when determinate progress needs context.
13. Use `status: 'success'` or `status: 'error'` only when the loading indicator is meant to communicate the end-state visually.
14. If the user asks for a page, return a complete minimal page or component file, not only isolated snippets.
15. If useful for debugging, include a note that `VegaLoader.load(...)` returns the loader `id` or `null`.
16. If the current project does not yet have Vega installed, still use this skill, but do not install packages as part of the skill output.
17. If Vega is missing, tell the user which package or packages they need before the code will work.
18. If Vega is already installed, do not include package installation messaging.
19. When generating new code for an existing app, follow that app's established conventions for framework usage, file placement, import style, component structure, naming, styling, and state patterns instead of forcing a generic template.
20. Prefer adapting loading examples to the host app's existing async flow and layout structure. Only fall back to the generic templates in this skill when the app structure is unknown or the user asks for a standalone example.
21. In SSR frameworks such as Next.js, trigger `VegaLoader` on the client side only.

## Expected Output

Structure the answer in this order:
1. Short explanation of whether `VegaLoader` or a direct `VegaLoadingIndicator` is the better fit.
2. Trigger or setup note explaining where the loading logic should live.
3. Framework-specific example.
4. Optional close or cleanup snippet when lifecycle control needs emphasis.
5. Optional verification or debugging note when it would help.

Keep the output concrete. If the user asks for a loading flow, return complete code rather than describing the process abstractly.
If the user asks for a page, prefer a complete runnable example that includes a visible trigger and the loading behavior in the same output.
If required packages are missing, include a short note telling the user what to install. If the packages are already present, omit that note.
When the request targets an existing app, tailor the output to that app's current code style and structure rather than returning a generic sample.

## Framework Templates

### JavaScript Or Web Component Usage

```html
<vega-flex direction="col" gap="size-16">
  <vega-button id="show-loader">Show Loader</vega-button>
  <vega-box id="loader-container" style="position: relative; min-height: 160px;"></vega-box>
</vega-flex>

<script type="module">
  document.querySelector('#show-loader')?.addEventListener('click', async () => {
    const loaderId = window.VegaLoader.load({
      containerSelector: '#loader-container',
      indicatorOptions: {
        shape: 'circle',
        size: 'small',
        label: 'Loading orders',
        hint: 'This may take a few seconds.'
      }
    });

    try {
      await fetchOrders();
    } finally {
      if (loaderId) {
        window.VegaLoader.close(loaderId);
      }
    }
  });
</script>
```

### React Usage

```tsx
import { VegaButton } from '@heartlandone/vega-react';
import { VegaLoader } from '@heartlandone/vega';

export function LoadingExample(): JSX.Element {
  async function handleLoadCustomers(): Promise<void> {
    const loaderId = VegaLoader.load({
      indicatorOptions: {
        shape: 'bar',
        mode: 'indeterminate',
        label: 'Loading customers',
        hint: 'Retrieving account data.'
      }
    });

    try {
      await loadCustomers();
    } finally {
      if (loaderId) {
        VegaLoader.close(loaderId);
      }
    }
  }

  return <VegaButton onClick={handleLoadCustomers}>Load Customers</VegaButton>;
}
```

### Angular, Vue, Or Stencil-Style Usage

```ts
import { VegaLoader } from '@heartlandone/vega';

export async function refreshDashboard(): Promise<void> {
  const loaderId = VegaLoader.load({
    containerSelector: '#dashboard-panel',
    indicatorOptions: {
      shape: 'circle',
      size: 'default',
      label: 'Refreshing dashboard'
    }
  });

  try {
    await fetchDashboardData();
  } finally {
    if (loaderId) {
      VegaLoader.close(loaderId);
    }
  }
}
```

### Next.js Usage Note

```tsx
'use client';

import { VegaLoader } from '@heartlandone/vega';
```

### Determinate Progress Example

```ts
const loaderId = VegaLoader.load({
  indicatorOptions: {
    shape: 'bar',
    mode: 'determinate',
    percent: 45,
    label: 'Uploading file',
    hint: 'Do not close this page.'
  }
});

if (loaderId) {
  // Close the current loader and reopen with updated progress when the next percentage is available.
  VegaLoader.close(loaderId);
}
```

## Loading Notes

- `VegaLoader.load(...)` is useful for blocking async flows such as page bootstrapping, data refreshes, submissions, and navigation transitions.
- `VegaLoader.load(...)` appends an overlay and returns the loader `id` when the loader is created.
- Use a direct `VegaLoadingIndicator` component when the loading state should remain in the page layout instead of covering the page or container.
- Container-scoped loading is a better fit than a full-page overlay when only one panel or region is waiting on data.
- If the user asks for code applied in the repo, place the loading logic in the existing async path instead of firing it at module load time.
- For global or playground-style pages, using `window.VegaLoader` is acceptable after Vega has loaded. For module-based apps, prefer direct imports.

## Example Request

`Show me how to display a Vega loading overlay while a React page fetches data.`

`Create a Vega page that shows a loading indicator inside a container while a report loads.`

`Help me decide whether to use VegaLoader or a direct VegaLoadingIndicator for a long-running process.`

## Example Behavior

- Import `VegaLoader` from `@heartlandone/vega` for module-based apps.
- Open the loader from a client-side async action and store the returned `id`.
- Close the loader in a cleanup path after the work completes.
- Use `containerSelector` when only one region should be blocked.
- Prefer a direct `VegaLoadingIndicator` component when the loading UI should stay inline in the normal page layout.