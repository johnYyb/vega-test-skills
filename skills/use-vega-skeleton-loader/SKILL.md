---
name: use-vega-skeleton-loader
description: 'Generates Vega skeleton loader examples for Vega apps. Use this skill when the user wants to show placeholder content, use VegaSkeletonLoader.load, remove a skeleton with VegaSkeletonLoader.unLoad, remove all skeleton loaders with VegaSkeletonLoader.unLoadAll, create text, paragraph, image, ellipse, or table skeleton layouts, or choose between VegaSkeletonLoader and direct VegaSkeleton components in JavaScript, React, Angular, Vue, Stencil, or Next.js.'
---

# Use Vega Skeleton Loader

## Purpose

Use this skill when the user wants to show placeholder content in a Vega app while real content is loading.

This skill is specifically for:
- Showing one or more skeleton placeholders with `VegaSkeletonLoader.load(...)`.
- Removing a specific skeleton loader with `VegaSkeletonLoader.unLoad(...)`.
- Removing all skeleton loaders with `VegaSkeletonLoader.unLoadAll()`.
- Configuring skeleton placeholder type, width, height, items, and layout.
- Choosing between the `VegaSkeletonLoader` API and directly rendered `VegaSkeleton` components.
- Returning ready-to-paste usage for JavaScript, React, Angular, Vue, Stencil, or Next.js style projects.
- Recommending where skeleton-loading logic should live in the application flow.

## When To Use This Skill

Use this skill when the user:
- Wants to show skeleton placeholder content in Vega.
- Asks how to use `VegaSkeletonLoader.load(...)`.
- Wants to display text, paragraph, image, ellipse, or table placeholders while content loads.
- Wants multiple skeleton placeholders managed together in one target area.
- Wants to remove a skeleton loader after data arrives.
- Wants to clear all skeleton loaders from a page.
- Wants to know when to use `VegaSkeletonLoader` versus direct `VegaSkeleton` components.
- Wants a full page or component example that shows skeleton state and content replacement.
- Wants skeleton behavior added to an existing Vega app, even if the target layout is not fully specified yet.

If the request is specifically about Vega skeleton placeholders, always use this skill first. Missing Vega dependencies are not a reason to skip the skill; instead, use the skill to provide the correct integration guidance and usage pattern.

## Required Vega API

Use these APIs exactly when the user wants to manage placeholder skeletons through the helper API:

```ts
VegaSkeletonLoader.load(config: VegaSkeletonLoaderConfig): string
VegaSkeletonLoader.unLoad(key: string): void
VegaSkeletonLoader.unLoadAll(): void

type VegaSkeletonLoaderConfig = {
  target: string;
  type?: VegaSkeletonLoaderItem;
  width?: number | string | SpacingTokenType;
  height?: number | string | SpacingTokenType;
  items?: VegaSkeletonLoaderItem[];
  layout?: VegaSkeletonLoaderLayout;
};

type VegaSkeletonLoaderItem = {
  type: 'text' | 'paragraph' | 'full-width-paragraph' | 'ellipse' | 'image' | 'table';
  width: number | string | SpacingTokenType;
  height: number | string | SpacingTokenType;
};

type VegaSkeletonLoaderLayout = {
  props?: Partial<VegaComponentInterface<HTMLVegaFlexElement>>;
  style?: Partial<CSSStyleDeclaration>;
};
```

Also use directly rendered `VegaSkeleton` components when the user needs inline placeholder markup that is already controlled by the framework render tree instead of by the skeleton loader API.

When a direct component example is needed, prefer these component properties:

```ts
type VegaSkeletonProps = {
  type?: 'text' | 'paragraph' | 'full-width-paragraph' | 'ellipse' | 'image' | 'table';
  width?: number | string;
  height?: number | string;
  animated?: boolean;
  corners?: CornerRadiusToken | CornerRadiusConfig;
};
```

## Inputs To Collect

Ask only for the missing information:
- Target framework or runtime.
- Whether the user wants API-managed skeleton placeholders or direct inline `VegaSkeleton` components.
- The target selector or container that should receive the skeleton loader.
- The placeholder shape or content pattern needed, such as text rows, paragraphs, images, ellipses, or tables.
- Whether the user needs one skeleton item or multiple coordinated items.
- Desired width, height, animation, and layout behavior.
- What async work should show the skeleton and when it should be removed.
- Whether the user wants a full page example or only a trigger/snippet.
- Whether the target app is SSR-based, such as Next.js.
- Target file or component if the user wants code changes applied.

If the user wants a complex skeleton layout but does not describe the placeholder structure, ask for the intended layout or provide a minimal placeholder layout and say it should be replaced with the real one.

## Rules

1. Use `VegaSkeletonLoader.load(...)` when the user wants to manage one or more skeleton placeholders in a target container through an imperative API.
2. Prefer direct `VegaSkeleton` components when the placeholder should be rendered inline as part of the normal framework render tree.
3. In module-based apps, import `VegaSkeletonLoader` from `@heartlandone/vega`.
4. Trigger `VegaSkeletonLoader.load(...)` from client-side async flows, setup logic, route transitions, or event handlers. Never call it directly during render.
5. The `target` selector must resolve to the intended element before loading the skeleton.
6. Store the key returned from `VegaSkeletonLoader.load(...)` when the app needs to remove that specific skeleton later.
7. Remove skeleton placeholders in a cleanup path after the real content is ready.
8. Use `VegaSkeletonLoader.unLoadAll()` only when the app intentionally clears every active skeleton loader on the page.
9. Use `items` when the placeholder layout needs multiple coordinated skeleton shapes.
10. Use the `type` field for a single placeholder shape or each item shape, and only use supported values.
11. Match placeholder width and height closely to the real content structure so the layout shift stays small.
12. Use `layout.props` and `layout.style` only when the surrounding skeleton arrangement needs explicit flex or style control.
13. If the user asks for a page, return a complete minimal page or component file, not only isolated snippets.
14. If useful for debugging, include a note that `VegaSkeletonLoader.load(...)` returns the skeleton loader key.
15. If the current project does not yet have Vega installed, still use this skill, but do not install packages as part of the skill output.
16. If Vega is missing, tell the user which package or packages they need before the code will work.
17. If Vega is already installed, do not include package installation messaging.
18. When generating new code for an existing app, follow that app's established conventions for framework usage, file placement, import style, component structure, naming, styling, and state patterns instead of forcing a generic template.
19. Prefer adapting skeleton examples to the host app's real content layout so the placeholder shape matches the final UI.
20. Only fall back to a generic template when the app structure is unknown or the user asks for a standalone example.
21. In SSR frameworks such as Next.js, run `VegaSkeletonLoader` on the client side only.

## Expected Output

Structure the answer in this order:
1. Short explanation of whether `VegaSkeletonLoader` or direct `VegaSkeleton` components are the better fit.
2. Trigger or setup note explaining where the skeleton logic should live.
3. Framework-specific example.
4. Optional cleanup snippet when lifecycle control needs emphasis.
5. Optional verification or debugging note when it would help.

Keep the output concrete. If the user asks for a skeleton flow, return complete code rather than describing the process abstractly.
If the user asks for a page, prefer a complete runnable example that includes placeholder content and the content replacement flow in the same output.
If required packages are missing, include a short note telling the user what to install. If the packages are already present, omit that note.
When the request targets an existing app, tailor the output to that app's current code style and structure rather than returning a generic sample.

## Framework Templates

### JavaScript Or Web Component Usage

```html
<vega-box class="customer-card" style="min-height: 180px;"></vega-box>

<script type="module">
  const skeletonKey = window.VegaSkeletonLoader.load({
    target: '.customer-card',
    items: [
      { type: 'image', width: '120px', height: '80px' },
      { type: 'text', width: 'size-160', height: 'size-16' },
      { type: 'paragraph', width: '100%', height: 'size-48' }
    ],
    layout: {
      props: {
        direction: 'col',
        gap: 'size-12'
      }
    }
  });

  loadCustomerCard().finally(() => {
    window.VegaSkeletonLoader.unLoad(skeletonKey);
  });
</script>
```

### React Usage

```tsx
import { useEffect, useRef, useState } from 'react';
import { VegaSkeleton } from '@heartlandone/vega-react';

export function SkeletonExample(): JSX.Element {
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    let active = true;

    loadOrderSummary().finally(() => {
      if (active) {
        setLoading(false);
      }
    });

    return () => {
      active = false;
    };
  }, []);

  if (loading) {
    return (
      <div>
        <VegaSkeleton type="text" width="140px" height="16px" animated={true}></VegaSkeleton>
        <VegaSkeleton type="paragraph" width="100%" height="56px" animated={true}></VegaSkeleton>
        <VegaSkeleton type="image" width="100%" height="160px" animated={true}></VegaSkeleton>
      </div>
    );
  }

  return <OrderSummaryContent></OrderSummaryContent>;
}
```

### Angular, Vue, Or Stencil-Style Usage

```ts
import { VegaSkeletonLoader } from '@heartlandone/vega';

export async function showAccountSkeleton(): Promise<void> {
  const skeletonKey = VegaSkeletonLoader.load({
    target: '#account-panel',
    items: [
      { type: 'ellipse', width: '48px', height: '48px' },
      { type: 'text', width: '140px', height: '16px' },
      { type: 'full-width-paragraph', width: '100%', height: '64px' }
    ],
    layout: {
      props: {
        direction: 'col',
        gap: 'size-12'
      }
    }
  });

  try {
    await fetchAccountDetails();
  } finally {
    VegaSkeletonLoader.unLoad(skeletonKey);
  }
}
```

### Next.js Usage Note

```tsx
'use client';

import { VegaSkeletonLoader } from '@heartlandone/vega';
```

### Remove All Skeleton Loaders

```ts
VegaSkeletonLoader.unLoadAll();
```

## Skeleton Notes

- `VegaSkeletonLoader` is useful when several placeholders in the same region must be added and removed together.
- Direct `VegaSkeleton` components are usually a better fit for React or other component-driven rendering when the placeholder is naturally part of the normal layout state.
- Match skeleton shapes to the final content structure to reduce layout shift and make the placeholder feel intentional.
- Use `animated={true}` when the placeholder should feel visibly active.
- If the user asks for code applied in the repo, place the skeleton show and cleanup logic in the app's existing async flow instead of firing it at module load time.

## Example Request

`Show me how to use VegaSkeletonLoader while a dashboard card loads.`

`Create a React example that uses VegaSkeleton components until data is ready.`

`Help me decide whether to use VegaSkeletonLoader or direct VegaSkeleton components for a list page.`

## Example Behavior

- Import `VegaSkeletonLoader` from `@heartlandone/vega` for API-managed placeholders.
- Import `VegaSkeleton` from `@heartlandone/vega-react` when the placeholder should be rendered inline in React.
- Store the returned loader key when the skeleton needs to be removed later.
- Remove the skeleton when the real content is ready.
- Prefer direct component skeletons for framework-managed layout state and `VegaSkeletonLoader` for imperative multi-placeholder flows.