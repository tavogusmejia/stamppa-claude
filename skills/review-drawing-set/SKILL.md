---
name: review-drawing-set
description: Review a set of construction or architectural drawings open in Stamppa. Use when the user asks to check, review or audit a drawing set or sheets (for example "review this set", "check the sheets for missing notes", "mark the problems on the plans"). Reads sheet text, finds issues and leaves AI-signed, unsaved markups for the user to approve.
---

# Review a drawing set in Stamppa

Use the Stamppa MCP tools. Stamppa must be running with "AI assistant" turned on (Settings > AI assistant). Adding markups also needs "Let it add and change markups" ticked there. If a tool answers that Stamppa is not running or access is off, tell the user which setting to change and stop.

## Steps

1. **Find the set.** Call `list_sets`. If there is one set, use it; if several, ask which one. Note the set id, its name and its number of sheets.
2. **Agree the scope.** Ask (one short question) what to check if the user did not say: for example missing scale or north arrow, inconsistent sheet numbers, unresolved notes ("TBD", "VERIFY"), or a specific discipline. Do not invent criteria.
3. **Locate candidate sheets.** Use `search_text` with the key words of each criterion. It returns the document, page and the words around each match. Prefer targeted searches over reading every page.
4. **Read before judging.** For each candidate, call `read_page_text` with the document id and page number (pages start at 1). Quote the exact words that justify a finding. If the text is empty, the sheet may be a scanned image: say so and do not guess.
5. **Mark each finding.** For every confirmed issue call `add_markup` with `doc`, `page`, a `kind` (`cloud` for an area, `note` for a sticky note, `highlight` for text) and a `box` in PDF points on the page as displayed (origin top-left, y down). Put the finding in `comment` (or `text` for notes), starting with the sheet number and one sentence. If you do not know where on the sheet the issue sits, place a `note` near the top-left corner and say so in the comment. Never mark an area you have not read.
6. **Set status only when asked.** If the user asked you to triage existing markups, use `set_status` with the markup code and one of Accepted, Rejected, Cancelled, Completed or None. Otherwise leave statuses alone.
7. **Report.** List what you checked, what you found (sheet, quote, markup code) and what you could not check. State plainly that you changed nothing outside the open documents.

## Important

- Every markup you add is signed as made by the AI assistant, is one undo step, and is **left unsaved**. Tell the user to review the markups in Stamppa and save the file only if they agree. You cannot save, open or delete files or markups.
- Do not delete or alter markups the user made.
- Findings are suggestions for a professional to verify, not approvals or code-compliance judgments.
