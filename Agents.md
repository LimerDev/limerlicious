# Limerlicious — Agent Notes

VS Code color theme extension (`package.json` → `contributes.themes`). No build step, no code — just theme JSON files. README.md is the user-facing doc (variants, recommended fonts and settings); keep its variant list in sync when themes are added, renamed or removed.

Inspiration: the themes are inspired by Material and Gruvbox (specifically Gruvbox Material, the basis of the Modern palette). README.md has a Credits section saying so — keep it if themes are reworked.

## Themes

Two families, three variants each. **Classic and Modern are the sources of truth**; Soft and Light are derived from them.

| Label | File | Base | Derived from |
|---|---|---|---|
| Limerlicious Classic | `themes/limerlicious_classic.json` | `vs-dark` | — |
| Limerlicious Classic Soft | `themes/limerlicious_classic_soft.json` | `vs-dark` | Classic, brighter UI |
| Limerlicious Classic Light | `themes/limerlicious_classic_light.json` | `vs` | Classic, remapped to light |
| Limerlicious Modern | `themes/limerlicious_modern.json` | `vs-dark` | — |
| Limerlicious Modern Soft | `themes/limerlicious_modern_soft.json` | `vs-dark` | Modern, brighter UI |
| Limerlicious Modern Light | `themes/limerlicious_modern_light.json` | `vs` | Modern, remapped to light |

History: Classic was "Default", then "Visual Studio". Modern was "Material" (ported from the Zed *Gruvbox Material* theme by tokiory, at `%LOCALAPPDATA%\Zed\extensions\installed\gruvbox-material\themes\gruvbox-material.json`). The old Dark, Dark Neon and Dark Plus variants are gone.

**Any change to Classic or Modern must be carried over to its Soft and Light variants** (or the user told which variants it applies to). When a request names a family ("for modern…") but not a variant, check which ones are meant, or apply to the obvious set and say so.

There is currently **no generator script in the repo**; the derived files were produced by throwaway scripts. The derivation rules are below so they can be reproduced.

### Derivation rules

- **Soft:** UI `colors` only — every color with HSL lightness < 0.36 and saturation < 0.45 (backgrounds, borders, dark surfaces, accent-brown backgrounds) gets lightness +0.045. Fully transparent values (`…00`) and `#000000` untouched. Syntax colors unchanged.
- **Light:** each distinct dark color is mapped by *role* to a light counterpart — never a plain inversion (inverting makes borders/darker surfaces lighter than the background). Darker-than-editor surfaces (line highlight, tab bar, sticky scroll) get explicit overrides. Syntax colors are darkened versions of the same hues, aiming for ~4–5:1 contrast on the background; comments, Go imports and line numbers deliberately fainter (~2.5–3.7:1). Terminal ANSI colors follow light-background conventions. The orange accent becomes `#c27419` for contrast.
  - Classic Light: neutral grays with a faint warm cast (hue 35°, low saturation).
  - Modern Light: Gruvbox Material light palette, but UI neutrals pulled 80% toward a luminance-matched gray ("neat grey", e.g. editor `#f3f1e8`). Accents and syntax keep Gruvbox light hues.
  - Light themes have **no shadows**: `widget.shadow`, `scrollbar.shadow`, `editorStickyScroll.shadow`, `sideBarStickyScroll.shadow`, `panelStickyScroll.shadow` all `#00000000`.

## Design principles

- **Orange accent** `#cf8832` (light: `#c27419`) is the Limerlicious signature across all themes: cursor, active tab top border, activity bar, badges, progress bar. Selected list rows and buttons use the matching brown (`#4e3c28` dark). Don't boost or recolor it when adjusting other colors.
- **Color is earned:** variables, parameters and properties are the default foreground color, not colored. In Modern, properties were explicitly changed from Gruvbox red to match variables.
- **Italic keywords** in both families: `keyword`, `storage`, `storage.type`, `storage.modifier`, word operators (`new`, `typeof`, `instanceof`…), and semantic `controlKeyword`. Symbol operators (`keyword.operator`) are explicitly `fontStyle: ""`.
- **Classic** = neutral grays, no color cast; muted VS Code–style syntax palette.
- **Modern** = warm Gruvbox Material grays (editor `#292828`, chrome `#32302f`) with Gruvbox syntax colors, saturation boosted 20% over the original Gruvbox values. The user tested lowering Modern's UI warmth and reverted it — keep dark Modern warm.

### Classic specifics

| Role | Color |
|---|---|
| Editor background / chrome | `#2b2b2b` / `#303030` |
| Foreground / UI text | `#c9c9c9` / `#bdbdbd` |
| Variables / properties | `#c7c7c7` (`#cfcfcf` TextMate fallback) |
| Parameters | `#9cdcfe` |
| Keywords (italic) | `#368fb8` |
| Language builtins, storage (italic) | `#54a2c0` |
| Control keywords (italic, semantic) | `#be8ec0` |
| Strings | `#c9866c` |
| Numbers | `#b5cea8` |
| Functions / methods | `#ccd49b` |
| Types / classes | `#48cba4` |
| Structs | `#77b990` |
| Enums / interfaces / type params | `#bac79c` |
| Namespaces / packages | `#89bed6` |
| Comments | `#567e5b` |
| Error / warning | `#eb6e6e` / `#d7ba7d` |
| Go import paths | `#707070` |

### Modern specifics

| Role | Color |
|---|---|
| Foreground / variables / params / properties | `#d4be98` |
| Punctuation | `#c5b18d` |
| Keywords, tags, attributes (italic keywords) | `#78b3a6` |
| Functions, types, operators | `#85b97d` |
| Strings | `#aebe5d` |
| Numbers, builtins, `self` | `#b1667a` |
| Constants, enums | `#db7e97` |
| Errors | `#f85d54` |
| Namespaces | `#a89984` (gray — user disliked yellow/orange) |
| Comments (italic) | `#7c6f64` |
| Go import paths | `#707070` (shared with Classic) |

- Fuzzy-match highlights in the command palette, lists and suggest widget (`list.highlightForeground`, `list.focusHighlightForeground`, `editorSuggestWidget.*HighlightForeground`) are **teal `#78b3a6`, not orange**.
- Pop-ups (quick input, suggest, hover, menus, notifications, widgets) are lifted so they don't blend in: background `#383432`, border `#5a524c`, `widget.shadow` `#00000099`. Classic has **not** had this treatment yet.
- UI also has Gruvbox-colored bracket pairs, git decorations and diff gutters.

## Technical gotchas

- **Classic theme files are JSONC** (comments, trailing commas). Edit them textually or strip comments/trailing commas only for *reading*; don't round-trip them through `json.dumps` or the comments are lost. Modern files are plain JSON.
- **TextMate `fontStyle` inherits** from less specific rules when a more specific rule doesn't set it. So italic on `storage.type` also hits Go builtin types, Java import/package names, and `keyword.other.unit` (`px`) unless those rules set `fontStyle: ""`. Currently they inherit in both families.
- Theme files can't set fonts, ligatures, line height or UI font weight. The `"editor.lineHeight": 1.5` line in every theme file is ignored by VS Code (README recommends 1.7). Bold fuzzy-match letters are VS Code CSS — not themeable.
- Changing a theme's `id` in `package.json` resets users' theme selection.
- Light themes need `"uiTheme": "vs"` in `package.json`.

## Working with the user

- They iterate visually and often say "test, be ready to revert": back up the affected files (scratchpad) before experimental changes, and restore byte-for-byte on "revert".
- Report concrete before/after hex values for the key surfaces when changing colors.
- The user verifies in VS Code themselves; validate JSON (JSONC-aware for Classic) after every edit and check contrast when touching light themes.
