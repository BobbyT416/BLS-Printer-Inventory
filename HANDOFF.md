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


## Update — 2026-10-22 (v1.5.3)
- Replaced the separate Export backup and Import backup header buttons with one gear icon.
- Gear opens a compact choice prompt for Export backup or Import backup; both existing backup flows are retained. Report and Scan barcode remain visible.
- Validation: inline JavaScript syntax check passed; iPhone interaction has not been live-tested.


## Update — 2026-10-22 (v1.5.4)
- Removed the homepage subtitle “Receiving, refurbishment tracking, and resale readiness.”
- Validation: inline JavaScript syntax check passed.


## v1.5.5 — Print report dialog fix (2026-10-22)
- Fixed the Report setup dialog remaining open when **Print report** was clicked. The handler now prepares the inventory report, closes the setup dialog, then invokes the browser print UI after a short delay so the print stylesheet targets the inventory report rather than the setup screen.
- Validation: JavaScript syntax check passed. Not visually tested in a live browser or on iPhone; verify the printed preview on-device.


## v1.5.6 — Report column order (2026-10-22)
- Reordered printed inventory report columns to: Serial Number, Model, Asset Tag, Type, Page Count, Disposition, Cost, Price, Status.
- Reordered CSV export headers and row values to match the same requested order. Word document output inherits the report table order.
- Validation: JavaScript syntax check passed. Live print preview and downloaded-file output were not tested on-device.


## v1.5.7 — Home search barcode scanner (2026-10-22)
- Removed the standalone **Scan barcode** button from the home-page header; the scanner remains available from the Add Printer form.
- Added a compact barcode icon button at the right edge of the home search field. Tapping it opens the scanner in inventory-search mode; a scanned value fills the search field and filters the inventory rather than opening or creating a printer record.
- Bumped the visible app version to 1.5.7.
- Validation: JavaScript syntax check passed. No live browser or iPhone camera test performed.


## v1.5.8 — Header and home toolbar redesign (2026-10-22)
- Removed all interactive buttons from the header; header now uses a deep navy-to-blue gradient with a subtle diagonal accent and transparent logo styling.
- Moved Report and gear/settings controls next to Add Printer below the header, with the search field and barcode-search button alongside them. Responsive CSS wraps the controls on narrow screens.
- Logo asset replacement remains outstanding: the HTML still embeds the prior logo image; its white tile is removed by styling, but the exact newly supplied transparent logo has not yet been uploaded/embedded. Do not treat the exact-logo replacement as complete.
- Validation: JavaScript syntax check passed. No live browser/iPhone visual test performed.


## v1.5.9 — Header color (2026-10-22)
- Changed the header background to the cyan-blue sampled from the supplied HP logo image (`#42C0FB`).
- Validation: JavaScript syntax check passed. No live visual browser/iPhone test performed.


## v1.6.0 — Gradient header colors (2026-10-22)
- Changed the header from a flat HP blue to a left-to-right gradient: HP cyan-blue `#42C0FB` on the left transitioning to the vivid blue sampled from the supplied Brothers Lazer bolt image (`#024AD8`) on the right.
- Validation: JavaScript syntax check passed. No live visual browser/iPhone test performed.


## v1.6.1 — Logo vignette (2026-10-22)
- Added a subtle, broad radial light-blue/white vignette behind the logo to lift it from the blue gradient without creating a hard box or sticker effect. It is a CSS-only effect and does not modify inventory data or the logo image.
- Validation: JavaScript syntax check passed. No live browser/iPhone visual test performed.


## v1.6.2 — App title (2026-10-22)
- Updated the visible header title and browser document title to **BLS Used Printer Inventory**.
- Validation: JavaScript syntax check passed. No live browser/iPhone visual test performed.


## v1.6.3 — Backup settings dialog (2026-10-22)
- Replaced the gear icon's numeric prompt with a proper Backup & Settings dialog containing visible **Export Backup** and **Import Backup** buttons.
- Export continues to download a JSON backup of printer records and report settings. Import opens the JSON file picker and retains the confirmation that importing replaces records in this browser.
- Dialog explicitly explains that current storage is browser/device-local and does not automatically sync between users/devices.
- Important architecture note: multi-user sync requires a shared backend database/API and security rules. No shared backend is configured in this app currently; this change does not create cloud sync.
- Validation: JavaScript syntax check passed. No live browser/iPhone interaction test performed.


## v1.6.4 — Report download fixes (2026-10-22)
- Removed the **Save report setup** button from the Report setup dialog; the user no longer sees a separate setup-save action.
- **Save report** now downloads the selected format instead of sending PDF users to the print dialog. CSV downloads as .csv, Word-compatible report downloads as .doc, and PDF is generated and downloaded as .pdf using jsPDF.
- The report-format dropdown remains the choice between PDF, CSV, and DOC. The explicit **Print report** button still opens the browser print flow.
- Saving a report also persists the current report-column selections.
- PDF generation uses the jsPDF CDN library and therefore requires that library to load; if unavailable, the app displays an error instead of silently opening print.
- Validation: JavaScript syntax check passed. No live browser/iPhone download test performed.


## v1.6.5 — iPhone PDF save/share flow (2026-10-22)
- Updated PDF Save report behavior to generate a PDF Blob and, when supported by the browser, invoke the native share sheet with the PDF file so iPhone users can choose **Save to Files** or another destination. Browsers without file sharing fall back to a file download.
- Validation: JavaScript syntax check passed. No live iPhone test performed; verify that the deployed version opens the share sheet on iOS.


## v1.6.6 — Hide barcode from reports (2026-10-22)
- Removed Barcode from the configurable report-field list and default report selections. Barcode remains available for input/scanning and internal matching, but is no longer offered as a report column.
- Validation: JavaScript syntax check passed. No live browser/iPhone test performed.


## v1.6.7 — Search placeholder (2026-10-22)
- Changed the inventory search-field placeholder to **Search inventory items** and updated its accessible label to match.
- Validation: JavaScript syntax check passed. No live browser/iPhone test performed.


## v1.6.8 — Logo size (2026-10-22)
- Increased the logo's maximum width and height by approximately 10% for desktop and mobile breakpoints, preserving aspect-ratio containment and transparent styling.
- Validation: JavaScript syntax check passed. No live browser/iPhone visual test performed.


## v1.6.9 — Compact fixed header and contained scrolling (2026-10-22)
- Reduced header vertical padding and logo/title gap; tightened mobile header spacing and title size to reduce unused space above and below.
- Set the app viewport to a full-height flex layout with the header kept at the top and the main content area as the scroll container. Added touch scrolling and overscroll containment to prevent the page behind the active content from scrolling. Added print overrides to preserve report printing layout.
- Validation: JavaScript syntax check passed. No live iPhone visual/scroll test performed.


## v1.7.0 — Logo size increase (2026-10-22)
- Increased logo width and maximum-height limits by approximately 15% across desktop and mobile styles while retaining object-fit containment and the compact fixed-header layout.
- Validation: JavaScript syntax check passed. No live browser/iPhone visual test performed.


## v1.7.1 — iPhone scrolling correction (2026-10-22)
- Corrected the viewport flex layout so the main content is explicitly a zero-basis, zero-height flex child with its own vertical scrolling area. Added touch-action/overscroll containment and kept the version footer from consuming the content area's scroll height.
- Preserved fixed-height header and print overrides.
- Validation: JavaScript syntax check passed. This is a CSS/runtime issue; no live iPhone test performed. User reported scrolling failure in v1.7.0 and should retest this build.


## v1.7.2 — Fixed viewport scroll region (2026-10-22)
- Replaced the flex-child scrolling approach after the user confirmed v1.7.1 still would not scroll on iPhone. The header is now fixed to the viewport top, the main inventory region is explicitly fixed between the measured header bottom and footer top, and the footer stays fixed at the bottom.
- Added viewport measurement on initial render, resize, orientation change, and iOS visual viewport resize to keep the scroll region sized to the visible screen.
- Validation: JavaScript syntax check passed. No physical iPhone test performed; user should test this build directly.


## v1.7.3 — Header-only fixed scrolling (2026-10-22)
- Reworked layout again after user reported content was hidden under the header and footer did not need to remain fixed. Only the header is fixed. The main inventory and version footer now participate in normal document scrolling; main content starts below the measured header height.
- Header height is recalculated on load, resize, orientation/visual viewport changes, and with ResizeObserver to prevent the first content from starting beneath the header after logo/layout sizing settles.
- Validation: JavaScript syntax check passed. No live iPhone test performed.


## v1.7.4 — Disposition menu stays below header (2026-10-22)
- Fixed the disposition filter dropdown placement so its top edge is clamped below the fixed header, even when the filter summary is partially obscured or the menu is taller than the available space. The menu now uses the remaining visible viewport height instead of flipping above the trigger into the header area.
- Validation: JavaScript syntax check passed. No live iPhone test performed.


## v1.7.5 — Disposition filter defaults (2026-10-22)
- Renamed the filter summary from **All dispositions** to **Dispositions**.
- Unchecked **Parts only / scrap** and **Sold** by default; other disposition filters remain selected by default.
- Validation: JavaScript syntax check passed. No live browser/iPhone test performed.


## v1.7.6 — Disposition filter summary label (2026-10-22)
- Changed the filter summary so any nonzero partial selection displays **Disposition** instead of a count such as **5 selected**. All selected still displays **Dispositions**; zero selected displays **No dispositions selected**.
- Validation: JavaScript syntax check passed. No live browser/iPhone test performed.


## v1.7.7 — Apply Filter button label (2026-10-22)
- Changed the disposition menu's **Close** button label to **Apply Filter**. Its existing behavior remains closing the menu with the current checkbox selections applied.
- Validation: JavaScript syntax check passed. No live browser/iPhone test performed.


## v1.7.8 — Acquired-from field and detail cleanup (2026-10-22)
- Removed Barcode from the read-only Printer Details list. Barcode remains in the Add/Edit Printer form for scan/input and internal matching.
- Added **Acquired from** to the Add/Edit Printer form and read-only details. The value is stored on the device record and carried through the existing field save/load loop.
- Validation: JavaScript syntax check passed. No live browser/iPhone test performed.


## v1.7.9 — Details order and date-received removal (2026-10-22)
- Reordered the Printer Details field sequence so **Acquired from** appears immediately after **Disposition**.
- Removed **Date received** from the Add/Edit Printer form and the standard field/report configuration list. Any dateReceived values in older saved records are left in stored data for compatibility, but the field is no longer presented.
- Validation: JavaScript syntax check passed. No live browser/iPhone test performed.


## v1.8.0 — Compact printer details layout (2026-10-22)
- Tightened read-only details spacing on mobile: reduced modal body padding, reduced the gap between each field label and its value, and compacted the vertical gap between fields.
- Moved **Acquired from** to immediately follow **Disposition** in Printer Details.
- Removed **Date received** from the read-only Printer Details list as well as the Add/Edit form.
- Validation: JavaScript syntax check passed. No live iPhone/browser test performed.


## v1.8.1 — Return home after saving (2026-10-22)
- After a successful save from Add/Edit Printer, close both the edit dialog and any underlying Printer Details dialog, then reset inventory filters and scroll to the home inventory list.
- Validation: JavaScript syntax check passed. No live iPhone/browser test performed.


## v1.8.2 — iPhone dialog viewport and printer-save fix (2026-10-22)
- Dialogs now size to the current `visualViewport` height and update when that viewport resizes or scrolls, addressing iPhone Safari browser-chrome overlap that could hide the bottom action buttons.
- Fixed a save-data bug: the form's barcode was validated but not copied onto a newly created device record. The barcode is now explicitly saved, which supports inventory matching and duplicate detection.
- Confirmed the successful-save flow closes both Add/Edit and underlying Details dialogs, resets home inventory filters, renders the inventory, and scrolls to the top.
- Validation: JavaScript syntax checks passed. No live iPhone/browser test or real save test performed in this environment.
