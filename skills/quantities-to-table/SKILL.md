---
name: quantities-to-table
description: Turn the measurements taken in Stamppa (lengths, areas, counts) into a clean quantity table in Markdown or CSV. Use when the user asks for quantities, takeoffs, a bill of quantities or "the measurements as a table" from a drawing open in Stamppa.
---

# Quantities from Stamppa to a table

Use the Stamppa MCP tools. Stamppa must be running with "AI assistant" turned on (Settings > AI assistant). This skill only reads: it changes nothing in Stamppa.

## Steps

1. **Pick the document.** The tools take a document id (`d1`, `d2`...). If the user did not say which drawing, ask once, or take the one that is in front when only one is open.
2. **Read the measurements.** Call `list_quantities` with the document id. It returns every measurement with its value and unit, plus totals by kind and unit. The values come from the scale calibrated in Stamppa; do not recompute them from the page.
3. **Get the context.** Call `export_markups` with the same document and `format` `csv` (or `json` if you need to group by field). Use it to attach to each quantity its markup code, page, comment, layer and status, matching rows by code.
4. **Build the table.** Default to a Markdown table with these columns when the data has them: Code, Sheet/page, Description (the markup comment), Kind (length, area, count), Value, Unit, Status. Add a final totals block per kind and unit taken from `list_quantities`, not summed by you. If the user wants CSV, give the CSV in a code block with a header row.
5. **Check before you hand it over.** Compare the number of rows with what `list_quantities` reported. If anything is missing, empty or has no unit, list it under "Needs attention" instead of filling it in. State which scale/units Stamppa reported; if measurements have no calibrated scale, say the values are not reliable.
6. **Offer next steps** only as suggestions (group by sheet, by layer, or by description), without redoing the extraction unless asked.

## Important

- The data stays what Stamppa says: never round, convert units or merge rows silently. If you convert units, say so and show both.
- The tool replies come back into this conversation, so the user's measurements reach the model. That is expected; do not copy them anywhere else.
