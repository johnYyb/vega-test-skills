---
name: use-vega-render-status
description: 'Generates Vega render-status examples for Vega apps. Use this skill when the user wants to wait until Vega components finish rendering, use waitForVega, run code after Vega components are ready, coordinate post-render application flow, avoid timing issues with asynchronously rendered Vega components, or create JavaScript, React, Angular, Vue, Stencil, or Next.js examples for Vega render-status checks.'
---

# Use Vega Render Status

## Purpose

Use this skill when the user needs to wait for Vega components to finish their initial asynchronous render cycle before running dependent application logic.

This skill is specifically for:
- Waiting for Vega components to be ready with `waitForVega()`.
- Running code only after all Vega components on a page complete initial render.
- Preventing timing issues when app logic depends on fully rendered Vega components.
- Returning ready-to-paste usage for JavaScript, React, Angular, Vue, Stencil, or Next.js style projects.
- Recommending where render-status checks should live in the application flow.

## When To Use This Skill

Use this skill when the user:
- Wants to check whether Vega components have fully rendered.
- Asks how to use `waitForVega()`.
- Needs to run code only after Vega components are ready.
- Is hitting timing issues caused by asynchronous Vega rendering.
- Wants to wait for Vega before measuring layout, attaching dependent behavior, or continuing an app workflow.
- Wants render-status checks added to an existing Vega app, even if the exact lifecycle location is not fully specified yet.
- Wants a full page or component example that waits for Vega components before continuing.

If the request is specifically about Vega render status or waiting for Vega render completion, always use this skill first. Missing Vega dependencies are not a reason to skip the skill; instead, use the skill to provide the correct integration guidance and usage pattern.

## Required Vega API

Use these APIs exactly:

```ts
waitForVega(): Promise<Awaited<HTMLVegaElementType>[]>
```

Common usage forms are:

```ts
await waitForVega();
```

```ts
waitForVega().then(() => {
  // Run code after Vega components are ready.
});
```

## Inputs To Collect

Ask only for the missing information:
- Target framework or runtime.
- What logic should run after Vega components are ready.
- Whether the user wants startup-only guidance or a page/component example.
- Whether the target app is SSR-based, such as Next.js.
- Whether the wait should happen after a route change, page mount, or another client-side lifecycle step.
- Target file or component if the user wants code changes applied.

If the user wants to wait for Vega before running follow-up logic but does not specify that logic, ask what should happen after the components are ready or provide a placeholder callback and say it should be replaced.

## Rules

1. Use `waitForVega()` when application logic must run only after Vega components complete their initial render cycle.
2. In module-based apps, import `waitForVega` from `@heartlandone/vega`.
3. In plain async flows, prefer `await waitForVega()` when the surrounding function is already async.
4. Use `.then(...)` only when an async function wrapper is less appropriate for the host code style.
5. Trigger render-status checks from startup logic, mount effects, route-transition handlers, or other client-side lifecycle paths. Never call them directly during render.
6. Use `waitForVega()` before layout measurement, focus management, dependent DOM access, or follow-up UI logic that relies on fully rendered Vega components.
7. If the user asks for a page, return a complete minimal page or component file, not only isolated snippets.
8. If useful for debugging, include a simple confirmation log or follow-up action after `waitForVega()` resolves.
9. If the current project does not yet have Vega installed, still use this skill, but do not install packages as part of the skill output.
10. If Vega is missing, tell the user which package or packages they need before the code will work.
11. If Vega is already installed, do not include package installation messaging.
12. When generating new code for an existing app, follow that app's established conventions for framework usage, file placement, import style, component structure, naming, styling, and state patterns instead of forcing a generic template.
13. Prefer adapting render-status examples to the host app's existing lifecycle and async flow. Only fall back to the generic templates in this skill when the app structure is unknown or the user asks for a standalone example.
14. In SSR frameworks such as Next.js, call `waitForVega()` on the client side only.
15. Make it clear that `waitForVega()` is for initial Vega render readiness, not a general-purpose watcher for every later UI change.

## Expected Output

Structure the answer in this order:
1. Short explanation of where the render-status check should live.
2. Setup snippet with `waitForVega()`.
3. Framework-specific example.
4. Optional follow-up or verification snippet when debugging would help.

Keep the output concrete. If the user asks for a render-status flow, return complete code rather than describing the process abstractly.
If the user asks for a page, prefer a complete runnable example that includes Vega usage and a visible post-render action in the same output.
If required packages are missing, include a short note telling the user what to install. If the packages are already present, omit that note.
When the request targets an existing app, tailor the output to that app's current code style and structure rather than returning a generic sample.

## Framework Templates

### JavaScript Or Web Component Usage

```html
<vega-button id="run-after-vega">Run After Vega Ready</vega-button>

<script type="module">
  document.querySelector('#run-after-vega')?.addEventListener('click', async () => {
    await window.waitForVega();
    console.log('Vega components are ready');
    runPostRenderFlow();
  });
</script>
```

### React Usage

```tsx
import { useEffect, useState } from 'react';
import { waitForVega } from '@heartlandone/vega';

export function VegaReadyExample(): JSX.Element {
  const [ready, setReady] = useState(false);

  useEffect(() => {
    let active = true;

    waitForVega().then(() => {
      if (active) {
        setReady(true);
        runPostRenderFlow();
      }
    });

    return () => {
      active = false;
    };
  }, []);

  return <div>{ready ? 'Vega is ready' : 'Waiting for Vega...'}</div>;
}
```

### Angular, Vue, Or Stencil-Style Usage

```ts
import { waitForVega } from '@heartlandone/vega';

export async function initializeAfterVega(): Promise<void> {
  await waitForVega();
  runPostRenderFlow();
}
```

### Next.js Usage Note

```tsx
'use client';

import { waitForVega } from '@heartlandone/vega';
```

### Verification Snippet

```ts
await waitForVega();
console.log('Vega components are ready');
```

## Render Status Notes

- Vega components render asynchronously, so code that depends on their completed initial render should wait explicitly.
- `waitForVega()` resolves when Vega components on the page finish their initial render cycle.
- This utility is useful before layout measurement, focus management, dependent DOM logic, or follow-up workflows that assume Vega is ready.
- If the user asks for code applied in the repo, place the wait logic in the app's existing client-side lifecycle or startup flow instead of scattering it across unrelated components.
- For module-based apps, prefer direct imports. If a global utility is already exposed by the host app, adapt to that pattern instead of forcing a different one.

## Example Request

`Show me how to wait until Vega components finish rendering in React.`

`Create a Vega page that runs a callback only after Vega components are ready.`

`Help me fix a timing issue by using waitForVega before my post-render logic runs.`

## Example Behavior

- Import `waitForVega` from `@heartlandone/vega` for module-based apps.
- Wait for Vega components before running dependent post-render logic.
- Use `await waitForVega()` in async flows or `.then(...)` when that better matches the host code style.
- Run render-status checks from client-side lifecycle paths, not during render.
- Treat `waitForVega()` as an initial readiness gate, not a general-purpose state observer.