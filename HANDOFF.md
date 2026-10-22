# BLS Printer Inventory — Project Handoff

Last updated: 2026-10-21
Repository: https://github.com/BobbyT416/BLS-Printer-Inventory
Live app: https://bobbyt416.github.io/BLS-Printer-Inventory/
Production branch: main
Current version: 1.4.4

## Purpose
Mobile-friendly browser app for tracking printers received for refurbishment, disposition, costs, sale status, and inventory details.

## Current home inventory design (v1.3.9)
- Simple selectable list; tapping a machine opens read-only Machine Details.
- Editing is through a separate Edit Printer button.
- No visible column labels on the home list.
- Two columns per machine, each machine row has two stacked lines:
  - Left column: Model number on top; Serial Number directly underneath.
  - Right column: Disposition on top; Page Count underneath (formatted as “Pages: …”).
- Alternating row shading: odd rows white, even rows light blue-gray.
- Keep the two-column layout compact and visually balanced on phones.
- Model is now intentionally shown on the home list; Asset Tag is omitted from the home list but remains available in Machine Details.
- Keep the full serial number visible; allow wrapping only if needed rather than clipping it.
- Do not reintroduce the old four-column header/table layout unless user asks.

## Record fields / behavior
Fields include Barcode, Serial Number, Asset Tag, Date Received, Type, Model, Page Count, Disposition, Refurbished By, Cost, Selling Price, Sale Status, Salesperson, and Refurbishment Notes.
- Printer Type options: Single function printer and MFP only.
- Serial number should auto-fill from barcode when blank and be uppercase on blur/save.
- Date Received should be blank for a new record.
- After saving, close the edit form and return to inventory.
- Do not claim persistence is confirmed without reload testing. Existing storage code has a possible issue: safeSet falls back to memory storage and suppresses localStorage errors, while persist can still return true. If save issues recur, verify storage persistence and communicate when data is session-only.

## Recent version history
- 1.3.4: home list became Serial Number, Asset Tag, Disposition, Page Count; no serial truncation and horizontal scrolling.
- 1.3.5: visible column labels.
- 1.3.6–1.3.8: repeated column spacing refinements; mobile layout remained too spread out in screenshot.
- 1.3.9: replaced four-column list with compact two-column, two-line rows; left Model + Serial Number, right Disposition + Page Count; removed labels; alternating shaded rows.

## Verification / release discipline
- v1.3.9 and this handoff committed to main.
- JavaScript syntax check passed; this does not verify appearance on a physical iPhone.
- Always fetch current main index.html before editing, make targeted changes, increment visible version, run JavaScript syntax check, commit to main, and report exact verification level.
- Never claim a real-device test unless one actually occurred.
- Do not alter unrelated repositories or projects.


## v1.4.0 — Print report behavior
- Preserved the approved home list design exactly: two columns, two lines per machine; left Model + Serial Number, right Disposition + Page Count; no labels; alternating shaded rows.
- Print List now uses a dedicated report table, not the compact home list.
- Printed report includes every record field: Barcode, Serial Number, Asset Tag, Date Received, Type, Model, Page Count, Disposition, Refurbished By, Cost, Selling Price, Sale Status, Salesperson, and Refurbishment Notes.
- Report respects the current archive mode, search text, type filter, and disposition filter. Print Setup visibility preferences do not omit fields from the report because the requirement is to include all fields.
- v1.4.0 committed to main; JavaScript syntax check passed. Print output has not been physically tested on a printer.


## v1.4.1 — Print report refinements
- Printed report excludes Barcode, Date Received, Refurbished By, and Salesperson.
- Remaining report columns: Serial Number, Asset Tag, Type, Model, Page Count, Disposition, Cost, Selling Price, Sale Status, Refurbishment Notes.
- Print stylesheet requests landscape orientation with 10 mm margins via @page.
- Home list remains unchanged.
- Committed to main; JavaScript syntax check passed. Browser/printer print dialog behavior not physically tested.


## v1.4.2 — Print preview refinement
- Report column labels shortened to Price, Status, and Notes (previously Selling Price, Sale Status, Refurbishment Notes).
- Print headings reduced to 7pt, body remains 8pt; removed aggressive word breaking and set practical minimum widths to reduce mid-word wrapping.
- Landscape page orientation remains requested by @page size: landscape with 10 mm margins.
- Home list unchanged.
- JavaScript syntax check passed. Print preview has not been physically retested; browser print settings may override landscape request.


## v1.4.3 — Report and type terminology
- Removed Notes entirely from the printed report; remaining columns are Serial Number, Asset Tag, Type, Model, Page Count, Disposition, Cost, Price, Status.
- Renamed the type option “Single function printer” to “Printer” in the form and type filter.
- On load, existing saved records with deviceType exactly “Single function printer” are migrated to “Printer” and written back to browser storage. MFP remains unchanged.
- Home list design unchanged; landscape print request retained.
- JavaScript syntax check passed; not tested on physical device/print dialog.


## v1.4.4 — Print layout and landscape investigation
- Print table changed to fixed layout with assigned column proportions, smaller 6pt uppercase headings and 7pt body, tighter cell padding, and controlled wrapping to reduce broken heading words and improve fit.
- @page now explicitly requests US Letter landscape with 0.25-inch margins.
- Important platform limitation investigated: CSS @page size is not reliably honored by Safari's native print dialog on iPhone; evidence includes MDN browser compatibility and Safari-specific compatibility reports. CSS can request landscape but cannot force the iOS print UI to select it in all cases. User may still need to choose Landscape manually. A truly reliable automatic landscape workflow would require generating a PDF with landscape page dimensions and opening/sharing that PDF instead of calling window.print().
- Home list unchanged. JS syntax check passed; print preview not tested in a live browser/device.


## Update — 2026-10-21 (v1.4.5)
- Removed the two summary cards at the top of the inventory page: “Devices in inventory” and “Ready for sale.”
- The inventory list, filters, and printer-record behavior are otherwise unchanged.
- Updated the visible app version to 1.4.5.
- Validation: extracted inline JavaScript passed `new Function` syntax compilation. Visual/browser testing was not performed in this change.


## Update — 2026-10-22 (v1.4.6)
- Replaced the single disposition dropdown with a checkbox multi-select containing all seven dispositions and an Archived machines checkbox.
- Added Select all and Deselect all controls. Archived mode shows sold devices with an archive timestamp; normal mode excludes sold devices.
- Removed the Archived machines button from the home-page header.
- Print list now follows the disposition checkbox selection and archive checkbox.
- Validation: inline JavaScript syntax check passed. Visual/browser testing was not performed.


## Update — 2026-10-22 (v1.4.7)
- Fixed the Home navigation/reset handler to use the new disposition checkboxes and archived checkbox instead of the removed filterDisposition element. The old reference could cause a JavaScript error when returning home.
- Preserved the existing browser storage key `printerInventoryPrototype.v1`; this change does not clear or replace stored inventory records.
- Validation: inline JavaScript syntax check passed. Actual records in the user's browser cannot be inspected remotely; visual/runtime testing remains necessary.


## Update — 2026-10-22 (v1.4.8)
- Added a visible Close button to the disposition multi-select menu and close-on-outside-tap behavior to improve usability on iPhone Safari.
- Validation: inline JavaScript syntax check passed. No live iPhone interaction test was available.


## Update — 2026-10-22 (v1.4.9)
- Reworked the disposition filter popup to use fixed positioning and calculate its position/maximum height against the current viewport. It opens below the filter when possible, otherwise above, and keeps its own options scrollable on small screens.
- Repositions on window resize and scrolling so it should not be clipped by the inventory panel or cover controls outside the viewport.
- Validation: inline JavaScript syntax check passed. Physical iPhone Safari testing was not available.


## Update — 2026-10-22 (v1.5.0)
- Renamed the header button from Print setup to Report and changed the dialog heading to Report setup.
- Removed the Print list button from the inventory panel.
- Added report format selector (PDF, CSV, Word DOC), Save report and Print report actions inside Report setup. CSV exports the report rows. DOC exports a Word-compatible HTML document. PDF uses the browser print dialog so the user can choose Save as PDF/Files; Print report opens the print dialog.
- Existing report field preferences and current inventory filters are retained. The report uses the nine approved columns.
- Validation: inline JavaScript syntax check passed. PDF save behavior and download handling have not been physically tested on iPhone.


## Update — 2026-10-22 (v1.5.1)
- Removed the Inventory heading from the home-page inventory panel.
- Left the Add printer button aligned to the left side of the panel header.
- Validation: inline JavaScript syntax check passed; no live iPhone visual test was performed.


## Update — 2026-10-22 (v1.5.2)
- Removed the Home button from the top navigation.
- Made the Brothers Lazer Service logo a keyboard-accessible link that resets search/type/disposition/archive filters and returns to the top of the inventory.
- Validation: inline JavaScript syntax check passed; no live iPhone visual test was performed.
