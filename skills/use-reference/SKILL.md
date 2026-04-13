---
name: use-reference
description: Load a design reference into context when building a frontend UI. Use `/use-reference <style>` to load a specific style, or `/use-reference` with no argument to list all available styles.
---

# use-reference

**REFERENCES_DIR:** `$DESIGN_REFERENCES_DIR/references/`

## Configuration

Set the `DESIGN_REFERENCES_DIR` environment variable to the root of your design references repository.
Example: `export DESIGN_REFERENCES_DIR="/Users/carlsenze/repos/design-references"`

## When called with no argument

1. List every subdirectory in `$DESIGN_REFERENCES_DIR/references/` (each subdirectory is a style)
2. For each style, read its `principles.md` and extract the first complete sentence (text up to and including the first period)
3. If the `## What it is` section is missing or has no complete sentence, use: `<style> — (no description available)`
4. Output a numbered list in this format:

```
Available design references:

1. brutalism — Brutalism in web design is a deliberately raw, unpolished aesthetic that rejects the conventions of modern "clean" UI.
2. swiss — ...
```

5. Tell the user: "Run `/use-reference <style-name>` to load one into context."

## When called with a style name (e.g. `/use-reference brutalism`)

1. Verify `$DESIGN_REFERENCES_DIR/references/<style>/principles.md` exists. If not, output: "No reference found for '<style>'. Run `/use-reference` to see available styles." and stop.

2. Read `$DESIGN_REFERENCES_DIR/references/<style>/principles.md` in full.

3. If `$DESIGN_REFERENCES_DIR/references/<style>/snippets/` exists and contains files, read each file's contents.

4. Output to context in this format:

---
**Design reference loaded: <style>**

Build this UI according to the following design reference. Follow the principles exactly. Pay particular attention to the "What to avoid" section — these are the defaults you must actively resist.

[full contents of principles.md]

[if snippets exist:]
## Code snippets

### snippet1.css
```css
[full file contents]
```

### snippet2.html
```html
[full file contents]
```
---

5. Confirm to the user: "Loaded <style> reference. Snippets included: yes/no."
