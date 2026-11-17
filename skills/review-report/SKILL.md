---
name: review-report
description: Build a review report for a drawing set from Stamppa: the open issues from the markups, the content of the sheets and the sheet package. Use when the user asks for a review report, a comment log, a summary of markups, or a status report of a drawing set open in Stamppa.
---

# Review report from Stamppa

Use the Stamppa MCP tools. Stamppa must be running with "AI assistant" turned on (Settings > AI assistant). This skill only reads: it changes nothing in Stamppa.

## Steps

1. **Pick the set and the documents.** Call `list_sets` for the sheet sets open in Stamppa and ask which one if there is more than one. The markup and document tools take a document id (`d1`...).
2. **Get the sheet package index.** Call `set_package` with the set id and without `to_folder`: it returns the index and the list of sheets in the reply and writes nothing. Use it for sheet numbers, titles and the report's structure. Only if the user asks to save the package to a folder, call it again with `to_folder` true and say where Stamppa wrote it (the folder the user chose in Stamppa's settings).
3. **Read the markups.** Call `markups_markdown` with the document id. It returns one section per sheet with code, type, status, comment and replies. This is the source of every issue in the report.
4. **Read the sheets when needed.** Call `document_markdown` with `pages` (for example "1-3, 7") for the sheets whose markups need context. Use `drop_cad_clutter` true on dense CAD sheets. Do not load the whole set when a few pages are enough.
5. **Write the report.** Structure: Summary (counts by status and by sheet), Open issues grouped by sheet (code, comment, status, who replied), Resolved items, and Sheets without markups. Quote markup comments verbatim; keep your own words for the summary. Mark anything you inferred as "Inference".
6. **Close with gaps.** List what could not be read (empty text, cut replies, pages beyond the limit) so the reader knows the limits of the report.

## Important

- Every statement about a sheet must come from the tool replies. If something is not there, say it is not there.
- If a reply is cut (`max_chars`), say so and ask before reading more.
- This is a working document for a professional to verify, not an approval or a compliance opinion.
