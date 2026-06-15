# Suggested Setup for WebDNA in VS Code

This guide covers **two separate, optional setup processes** that improve the
WebDNA editing experience in Visual Studio Code. They are independent — you can
do either one, both, or neither.

| # | Process | What it does | Required? |
| - | ------- | ------------ | --------- |
| **1** | [File Associations](#process-1--file-associations) | Makes VS Code treat your files (`.html`, `.dna`, `.tpl`, `.inc`, etc.) as WebDNA so they get highlighted | Recommended — especially for `.html` |
| **2** | [Suggested Colors](#process-2--suggested-colors) | Applies a set of suggested syntax-highlighting colors for WebDNA | Optional — purely cosmetic |

> Prerequisite for both: the **WebDNA** extension must be installed and enabled.

---

# Process 1 — File Associations

This process makes sure your files actually open *as WebDNA* so they get
highlighted.

## What works automatically

When the WebDNA extension is installed, the following file extensions are
recognized as WebDNA with **no configuration required**:

`.html`  `.htm`  `.tpl`  `.inc`  `.dna`

Open a file with one of these extensions and it should highlight as WebDNA
straight away.

## The `.html` caveat (important)

VS Code ships with a **built-in HTML language** that also claims `.html`. When
two languages claim the same extension, VS Code may open your `.html` files as
plain HTML instead of WebDNA. If that happens, force the association as shown
below.

## How to force or add file associations

Use this if `.html` files open as HTML, or to add extensions that aren't in the
default list (for example `.tmpl`).

### Step 1 — Open your user settings (JSON)

1. Press `Cmd + Shift + P` (macOS) or `Ctrl + Shift + P` (Windows/Linux) to open
   the Command Palette.
2. Type **`Preferences: Open User Settings (JSON)`** and press `Enter`.

### Step 2 — Add file associations

Add this block to your `settings.json` (inside the outer `{ ... }`, separated
from other settings with a comma):

```jsonc
"files.associations": {
  "*.dna": "webdna",
  "*.html": "webdna",
  "*.tpl": "webdna",
  "*.inc": "webdna",
  "*.tmpl": "webdna"
}
```

Add or remove lines to match the extensions you use. Each entry maps a file
pattern to the `webdna` language.

### Step 3 — Save

Save `settings.json`. Matching files now open as WebDNA.

## Forcing a single file (quick, no settings change)

To change just one open file without editing settings:

1. Click the **language indicator** in the bottom-right of the VS Code status bar
   (it shows the current language, e.g. "HTML").
2. Choose **Configure File Association for '.html'…** and pick **WebDNA**.

## Optional: make WebDNA the default language

If most files you create are WebDNA, you can make new untitled files default to
it:

```jsonc
"files.defaultLanguage": "webdna"
```

---

# Process 2 — Suggested Colors

This process applies a set of recommended syntax-highlighting colors for WebDNA.
It is purely cosmetic and independent of Process 1.

## How it works

There are two layers involved:

1. **The grammar** (provided by this extension) labels each part of your WebDNA
   code with a *scope* name, for example `entity.name.tag.webdna` for a tag name
   or `variable.parameter.webdna` for a parameter.
2. **Colors** are assigned to those scopes. You can override any scope's color in
   your VS Code settings using `editor.tokenColorCustomizations`.

Because the scope names ship with the extension, these overrides work for anyone
who has the WebDNA extension installed — regardless of which color theme they use.

## How to apply the colors

### Step 1 — Open your user settings (JSON)

Same as Process 1: Command Palette → **`Preferences: Open User Settings (JSON)`**.

### Step 2 — Add the WebDNA color rules

Copy the block below into your `settings.json`.

> **If you do not already have `editor.tokenColorCustomizations`**, paste the
> whole block in (inside the outer `{ ... }`, separated with a comma).
>
> **If you already have `editor.tokenColorCustomizations`**, only copy the
> objects from inside the `textMateRules` array and add them to your existing
> `textMateRules` list — do not create a second `editor.tokenColorCustomizations`
> key.

```jsonc
"editor.tokenColorCustomizations": {
  "textMateRules": [
    {
      "name": "WebDNA Functions",
      "scope": "support.function.webdna",
      "settings": { "foreground": "#bdf794" }
    },
    {
      "name": "WebDNA Tag Names",
      "scope": "entity.name.tag.webdna",
      "settings": { "foreground": "#f6929a" }
    },
    {
      "name": "WebDNA Parameters",
      "scope": "variable.parameter.webdna",
      "settings": { "foreground": "#52aff5" }
    },
    {
      "name": "WebDNA Conditionals",
      "scope": "keyword.control.conditional.webdna",
      "settings": { "foreground": "#f38b6e" }
    },
    {
      "name": "WebDNA Flow Control",
      "scope": "keyword.control.flow.webdna",
      "settings": { "foreground": "#ff0000ac" }
    },
    {
      "name": "WebDNA Comments",
      "scope": "comment.block.webdna",
      "settings": { "foreground": "#d0cdcd5a" }
    },
    {
      "name": "WebDNA Context Content",
      "scope": "meta.embedded.context.formvariables.webdna",
      "settings": { "foreground": "#fd7049c6" }
    },
    {
      "name": "WebDNA Storage Variables",
      "scope": "storage.type.variable.webdna",
      "settings": { "foreground": "#d65fdee1" }
    }
  ]
}
```

### Step 3 — Save

Save `settings.json`. The colors apply **immediately** — open any `.dna`, `.tpl`,
`.inc`, or `.html` file to see the result. No reload or restart is needed.

## What each rule colors

| Rule | Scope | Color | Applies to |
| --- | --- | --- | --- |
| WebDNA Functions | `support.function.webdna` | `#bdf794` (green) | Built-in functions |
| WebDNA Tag Names | `entity.name.tag.webdna` | `#f6929a` (pink/red) | Context tag names, e.g. `[search]`, `[grep]` |
| WebDNA Parameters | `variable.parameter.webdna` | `#52aff5` (blue) | Parameter names, e.g. `search=`, `db=` |
| WebDNA Conditionals | `keyword.control.conditional.webdna` | `#f38b6e` (orange) | Conditional tags, e.g. `[showif]`, `[hideif]` |
| WebDNA Flow Control | `keyword.control.flow.webdna` | `#ff0000ac` (red) | Flow tags, e.g. `[loop]`, `[function]` |
| WebDNA Comments | `comment.block.webdna` | `#d0cdcd5a` (dim grey) | Comment blocks `[!] ... [/!]` |
| WebDNA Context Content | `meta.embedded.context.formvariables.webdna` | `#fd7049c6` (orange) | Content inside a context |
| WebDNA Storage Variables | `storage.type.variable.webdna` | `#d65fdee1` (purple) | Storage variables |

> Colors are written as hex values. An 8-digit hex (e.g. `#ff0000ac`) includes an
> alpha/transparency channel — the last two digits control opacity.

## Customizing the colors

To use your own colors, change the `foreground` value of any rule to a different
hex code. To find the right scope for an element you want to recolor:

1. Open a WebDNA file.
2. Open the Command Palette and run **`Developer: Inspect Editor Tokens and Scopes`**.
3. Click on the element in your code — VS Code shows the scope name(s) under it.
4. Add a rule for that scope with your chosen `foreground` color.

---

# Reverting

- **File associations (Process 1):** remove the entries you added from
  `files.associations` (and `files.defaultLanguage` if you set it) and save.
- **Colors (Process 2):** delete the rules you added from `textMateRules` (or
  remove the whole `editor.tokenColorCustomizations` block if you added it solely
  for WebDNA) and save. The extension's default theme-based highlighting returns
  immediately.
