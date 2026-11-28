---
name: ask-the-drawing
description: Answer questions about a drawing set open in Stamppa, such as counts of symbols or tags, room areas, which sheet has something, dimensions that do not match, broken sheet references, sheets that cite a detail, or contradictions between sheets. Uses Stamppa's ask_plan tool, which answers by fixed rules and cites page, sheet, place and rule. Use when the user asks about the content of their plans in Stamppa.
---

# Ask the drawing

Use the Stamppa MCP tools. Stamppa must be running with "AI assistant" turned on (Settings > AI assistant). This skill only reads: it changes nothing in Stamppa.

Stamppa reads each sheet as exact data and answers common questions about it **by rules, without AI**. Your part is to turn the user's words into one of those questions, then report the answer with its citation so the user can check it.

## Steps

1. **Pick the document.** Call `list_documents` to get the `doc` id (`d1`, `d2`...). If the user names a sheet ("A-101", "level 2"), keep it for the `sheet` argument.
2. **Choose the question** for `ask_plan`:
   - How many of something drawn: `count` with `page` and `symbol` (an id such as `S1` from `read_plan_language`) or `at` [x, y] on one copy of it.
   - How many of a written tag (door P-2, window W3): `count` with `text`.
   - Area of a room: `room_area` with `room` (name or number as written on the sheet), or none for every room.
   - Where something is: `find_sheet` with `text`.
   - Do the dimensions measure what they say: `check_dimensions`.
   - Broken references: `missing_refs`.
   - Which sheets cite a detail or a sheet: `cited_by` with `text` ("3/A-501" or "A-501").
   - What the sheets say against each other (tags missing from a schedule, schedule rows no plan shows, quantities that differ, references or specification sections that are not in the document): `cross_check`.
3. **Narrow it if you can.** `page` (from 1) limits the question to one page. `sheet` limits it to the pages with that sheet number, or whose title has those words ("LEVEL 2").
4. **Use the drawing's own words**, as written on the sheet: KITCHEN, not "cocina", when the sheet says KITCHEN. If you are not sure of the word, call `find_sheet` or `search_text` first.
5. **Call `ask_plan`** with the document id, the `question` and its arguments.

## Reading the answer

The reply has `answer`, `citations`, `unresolved`, `pages_read`, `pages_total` and `truncated`.

- Each citation has a `page`, a `sheet` and a place: `box` = [x0, y0, x1, y1] or `at` = [x, y], in points on the page as displayed (origin top-left). It also has the `rule` that produced it and a `source`.
- `source: rule` means the rule leaves no doubt. `source: rule_unsure` means a good guess, not a certainty.
- Common rules: `symbols.same_block` (the same CAD block inserted again), `texts.exact_tag` (the tag stands alone on its line), `texts.tag_in_line` (the tag sits inside a longer line, so a note may name it without it being drawn: unsure), `rooms.closed_outline` (a closed outline around the room name), `rooms.closed_with_doors_shut` (closes only with the doors covered: unsure), `dimensions.line_at_scale` and `dimensions.line_off_scale` (the dimension line does or does not measure what it says, within 2 %, at the sheet's scale), `refs.detail_over_sheet` (3/A-501 is detail 3 on sheet A-501).
- `unresolved` is not an error. It lists what the rules could not settle, with `look_with` saying where to look: `read_page_text`, the page image, or `ask_plan` page by page (for pages beyond the limit of one question).
- `truncated: true` means the answer does not cover the whole document (page limit, time limit or too many citations). Say which part it covers.

## Answering the user

- Lead with the figure or the fact, then the citation: sheet number, page and the rule in plain words ("4 chairs on A-101, page 1, counted as the same CAD block").
- For a `rule_unsure` citation, say so and why ("looks like", "probably": the room only closes with its doors shut, the tag also appears inside a note).
- For `unresolved`, say it in one sentence. If it helps, look with `look_with` and present what you find as your own reading, separate from Stamppa's rule-based answer.
- If no question fits, say so in one sentence and use `search_text`, `read_page_text` or `read_plan_language` instead.

## Limits

- Scanned sheets have no text or linework for the rules to read: everything comes back in `unresolved`.
- Symbols that are not CAD blocks, tags drawn as linework and detail marks split over two lines are not counted.
- The layer of each line is not read.
- For `cross_check`: a schedule pasted as an image or drawn as linework is not read (it shows in `unresolved`), and for specifications only section codes are read, not the free text.

## Important

- Every figure must come from the tool reply. If it is not there, say it is not there.
- The tool replies come back into this conversation, so the user's sheet data reaches the model. That is expected; do not copy it anywhere else.
- Answers are a working aid for a professional to verify, not an approval or a compliance opinion.
