---
name: use-vega-notification
description: 'Generates Vega notification examples for Vega apps. Use this skill when the user wants to show a notification, display a toast, use VegaNotify.open, close a notification, close all notifications, add notification action buttons, customize success or error notification types, or create JavaScript, React, Angular, Vue, Stencil, or Next.js examples for Vega notifications.'
---

# Use Vega Notification

## Purpose

Use this skill when the user wants to display a notification in a Vega app.

This skill is specifically for:
- Showing toast-style notifications with `VegaNotify.open(...)`.
- Customizing notification title, message, duration, type, close button visibility, and action buttons.
- Closing a notification through its returned `id`.
- Closing all visible notifications with `VegaNotify.closeAll()`.
- Returning ready-to-paste usage for JavaScript, React, Angular, Vue, Stencil, or Next.js style projects.
- Recommending where notification-triggering code should live.

## When To Use This Skill

Use this skill when the user:
- Wants to display a notification in Vega.
- Asks how to use `VegaNotify.open(...)`.
- Wants a success, info, warning, or error notification.
- Wants a notification that auto-dismisses after a custom duration.
- Wants a notification that must be dismissed manually.
- Wants action buttons inside a notification.
- Wants to close a specific notification or close all notifications.
- Wants a full page or component example that triggers notifications.
- Wants notification behavior added to an existing Vega app, even if the trigger location is not fully specified yet.

If the request is specifically about showing notifications in Vega, always use this skill first. Missing Vega dependencies are not a reason to skip the skill; instead, use the skill to provide the correct integration guidance and usage pattern.

## Required Vega API

Use these APIs exactly:

```ts
VegaNotify.open(options: VegaNotifyOption): Promise<string>
VegaNotify.close(id: string): Promise<void>
VegaNotify.closeAll(): Promise<void>

type VegaNotifyOption = {
  title?: string;
  message: string;
  duration?: number;
  type?: 'success' | 'info' | 'warning' | 'error';
  showCloseButton?: boolean;
  actionButtons?: VegaPageNotificationActionButtonConfig[];
};

type VegaPageNotificationActionButtonConfig = {
  label: string;
  onVegaClick?: (e: CustomEvent) => void;
};
```

## Inputs To Collect

Ask only for the missing information:
- Target framework or runtime.
- Notification title and message.
- Notification type.
- Whether the notification should auto-dismiss or require manual dismissal.
- Desired duration if auto-dismiss behavior should be customized.
- Whether action buttons are needed and what they should do.
- Whether the user wants a full page example or only a trigger/snippet.
- Whether the target app is SSR-based, such as Next.js.
- Target file or component if the user wants code changes applied.

If the user wants a notification flow with actions but does not describe the action behavior, ask what each button should do or provide a minimal placeholder example and say it should be replaced with the real action logic.

## Rules

1. Use `VegaNotify.open(...)` for standard toast-style notifications.
2. Trigger notifications from event handlers, async workflow callbacks, controller functions, or other user or system action paths. Never call `VegaNotify.open(...)` directly during render.
3. In module-based apps, import `VegaNotify` from `@heartlandone/vega`.
4. In plain script or global usage, `window.VegaNotify` may be used after Vega has loaded.
5. `VegaNotify.open(...)` is async and returns the notification `id`, so store that `id` when the notification may need to be closed programmatically.
6. If the notification should auto-dismiss, omit `duration` or set it to a positive millisecond value.
7. If the notification must remain visible until the user acts, set `duration: 0`.
8. Use `type: 'success' | 'info' | 'warning' | 'error'` only when the visual meaning matters. Do not invent unsupported types.
9. Use `showCloseButton: true` when the user should have an explicit dismiss affordance, especially for longer-lived notifications.
10. If action buttons are present, keep labels short and make sure each `onVegaClick` handler maps to a concrete outcome.
11. Use `VegaNotify.close(id)` when dismissing a specific notification from an action button or later event.
12. Use `VegaNotify.closeAll()` only when the user explicitly wants to clear the full notification stack.
13. If the user asks for a page, return a complete minimal page or component file, not only isolated snippets.
14. If useful for debugging, include a note that `VegaNotify.open(...)` returns the created notification `id`.
15. If the current project does not yet have Vega installed, still use this skill, but do not install packages as part of the skill output.
16. If Vega is missing, tell the user which package or packages they need before the code will work.
17. If Vega is already installed, do not include package installation messaging.
18. When generating new code for an existing app, follow that app's established conventions for framework usage, file placement, import style, component structure, naming, styling, and state patterns instead of forcing a generic template.
19. Prefer adapting notification examples to the host app's existing interaction flow. Only fall back to the generic templates in this skill when the app structure is unknown or the user asks for a standalone example.
20. In SSR frameworks such as Next.js, trigger notifications on the client side only.

## Expected Output

Structure the answer in this order:
1. Short explanation of where notification-triggering logic should live.
2. Trigger or setup note explaining when the notification should be opened.
3. Framework-specific example.
4. Optional close or close-all snippet when the user needs lifecycle control.
5. Optional verification or debugging note when it would help.

Keep the output concrete. If the user asks for a notification flow, return complete code rather than describing the process abstractly.
If the user asks for a page, prefer a complete runnable example that includes a visible trigger and the notification behavior in the same output.
If required packages are missing, include a short note telling the user what to install. If the packages are already present, omit that note.
When the request targets an existing app, tailor the output to that app's current code style and structure rather than returning a generic sample.

## Framework Templates

### JavaScript Or Web Component Usage

```html
<vega-flex direction="col" gap="size-16">
  <vega-button id="show-notification">Show Notification</vega-button>
  <vega-button id="clear-notifications" variant="secondary">Close All</vega-button>
</vega-flex>

<script type="module">
  document.querySelector('#show-notification')?.addEventListener('click', async () => {
    await window.VegaNotify.open({
      title: 'Saved',
      message: 'Your changes were saved successfully.',
      type: 'success',
      duration: 4000
    });
  });

  document.querySelector('#clear-notifications')?.addEventListener('click', async () => {
    await window.VegaNotify.closeAll();
  });
</script>
```

### React Usage

```tsx
import { VegaButton } from '@heartlandone/vega-react';
import { VegaNotify } from '@heartlandone/vega';

export function NotificationExample(): JSX.Element {
  async function handleShowNotification(): Promise<void> {
    await VegaNotify.open({
      title: 'Profile updated',
      message: 'Your profile settings have been updated.',
      type: 'success',
      duration: 4000
    });
  }

  async function handleShowUndoNotification(): Promise<void> {
    const id = await VegaNotify.open({
      title: 'Item archived',
      message: 'The item was moved to the archive.',
      duration: 0,
      showCloseButton: true,
      actionButtons: [
        {
          label: 'Undo',
          onVegaClick: async () => {
            await restoreArchivedItem();
            await VegaNotify.close(id);
          }
        }
      ]
    });
  }

  return (
    <div>
      <VegaButton onClick={handleShowNotification}>Show Success Notification</VegaButton>
      <VegaButton onClick={handleShowUndoNotification}>Show Undo Notification</VegaButton>
    </div>
  );
}
```

### Angular, Vue, Or Stencil-Style Usage

```ts
import { VegaNotify } from '@heartlandone/vega';

export async function showPaymentErrorNotification(): Promise<void> {
  await VegaNotify.open({
    title: 'Payment failed',
    message: 'Try again or use a different payment method.',
    type: 'error',
    showCloseButton: true,
    duration: 0
  });
}
```

### Next.js Usage Note

```tsx
'use client';

import { VegaNotify } from '@heartlandone/vega';
```

### Close A Specific Notification

```ts
const id = await VegaNotify.open({
  title: 'Connection lost',
  message: 'Reconnect to continue syncing.',
  duration: 0,
  actionButtons: [
    {
      label: 'Dismiss',
      onVegaClick: () => VegaNotify.close(id)
    }
  ]
});
```

## Notification Notes

- `VegaNotify.open(...)` stacks new notifications below existing ones and returns the new notification `id`.
- Notifications dismiss automatically after 5 seconds by default.
- Use `duration: 0` for notifications that must remain visible until the user dismisses them.
- Action buttons are useful for flows such as `Undo`, `Retry`, or `Dismiss`.
- If the user asks for code applied in the repo, place the notification trigger in the existing interaction path instead of firing it at module load time.
- For global or playground-style pages, using `window.VegaNotify` is acceptable after Vega has loaded. For module-based apps, prefer direct imports.

## Example Request

`Show me how to display a success notification in React with VegaNotify.open.`

`Create a Vega page that opens a notification from a button click and lets me clear all notifications.`

`Help me build a manual-dismiss error notification with an action button.`

## Example Behavior

- Import `VegaNotify` from `@heartlandone/vega` for module-based apps.
- Open notifications from a user action handler or async result path.
- Store the returned `id` when a later close action is needed.
- Use `duration: 0` for manual-dismiss notifications.
- Use `VegaNotify.closeAll()` only when the app intentionally clears the full notification stack.