---
name: use-vega-translation
description: 'Generates Vega translation examples for Vega apps. Use this skill when the user wants to localize Vega component text, add translation resources, switch languages with VegaTranslation.changeLanguage, read VegaTranslation.currentLanguage, use VegaTranslationResourceEN, or create JavaScript, React, Angular, Vue, Stencil, or Next.js examples for Vega translation.'
---

# Use Vega Translation

## Purpose

Use this skill when the user wants to initialize Vega translation resources and make Vega components render translated text.

This skill is specifically for:
- Initializing `VegaTranslation` with one or more languages.
- Switching languages with `VegaTranslation.changeLanguage(...)`.
- Reading the active language through `VegaTranslation.currentLanguage`.
- Returning ready-to-paste usage for JavaScript, React, Angular, Vue, Stencil, or Next.js style projects.
- Explaining where translation initialization should live.
- Helping users preserve Vega placeholder conventions such as `{0}` and bracketed fixed phrases.

## When To Use This Skill

Use this skill when the user:
- Wants to use Vega's translation function.
- Wants to localize Vega component labels, validation messages, or internal UI text.
- Asks how to initialize `VegaTranslation`.
- Asks how to change the current Vega language.
- Wants a working translation example for a Vega app.
- Provides translation resources and wants them wired into a page or component.
- Wants translation support added to an existing Vega app, even if they have not shown the bootstrap location yet.

If the request is specifically about Vega translation, always use this skill first. Missing Vega dependencies are not a reason to skip the skill; instead, use the skill to provide the correct integration guidance and startup pattern.

## Required Vega API

Use these APIs exactly:

```ts
VegaTranslation.init(config: VegaTranslationConfig): void
VegaTranslation.changeLanguage(language: string): void
VegaTranslation.currentLanguage: string

type VegaTranslationConfig = {
  resources: VegaTranslationResources;
  language: string;
};
```

Also use the English base resource when needed:

```ts
VegaTranslationResourceEN
```

## Inputs To Collect

Ask only for the missing information:
- Target framework or runtime.
- Languages to support.
- Translation resource content or the specific keys to translate.
- Whether the user wants bootstrap-only guidance or a full page/component example.
- Whether the target app is SSR-based, such as Next.js.
- Target file or component if the user wants code changes applied.

If the user wants translations but has not provided the translated strings, ask for the resource object or give a placeholder example and say it needs to be replaced.

## Rules

1. Initialize `VegaTranslation` once at application startup, module initialization, or another shared bootstrap path.
2. Never place `VegaTranslation.init(...)` inside a render body or other frequently executed path.
3. In module-based apps, import `VegaTranslation` and `VegaTranslationResourceEN` from `@heartlandone/vega`.
4. In plain script or global usage, `window.VegaTranslation` and `window.VegaTranslationResourceEN` may be used after Vega has loaded.
5. Include `en: VegaTranslationResourceEN` unless the user explicitly wants a different English base.
6. Call `VegaTranslation.changeLanguage(language)` only with language keys that exist in `resources`.
7. Preserve placeholder tokens such as `{0}` exactly. Do not translate or reorder them unless the target language requires a different sentence order while keeping the same tokens intact.
8. Preserve bracketed fixed phrases such as `Viewing [{0}-{1}] of {2}` and `Show [{0} items / page]` as a single translation unit. Do not split the text inside `[]` into separate keys.
9. If the user asks for a page, return a complete minimal page or component file, not only isolated snippets.
10. If useful for debugging, include a quick `VegaTranslation.currentLanguage` verification step.
11. If the current project does not yet have Vega installed, still use this skill, but do not install packages as part of the skill output.
12. If Vega is missing, tell the user which package or packages they need before the code will work.
13. If Vega is already installed, do not include package installation messaging.
14. When generating new code for an existing app, follow that app's established conventions for framework usage, file placement, import style, component structure, naming, styling, and state patterns instead of forcing a generic template.
15. Prefer adapting the translation example to the host app's existing bootstrap architecture. Only fall back to the generic templates in this skill when the app structure is unknown or the user asks for a standalone example.
16. In SSR frameworks such as Next.js, initialize translation on the client side only.

## Expected Output

Structure the answer in this order:
1. Short explanation of where translation initialization should live.
2. Initialization snippet with `resources` and default `language`.
3. Framework-specific page or render snippet.
4. Optional verification snippet when debugging would benefit from it.

Keep the output concrete. If the user provides translations, return complete code rather than describing the process abstractly.
If the user asks for a page, prefer a complete runnable example that includes initialization and a visible way to switch languages in the same output.
If required packages are missing, include a short note telling the user what to install. If the packages are already present, omit that note.
When the request targets an existing app, tailor the output to that app's current code style and structure rather than returning a generic sample.

## Framework Templates

### JavaScript Or Web Component Usage

```html
<vega-flex direction="col" gap="size-16">
  <vega-input required label="Name"></vega-input>
  <vega-button id="switch-language">Switch To Chinese</vega-button>
</vega-flex>

<script type="module">
  window.VegaTranslation.init({
    resources: {
      en: window.VegaTranslationResourceEN,
      zh: {
        'This field is required': '此字段为必填项'
      }
    },
    language: 'en'
  });

  document.querySelector('#switch-language')?.addEventListener('click', () => {
    window.VegaTranslation.changeLanguage('zh');
  });
</script>
```

### React Usage

```tsx
import { useState } from 'react';
import { VegaButton, VegaInput } from '@heartlandone/vega-react';
import { VegaTranslation, VegaTranslationResourceEN } from '@heartlandone/vega';

VegaTranslation.init({
  resources: {
    en: VegaTranslationResourceEN,
    zh: {
      'This field is required': '此字段为必填项'
    }
  },
  language: 'en'
});

export function TranslationExample(): JSX.Element {
  const [language, setLanguage] = useState(VegaTranslation.currentLanguage);

  function switchLanguage(nextLanguage: 'en' | 'zh'): void {
    VegaTranslation.changeLanguage(nextLanguage);
    setLanguage(VegaTranslation.currentLanguage);
  }

  return (
    <div>
      <p>Current language: {language}</p>
      <VegaInput required label="Name"></VegaInput>
      <VegaButton onClick={() => switchLanguage('zh')}>Switch To Chinese</VegaButton>
      <VegaButton onClick={() => switchLanguage('en')}>Switch To English</VegaButton>
    </div>
  );
}
```

### Angular, Vue, Or Stencil-Style Usage

```ts
import { VegaTranslation, VegaTranslationResourceEN } from '@heartlandone/vega';

VegaTranslation.init({
  resources: {
    en: VegaTranslationResourceEN,
    zh: {
      'This field is required': '此字段为必填项'
    }
  },
  language: 'en'
});
```

```html
<vega-input required label="Name"></vega-input>
```

### Next.js Usage Note

```tsx
'use client';

import { VegaTranslation, VegaTranslationResourceEN } from '@heartlandone/vega';

VegaTranslation.init({
  resources: {
    en: VegaTranslationResourceEN,
    zh: {
      'This field is required': '此字段为必填项'
    }
  },
  language: 'en'
});
```

## Translation Notes

- A value in curly brackets such as `{0}` represents a variable and must be preserved.
- A value in square brackets such as `[0 items / page]` represents a fixed phrase and should remain a single translation segment.
- Translation resources often need both validation messages and component internal labels when the UI includes Vega form, calendar, pagination, uploader, or editor components.
- If the user wants only one or two strings translated, provide the minimal resource object instead of a large schema dump.
- If the user asks for code applied in the repo, place translation initialization in the app bootstrap or shared initialization layer instead of the component render path.
- For global or playground-style pages, using `window.VegaTranslation` is acceptable after Vega has loaded. For module-based apps, prefer direct imports.

## Example Request

`Show me how to initialize VegaTranslation for React and switch between English and Chinese.`

`Create a Vega page that uses translation resources for required field validation and pagination labels.`

## Example Behavior

- Import `VegaTranslation` and `VegaTranslationResourceEN` from `@heartlandone/vega` for module-based apps.
- Initialize translation once before rendering translated Vega components.
- Switch the language with `VegaTranslation.changeLanguage('zh')`.
- Read the active language with `VegaTranslation.currentLanguage` when a verification step is useful.
- Preserve placeholder tokens such as `{0}` and bracketed fixed phrases such as `[0 items / page]`.