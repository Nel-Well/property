---
name: burmese-i18n
description: "Add or revise Burmese (Myanmar) translations and typography in the Property Portal web app while keeping locale catalogs editable and Burmese font rendering readable."
---

# Burmese i18n

Use this skill for Burmese localization work in the Property Portal web app, including translation edits, new interface strings, language switching, and Burmese typography adjustments.

## Locale files

- Keep interface messages in `app/src/locales/en.json` and `app/src/locales/my.json`. English is the key source for `MessageKey` in `app/src/lib/i18n.tsx`.
- Add or update the same key in both files. Preserve interpolation tokens, embedded markup expectations, and meaning across the pair. Keep Burmese natural and readable; retain product names, proper nouns, and user-authored listing content when translation is not part of the request.
- Use the existing `useLanguage()` / `t(key)` pattern for interface copy instead of adding Burmese literals in components. Keep the persisted language selection and document `lang` in sync if changing language behavior.
- Do not silently translate listing titles, descriptions, amenities, API responses, or other stored/dynamic data. Add localized presentation only when requested and when the data has a reliable translation source.

## Burmese typography

- Use the existing Noto Sans Myanmar font stack for Burmese text.
- Burmese headings should use a smaller scale than the English display headings and should not use negative letter spacing.
- Do not set `line-height` for Burmese text. Let the Burmese font and browser use their natural metrics. When a shared CSS rule sets a line height, scope it to non-Burmese documents (for example, `html:not([lang="my"]) ...`) so it does not constrain Burmese text. Do not add Burmese-specific `line-height: normal` or another explicit value.
- Check the affected headings and wrapping at narrow widths after typography changes; Burmese glyphs and word breaks can behave differently from Latin text.

## Change checks

- Confirm `en.json` and `my.json` have matching keys after edits and that `t()` keys still type-check.
- For web app code changes, run the app production build (`cd app && npm run build`) and report any remaining untranslated dynamic content that affects the requested scope.
