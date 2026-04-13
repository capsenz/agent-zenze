---
name: add-reference
description: Add a new design style to the design-references repo. Accepts a URL to scrape (requires Playwright MCP) or a style name with notes to scaffold manually.
---

# add-reference

**REFERENCES_DIR:** The directory where your design references are stored.

## Configuration

Set the `DESIGN_REFERENCES_DIR` environment variable to the root of your design references repository.

The skill will use `$DESIGN_REFERENCES_DIR/references/` as the target directory.

## Detecting input mode

- If the argument starts with `http://` or `https://` → **URL mode**
- Otherwise → **Name mode**
- If no argument is given → ask the user: "Provide a URL to scrape or a style name to scaffold."

---

## URL mode (requires Playwright MCP)

1. Navigate to the URL using Playwright.
2. Derive a style name and confirm with the user:
   - Default: use the primary domain name (e.g., `linear.app` → `linear`)
   - If the domain is too generic (e.g., `example.com`), describe the visual character in 1-2 words (e.g., `vercel-dark`, `swiss-minimal`)
   - Style names must be: lowercase letters, hyphens, and underscores only — no spaces, capitals, or special characters
   - Show the user: "I'll save this as **<proposed-name>**. Reply with a different name or 'looks good' to continue."
   - Wait for user input before proceeding
3. Take a screenshot. Save it to `$DESIGN_REFERENCES_DIR/references/<derived-style-name>/screenshots/screenshot-01.png`.
4. Get the page's HTML source. Extract all `<style>` blocks and linked stylesheet contents. Save to `$DESIGN_REFERENCES_DIR/references/<derived-style-name>/snippets/scraped.css`.
5. Analyze the visual design from the screenshot and HTML/CSS. Identify:
   - The dominant aesthetic character
   - Color palette (exact hex values where visible)
   - Typography choices (font families, sizes, weights)
   - Layout patterns (grid, spacing, density)
   - Interaction patterns (hover states, transitions)
   - What makes this look distinctive vs. generic AI output
6. Draft `principles.md` using this template:

```markdown
# [Style Name]

## What it is
[One paragraph describing the aesthetic, its origin or inspiration, and key characteristics.]

## Core principles
- [rule]
- [rule]
- [rule]

## What to avoid
- [thing that breaks this aesthetic]
- [common AI default that conflicts with this style]
- [another pattern to avoid]

## Color & typography notes
[Specific hex values, font names, sizing observations from the scraped page.]

## References
- [URL that was scraped]
```

All five sections are required. Minimum content:
- `## What it is` — at least one complete sentence
- `## Core principles` — at least 3 bullet points
- `## What to avoid` — at least 3 bullet points, targeting AI defaults specifically
- `## Color & typography notes` — specific values where known; flag as "fill in" if unknown
- `## References` — at minimum, the URL scraped (URL mode) or leave as empty list (Name mode)
If a section would be empty, flag it clearly in the draft for the user to complete.

7. Show the user the drafted `principles.md` and ask: "Does this look right? Edit it before I save, or reply 'looks good' to proceed."
8. Wait for user confirmation. Apply any edits the user specifies.
9. Write the confirmed `principles.md` to `$DESIGN_REFERENCES_DIR/references/<style-name>/principles.md`.
10. Tell the user: "Reference saved. Run `/use-reference <style-name>` to load it."
11. Do NOT commit. Tell the user: "Commit when ready with: `git -C $DESIGN_REFERENCES_DIR add references/<style-name>/ && git commit -m 'feat: add <style-name> reference'`"

---

## Name mode

1. Parse the input:
   - The first word after `/add-reference` is the style name (e.g., `/add-reference swiss`)
   - Any additional text is treated as notes (e.g., `/add-reference swiss clean grid, helvetica, whitespace` → style name: "swiss", notes: "clean grid, helvetica, whitespace")
   - If no notes are provided, fill `principles.md` from general knowledge of the style and flag thin sections for the user to complete
2. Create the folder structure:

```bash
mkdir -p "$DESIGN_REFERENCES_DIR/references/<style-name>/snippets"
mkdir -p "$DESIGN_REFERENCES_DIR/references/<style-name>/screenshots"
```

3. Shape the user's notes into the `principles.md` template. If no notes were provided, fill what you can from general knowledge of the style and mark thin sections clearly for the user to fill in.

4. Draft `principles.md` using this template:

```markdown
# [Style Name]

## What it is
[From user notes or general knowledge — flag if uncertain.]

## Core principles
- [derived from notes]

## What to avoid
- [at minimum: list 3 things that would break this aesthetic, especially AI defaults]

## Color & typography notes
[From notes, or flag as "fill in".]

## References
[From notes, or leave empty.]
```

All five sections are required. Minimum content:
- `## What it is` — at least one complete sentence
- `## Core principles` — at least 3 bullet points
- `## What to avoid` — at least 3 bullet points, targeting AI defaults specifically
- `## Color & typography notes` — specific values where known; flag as "fill in" if unknown
- `## References` — at minimum, the URL scraped (URL mode) or leave as empty list (Name mode)
If a section would be empty, flag it clearly in the draft for the user to complete.

5. Show the user the drafted `principles.md` and ask: "Does this look right? Edit it before I save, or reply 'looks good' to proceed."
6. Wait for user confirmation. Apply any edits.
7. Write the confirmed `principles.md` to `$DESIGN_REFERENCES_DIR/references/<style-name>/principles.md`.
8. Tell the user: "Scaffolded `references/<style-name>/`. You can now:
   - Add CSS/HTML/Tailwind samples to `references/<style-name>/snippets/` (files named `custom.css`, `component.html`, etc.)
   - Add screenshots to `references/<style-name>/screenshots/` (named `screenshot-01.png`, `screenshot-02.png`, etc.)
   These are optional but will be included automatically when you run `/use-reference <style-name>`."
9. Do NOT commit. Tell the user: "Commit when ready with: `git -C $DESIGN_REFERENCES_DIR add references/<style-name>/ && git commit -m 'feat: add <style-name> reference'`"
