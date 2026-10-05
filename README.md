# Limerlicious
A theme for Visual Studio Code with focus on clean soft look.

## Variants
* **Limerlicious Classic** — neutral dark grays, muted syntax palette, orange accent.
* **Limerlicious Classic Soft** — Classic with a slightly brighter UI.
* **Limerlicious Classic Light** — light version of Classic: neutral light grays, darker syntax colors.
* **Limerlicious Modern** — Gruvbox Material colors (warm grays, beige text) with the Limerlicious orange accent.
* **Limerlicious Modern Soft** — Modern with a slightly brighter UI.
* **Limerlicious Modern Light** — light version of Modern, using the Gruvbox Material light palette (cream background, brown text).

## Credits
The themes are inspired by [Material](https://m3.material.io/) and [Gruvbox](https://github.com/morhetz/gruvbox) — in particular [Gruvbox Material](https://github.com/sainnhe/gruvbox-material) by sainnhe, whose palette the Modern variants are based on (by way of the [Zed port](https://github.com/tokiory/zed-gruvbox-material) by tokiory).

## Recommended Fonts
Both fonts have programming ligatures, so `!=` shows as a slashed equals, `=>` as an arrow, and so on.

* [CaskaydiaCove Nerd Font Mono](https://www.nerdfonts.com/font-downloads) — Cascadia Code patched with Nerd Font icons. Pick **CaskaydiaCove**, not *CaskaydiaMono* — that one is based on Cascadia Mono and has no ligatures.
* [JetBrains Mono](https://www.jetbrains.com/lp/mono/) — clean, tall letterforms made for code. Also available with Nerd Font icons as *JetBrainsMono Nerd Font Mono* on the Nerd Fonts download page.

Install the font, then restart VS Code.

## Recommended Settings
A color theme can't set fonts or line height, so add this to your user `settings.json` (`Ctrl+Shift+P` → *Preferences: Open User Settings (JSON)*). Recommended line height is **1.7**.

```jsonc
// CaskaydiaCove
"editor.fontFamily": "'CaskaydiaCove Nerd Font Mono', monospace",
// …or JetBrains Mono
// "editor.fontFamily": "'JetBrains Mono', monospace",

"editor.fontLigatures": true,
"editor.lineHeight": 1.7,

// Same font and ligatures in the integrated terminal
"terminal.integrated.fontFamily": "'CaskaydiaCove Nerd Font Mono'",
"terminal.integrated.fontLigatures.enabled": true
```

Tip: `editor.fontLigatures` also takes a font-feature string, e.g. `"'calt', 'ss01'"` to turn on Cascadia's cursive italics.

## Recommended Icon Set
[Studio Icons V2](https://github.com/vigan-abd/studio-icons-v2)

## Install extension
* Clone and copy it into the `<user home>/.vscode/extensions` folder and restart VS Code.

## Development
* Clone and open project.
* Press `F5` to open a new window with your extension loaded.
* Open the color theme picker with  the `File > Preferences > Theme > Color Theme` menu item, or use the `Preferences: Color Theme command (Ctrl+K Ctrl+T)` and pick your theme
* Open a file that has a language associated. The languages' configured grammar will tokenize the text and assign 'scopes' to the tokens. To examine these scopes, invoke the `Developer: Inspect Editor Tokens and Scopes` command from the Command Palette (`Ctrl+Shift+P` or `Cmd+Shift+P` on Mac).
