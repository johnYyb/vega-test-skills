---
name: display-vega-dialog
description: 'Generates Vega dialog examples for Vega apps. Use this skill when the user wants to display a dialog, show a confirmation dialog, open a modal, use VegaDialog.open, customize dialog buttons, handle dialog OK or Cancel actions, or choose between VegaDialog and vega-modal in JavaScript, React, Angular, Vue, Stencil, or Next.js.'
---

# Display Vega Dialog

## Purpose

Use this skill when the user wants to display a dialog in a Vega app.

This skill is specifically for:
- Showing quick alert or confirmation dialogs with `VegaDialog.open(...)`.
- Customizing dialog title, content, button labels, and destructive actions.
- Handling OK, Cancel, or close actions correctly.
- Choosing between the `VegaDialog` API and the `vega-modal` component.
- Returning ready-to-paste usage for JavaScript, React, Angular, Vue, Stencil, or Next.js style projects.
- Recommending where dialog-triggering code should live.

## When To Use This Skill

Use this skill when the user:
- Wants to display a dialog in Vega.
- Asks how to use `VegaDialog.open(...)`.
- Wants an alert, confirmation dialog, or destructive confirmation flow.
- Wants to customize dialog button labels or handlers.
- Wants to know when to use `VegaDialog` versus `vega-modal`.
- Wants a full page or component example that opens a dialog.
- Wants dialog behavior added to an existing Vega app, even if the trigger location is not fully specified yet.

If the request is specifically about showing dialogs in Vega, always use this skill first. Missing Vega dependencies are not a reason to skip the skill; instead, use the skill to provide the correct integration guidance and usage pattern.

## Required Vega API

Use these APIs exactly for quick dialogs:

```ts
VegaDialog.open(options: VegaDialogOption): HTMLVegaDialogElement

type VegaDialogOption = {
  title?: string;
  content?: string;
  type?: 'default' | 'danger';
  okButton?: VegaDialogActionButtonProps;
  handleOk?: VegaDialogAction;
  cancelButton?: VegaDialogActionButtonProps;
  handleCancel?: VegaDialogAction;
  showCancel?: boolean;
  size?: ResponsiveType<SizeType>;
  isVerticallyCenter?: boolean;
  isVerticallyCentered?: boolean;
  showCloseButton?: boolean;
  handleClose?: () => boolean;
};
```

Also use `vega-modal` when the user needs a fully custom dialog layout instead of the standard `VegaDialog` presentation.

## Inputs To Collect

Ask only for the missing information:
- Target framework or runtime.
- Whether the user wants a quick standard dialog or a fully custom modal.
- Dialog title and content.
- Whether the action is destructive.
- Button labels and whether a Cancel button should appear.
- What should happen on OK, Cancel, or close.
- Whether the user wants a full page example or only a trigger/snippet.
- Whether the target app is SSR-based, such as Next.js.
- Target file or component if the user wants code changes applied.

If the user wants a custom dialog layout but does not describe the content structure, ask for the intended layout or provide a minimal placeholder modal example and say it should be replaced with the real content.

## Rules

1. Use `VegaDialog.open(...)` for standard alerts, confirmations, and simple destructive confirmations.
2. Use `vega-modal` when the user needs fully custom layout, richer form content, or custom footer composition.
3. In module-based apps, import `VegaDialog` from `@heartlandone/vega`.
4. In plain script or global usage, `window.VegaDialog.open(...)` may be used after Vega has loaded.
5. Trigger dialogs from event handlers, controller functions, or other user action paths. Never call `VegaDialog.open(...)` directly during render.
6. If `handleOk` or `handleCancel` is defined, it must return `true` to close the dialog. Returning anything else should be treated as keeping the dialog open.
7. Use `type: 'danger'` when the primary action is destructive.
8. Prefer `isVerticallyCentered` for new examples. Mention `isVerticallyCenter` only when backward compatibility matters because it is deprecated.
9. For a single-action informational dialog, set `showCancel: false`.
10. If the user asks for a page, return a complete minimal page or component file, not only isolated snippets.
11. If useful for debugging, include a note that `VegaDialog.open(...)` returns the created `HTMLVegaDialogElement`.
12. If the current project does not yet have Vega installed, still use this skill, but do not install packages as part of the skill output.
13. If Vega is missing, tell the user which package or packages they need before the code will work.
14. If Vega is already installed, do not include package installation messaging.
15. When generating new code for an existing app, follow that app's established conventions for framework usage, file placement, import style, component structure, naming, styling, and state patterns instead of forcing a generic template.
16. Prefer adapting dialog examples to the host app's existing interaction flow. Only fall back to the generic templates in this skill when the app structure is unknown or the user asks for a standalone example.
17. In SSR frameworks such as Next.js, open dialogs on the client side only.

## Expected Output

Structure the answer in this order:
1. Short explanation of whether `VegaDialog` or `vega-modal` is the better fit.
2. Trigger or setup note explaining where the dialog-opening logic should live.
3. Framework-specific example.
4. Optional custom `vega-modal` example when the user needs a fully custom dialog.
5. Optional verification or debugging note when it would help.

Keep the output concrete. If the user asks for a dialog flow, return complete code rather than describing the process abstractly.
If the user asks for a page, prefer a complete runnable example that includes a visible trigger and the dialog behavior in the same output.
If required packages are missing, include a short note telling the user what to install. If the packages are already present, omit that note.
When the request targets an existing app, tailor the output to that app's current code style and structure rather than returning a generic sample.

## Framework Templates

### JavaScript Or Web Component Usage

```html
<vega-button id="show-dialog">Show Dialog</vega-button>

<script type="module">
  document.querySelector('#show-dialog')?.addEventListener('click', () => {
    window.VegaDialog.open({
      title: 'Dialog Title',
      content: 'This is the content of the dialog.',
      showCancel: false,
      okButton: {
        label: 'Close'
      }
    });
  });
</script>
```

### React Usage

```tsx
import { VegaButton } from '@heartlandone/vega-react';
import { VegaDialog } from '@heartlandone/vega';

export function DialogExample(): JSX.Element {
  function handleOpenDialog(): void {
    VegaDialog.open({
      title: 'Please Confirm',
      content: 'Are you sure you want to delete this item?',
      type: 'danger',
      okButton: {
        label: 'Delete'
      },
      cancelButton: {
        label: 'Cancel'
      },
      handleOk: async () => {
        await apiCallToDeleteItem();
        return true;
      }
    });
  }

  return <VegaButton onClick={handleOpenDialog}>Open Dialog</VegaButton>;
}
```

### Angular, Vue, Or Stencil-Style Usage

```ts
import { VegaDialog } from '@heartlandone/vega';

export function openDeactivateDialog(): void {
  VegaDialog.open({
    title: 'Please Confirm',
    content: 'Are you sure you want to deactivate this account?',
    type: 'danger',
    okButton: {
      label: 'Deactivate'
    },
    cancelButton: {
      label: 'Keep Account'
    },
    handleOk: async () => {
      await apiCallToDeactivateAccount();
      return true;
    }
  });
}
```

### Next.js Usage Note

```tsx
'use client';

import { VegaDialog } from '@heartlandone/vega';
```

### Custom `vega-modal` Example

```tsx
import { useState } from 'react';
import { VegaButton, VegaModal } from '@heartlandone/vega-react';

export function CustomDialogExample(): JSX.Element {
  const [open, setOpen] = useState(false);

  return (
    <>
      <VegaButton onClick={() => setOpen(true)}>Edit Profile</VegaButton>
      <VegaModal
        open={open}
        modalTitle="Edit Profile"
        size={560}
        isVerticallyCentered={true}
        onVegaClose={() => setOpen(false)}
      >
        <div slot="modal-content">Custom dialog content goes here.</div>
        <div slot="modal-footer">
          <VegaButton variant="secondary" onClick={() => setOpen(false)}>
            Cancel
          </VegaButton>
          <VegaButton onClick={() => setOpen(false)}>Save</VegaButton>
        </div>
      </VegaModal>
    </>
  );
}
```

## Dialog Notes

- `VegaDialog` wraps a `vega-modal` internally and is the faster option when the content is only message text plus actions.
- `showCancel` defaults to `true`, so hide it explicitly for one-button informational dialogs.
- `handleClose` is part of the modal configuration and can be used when the close button itself needs custom logic.
- The standard dialog uses a static backdrop, so closing behavior should be handled through OK, Cancel, or close-button logic.
- For destructive actions, prefer a danger-styled primary action and explicit button labels such as `Delete` or `Deactivate`.
- If the user asks for code applied in the repo, place the trigger in the existing user interaction path instead of opening the dialog at module load time.

## Example Request

`Show me how to display a confirmation dialog in React with VegaDialog.open.`

`Create a Vega page that opens an informational dialog from a button click.`

`Help me decide whether to use VegaDialog or vega-modal for a dialog with a form inside it.`

## Example Behavior

- Import `VegaDialog` from `@heartlandone/vega` for module-based apps.
- Open a standard confirmation dialog from a user action handler.
- Return `true` from `handleOk` or `handleCancel` when the dialog should close.
- Use `type: 'danger'` for destructive confirmations.
- Prefer `vega-modal` instead of `VegaDialog` when the dialog body needs custom layout or embedded form controls.