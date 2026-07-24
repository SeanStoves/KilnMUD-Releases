# KilnMUD translations

Interface translations live here. Drop a `<lang>.json` file in this folder and the next build bundles
it into the app; it then appears in **Settings → Language**. No code changes needed.

## Add a language

1. Copy `en.json` to your language code, e.g. `es.json` (Spanish), `fr.json` (French), `de.json`
   (German). Use the [ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) two-letter
   code.
2. Translate the **values** on the right. **Leave the keys** (the left side) exactly as they are.
3. Set `"_name"` to your language's own name, e.g. `"Español"`, `"Français"` — that is what the
   language picker shows.
4. Keep any `{{placeholder}}` tokens in place; they are filled in at runtime (e.g.
   `"home.welcome": "Bienvenido a {{app}}"`). Move them where the sentence needs them; do not rename
   them.
5. Open a pull request. Once it merges, the next release includes your language.

## Notes

- `en.json` is the reference key list. English itself ships inside the app, so an `en.json` here is
  ignored for loading; it is only the template to copy.
- A missing key falls back to English, so a partial translation is fine and safe to ship — untranslated
  strings just stay English.
- Keys are grouped by area (`toolbar.*`, `home.*`, `settings.*`, …). New keys appear in `en.json` as the
  app grows; re-copy the new ones as they show up.
