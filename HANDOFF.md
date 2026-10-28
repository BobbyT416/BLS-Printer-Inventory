# BLS Printer Inventory — Project Handoff

Last updated: 2026-10-28 (Version 1.8.73)
Repository: https://github.com/BobbyT416/BLS-Printer-Inventory
Live app: https://bobbyt416.github.io/BLS-Printer-Inventory/
Production branch: main
Current version: 1.8.73

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


## v1.8.3 — Home-screen icon and web app metadata (2026-10-22)
- Added `icon.svg`, a blue-gradient printer/checkmark icon branded BLS Inventory.
- Added `manifest.webmanifest` with the app name, short name, standalone display mode, theme/background colors, and icon declaration for supported browsers/Android installation.
- Added favicon, Apple touch icon, and manifest links to `index.html` with cache-busting query strings.
- Validation: JavaScript syntax checks passed. Not tested on physical iPhone/Android. Note: iOS versions can require a PNG `apple-touch-icon`; this commit currently points that tag to SVG, so an actual iPhone home-screen icon still needs device verification and may require a PNG asset for reliable iOS support.


## v1.8.4 — Honest local-storage save failure handling (2026-10-22)
- Self-audit found `safeSet()` swallowed `localStorage.setItem()` failures, then `persist()` returned success anyway because the write helper never reported failure. In Safari private/restricted storage or quota failure, the app could close the form and appear to save while the record only existed in memory and disappeared on reload.
- `safeSet()` now returns true only after a successful `localStorage` write; `persist()` aborts the save flow and shows an explicit **Printer was NOT saved** message if the write fails. This prevents false-success navigation.
- Barcode remains a required field. The form's submit button and successful-save navigation were inspected; successful persistence should close the form/details and reset the home inventory list.
- Validation: JavaScript syntax checks passed. No live iPhone save/reload test performed.


## v1.8.5 — Save confirmation and button label (2026-10-22)
- Renamed the Add/Edit Printer submit button from **Save printer** to **Save**.
- After a successful storage write, the app closes the editor/details, resets filters and returns to the home inventory, and displays **Printer saved** for approximately two seconds before dismissing the confirmation. Failed saves still show the explicit failure message and do not run the success flow.
- Validation: JavaScript syntax checks passed. No live iPhone test performed.


## v1.8.6 — Save-form self-audit follow-up (2026-10-22)
- Audited the Add/Edit form's native required fields. Barcode was marked required, so the browser could block submission before the save handler ran when the user did not have a sticker barcode ready.
- Made barcode optional. When blank, the save handler creates a unique temporary `AUTO-...` barcode; serial number defaults to that value only if serial number was also left blank. Users can enter or scan a real barcode later.
- Added explicit, visible checks for required printer type and model before saving, with focus moved to the missing field.
- Local storage write failures remain explicit and abort success navigation; successful saves still show the two-second confirmation and return home.
- Validation: JavaScript syntax checks passed. No live iPhone save/reload test performed.


## v1.8.7 — Roomier mobile Add/Edit Printer form (2026-10-22)
- Reworked Add/Edit dialog spacing to show more of the form at once on phones: reduced header/note/panel padding, moved most fields into a two-column grid, and tightened labels and hints.
- Kept the longer refurbishment notes field full width and preserved a scrollable form body plus fixed footer actions. Dialog sizing uses the current visible viewport height.
- Inputs retain a 16px font size on mobile to avoid iOS Safari auto-zoom while typing.
- Validation: JavaScript syntax checks passed. No live iPhone layout test performed.


## Version 1.8.8 — 2026-10-22
- Changed Add/Edit Printer form to a single-column layout on desktop and mobile so fields are easier to read and the user can scroll vertically.
- Removed automatic focus on Barcode when opening Add/Edit; the iPhone keyboard now stays hidden until the user taps a field.
- Added scroll/touch-move handling inside the Add/Edit form to blur a focused text field and dismiss the software keyboard when scrolling begins. Tapping another field brings it back.
- Validation: both inline JavaScript blocks passed syntax compilation. This is not a live iPhone Safari test; verify keyboard dismissal and field selection on-device.


## Version 1.8.9 — Less-sensitive keyboard dismissal (2026-10-22)
- Fixed the keyboard dismissing while the user was typing in Add/Edit Printer. Removed the generic scroll listener, which could react to Safari's automatic input/caret scrolling.
- The keyboard now dismisses only after a deliberate touch movement of at least 14 pixels inside the form. Normal typing and small finger movements should leave it open; tapping a field brings it back after scrolling.
- Version bumped to 1.8.9.
- Validation: source change committed to main. Live iPhone Safari behavior has not been physically tested.


## Version 1.8.9 — 2026-10-22
- Reordered the Add/Edit Printer form so Model number appears immediately below Serial number in the single-column layout.
- Validation: both inline JavaScript blocks passed syntax compilation. No live iPhone test performed.


## Version 1.8.10 — 2026-10-22
- Corrected Add/Edit Printer field order: Serial number, Model number, then Asset tag.
- Validation: both inline JavaScript blocks passed syntax compilation. No live iPhone test performed.


## Version 1.8.11 — 2026-10-22
- Adjusted Add/Edit form touch handling so a scroll gesture starting on an input, select, button, label, or Notes textarea does not immediately dismiss the keyboard. Gestures starting in non-control form space can still dismiss it.
- Added extra scroll clearance after the Notes textarea to help keep the final field usable near the bottom of the form.
- Validation: both inline JavaScript blocks passed syntax compilation. No live iPhone test performed.


## Version 1.8.12 — 2026-10-22
- Barcode field remains blank unless the user manually enters a value or scans a barcode.
- Removed automatic barcode-to-serial-number copying and removed generated AUTO-* barcode IDs on save. Serial number is only uppercased; it is not filled from barcode.
- Blank barcodes are allowed on multiple devices; duplicate checks run only when a barcode was provided.
- Updated the barcode hint text to clarify this behavior.
- Validation: both inline JavaScript blocks passed syntax compilation. No live iPhone test performed.


## Version 1.8.13 — 2026-10-22
- Configured the ZXing fallback scanner with explicit common barcode formats including Code 128, plus TRY_HARDER when the library exposes the corresponding hints. The native BarcodeDetector branch remains disabled because the implementation uses the ZXing fallback.
- Scanning now fills only the Barcode field; it no longer copies scanned values into Serial number.
- Validation: both inline JavaScript blocks passed syntax compilation. Scanner behavior still needs an on-device test with the supplied label; no physical iPhone test performed.


## Version 1.8.14 — 2026-10-22
- Fixed the native BarcodeDetector branch being hard-disabled; it is now used only when the browser reports Code 128 support, otherwise the ZXing fallback is used.
- Raised requested camera resolution to 2560×1440 and reduced ZXing scan interval to 150 ms to improve chances of reading dense linear barcodes.
- Validation: both inline JavaScript blocks passed syntax compilation. Still requires on-device testing against the user's physical label; no live iPhone test performed.


## Version 1.8.16 — 2026-10-22
- Added Product number field directly below Model number and included it in saved printer fields and report preferences.
- The Add/Edit screen's Scan button now starts a multi-label scan mode. It tries to capture both barcodes from the same HP label: a 10-character alphanumeric value is assigned to Serial number and a 6-character alphanumeric value to Product number. Camera detection handles multiple results in one frame where supported; the ZXing callback keeps scanning until both fields are populated.
- The scanner does not put these label values into the inventory Barcode field.
- Validation: both inline JavaScript blocks passed syntax compilation. Real-world multi-barcode detection on iPhone remains unverified; formats are classified by expected length/pattern, so confirm both values before saving.


## Version 1.8.17 — 2026-10-22
- Added product-number-to-model auto-fill in Add/Edit Printer. Verified mapping: HP product number F2A80A → HP LaserJet Enterprise Flow MFP M527cm.
- When a mapped product number is entered or captured by the dual-barcode scanner, Model number is populated automatically. If a previously auto-filled product number is changed to an unknown value, the app clears only the model value it had auto-filled; manually entered model values are preserved.
- Validation: both inline JavaScript blocks passed syntax compilation. Mapping is intentionally conservative; additional product numbers should be added only after verification.


## Version 1.8.18 — 2026-10-22
- Fixed the dual-label scanner path: assigning the captured product number directly to the field did not trigger its `input`/`change` listeners, so the model lookup never ran. The scanner now explicitly calls `updateModelFromProductNumber()` immediately after capturing the product number.
- Verified mapping remains HP product number F2A80A → HP LaserJet Enterprise Flow MFP M527cm. Unknown product numbers still require manual model entry.
- Validation: both inline JavaScript blocks passed syntax compilation. Code-path fix committed; no physical iPhone test performed.


## Version 1.8.19 — faster label barcode recognition (2026-10-22)
- Optimized the primary HP label scanning flow for Code 128, which is the barcode format confirmed on the user's printer label.
- Requests 1920×1080 video and continuous-focus preference at camera startup.
- In label mode, both native BarcodeDetector and ZXing prioritize Code 128 rather than trying multiple barcode formats. ZXing polling interval reduced from 150 ms to 80 ms for label scanning (100 ms for general scanning), and the slower TRY_HARDER hint was removed.
- JavaScript syntax checks passed for both inline script blocks. Not physically tested on the user's iPhone; validate scan speed on the same clear label. Camera hardware/browser support may limit the effect of requested resolution and focus.


## Version 1.8.20 — scanning reliability adjustment (2026-10-22)
- Lowered requested camera resolution to 1280×720 for both native BarcodeDetector and ZXing paths to reduce camera/decode load while retaining sufficient resolution for label barcodes.
- Throttled native BarcodeDetector polling to a maximum of 10 attempts per second rather than scheduling a detect call every animation frame.
- Updated scanner guidance to center one barcode and hold it steady while autofocus settles.
- JavaScript syntax checks passed for both inline script blocks. Not physically tested on the user's device. If recognition remains inconsistent, next diagnostic step is to identify which reader path the browser is using and add fallback from native BarcodeDetector to ZXing after a short timeout.


## Version 1.8.21 — Safari scanner resolution adjustment (2026-10-22)
- User confirmed the inventory app opens in Safari and camera preview appears immediately, so startup delay is not the likely cause.
- Restored requested 1920×1080 resolution for native and ZXing camera constraints after 720p may have reduced barcode detail; kept Code 128-specific label decoding and 100 ms ZXing interval.
- JavaScript syntax checks passed for both inline script blocks. Not physically tested on the user's iPhone.


## Version 1.8.22 — product lookup improvements and shorter display names (2026-10-22)
- Expanded verified product lookup entry F2A80A to identify model `HP LaserJet Enterprise Flow MFP M527cm` and type `MFP` when product number is captured or edited. If that product number is changed to an unknown value, auto-filled model/type values are cleared; manually entered model values are preserved.
- Added `shortModelName()` to simplify model labels on the inventory home list and generated reports (print/PDF/Word/CSV) while retaining full model name in Add/Edit and printer details. Example: `HP LaserJet Enterprise Flow MFP M527cm` displays as `M527cm` in list/reports.
- Scanner glare itself cannot be removed by software; guidance is to keep the label flat, avoid direct reflections, and try a slight angle or diffuse lighting. Avoid aggressive image processing until scanner behavior is diagnosed.
- JavaScript syntax checks passed for both inline script blocks. Not physically tested on the user's device.

## Version 1.8.23 — scanner fallback improvement (2026-10-22)
- Kept the native browser BarcodeDetector as the first reader when it supports Code 128, but added a 3-second timeout fallback to ZXing if the native reader does not complete the scan. In dual-label mode, fallback continues if either the serial number or product number is still missing.
- Added a per-scan override so the fallback attempt skips the native reader and reaches ZXing instead of repeating the same failed path. The override resets when opening a new scan session.
- Updated visible app version to 1.8.23.
- This is a software fallback improvement, not a guarantee of camera focus or barcode readability. No physical iPhone or Android test has been performed yet.
- Model/type identification remains limited to verified entries in PRODUCT_MODEL_MAP. At this version, F2A80A is the only verified product-number mapping; additional models require a verified lookup catalog or data source.


---

# Current authoritative handoff — 2026-10-26

> **Use this section as the source of truth for continuing work in a new chat.** The older version-by-version notes above preserve history, but they are not all current. In particular, older notes about a home-page barcode scanner were superseded by Version 1.8.72.

## Current release and repository state

- **App:** BLS Used Printer Inventory
- **Repository:** https://github.com/BobbyT416/BLS-Printer-Inventory
- **Branch:** `main`
- **Live app:** https://bobbyt416.github.io/BLS-Printer-Inventory/
- **Current committed version:** **1.8.72**
- **Latest known commit:** `9eadfeee8bb3ace39ee405d1a7098f5261b4c9d2`
- **Commit purpose:** remove the barcode scan option from the home-page search field.
- **Versioned app URL for cache refresh:** https://bobbyt416.github.io/BLS-Printer-Inventory/?v=1872
- **Source verification:** after commit, fetched `index.html` again and confirmed the home scan button and its handler are absent, the visible version is 1.8.72, and the Add Printer scan handler remains. This was source verification, **not** a physical iPhone camera test.

## Immediate user priority and guardrails

The project is nearly complete. Preserve working behavior; avoid broad rewrites and regressions. Make only the requested, tightly scoped change, verify the current source before editing, increment the visible version, commit, then fetch the file again and verify the committed result. State exactly what was verified and what still needs the user's device test.

**Critical scanner constraint: Do not modify the Add Printer scanning workflow unless the user explicitly asks.** The user has spent significant time getting it to work and is especially sensitive to regressions.

## Scanner status and decisions

### Home page
- As of **1.8.72**, the barcode-scan icon/button beside the home-page search input has been removed.
- The `scanSearchBtn` element, its click handler, and its dedicated button styling were removed.
- The ordinary home-page text search remains.
- Do not restore a home-page scanner unless the user specifically requests it.

### Add Printer screen
- The Add Printer form's own **Scan** button remains connected to `openScanner('formLabel')`.
- The Add Printer scanner's OCR-assisted label capture path remains in the code. Do not remove OCR from this path as part of home-page search changes.
- The user explicitly rejected OCR for barcode/serial recognition in the search context because it misread characters (examples included `g` for `6`, and confusion among `O`/`0`, `v`/`y`). Do not add OCR to the home search.
- Scanning previously worked inconsistently and sometimes required tilting the phone due to suspected subtle glare/reflection. User reported that Add Printer scanning had finally become usable; small changes later broke preview/recognition, so preserve it.
- Prior regressions included a black half-camera preview and a camera view zoomed so far in that the phone had to be held overhead. Do not reintroduce those CSS/camera changes.
- The user does not want to deal with scanning-library licensing; favor no-license-cost options if scanner work is requested.
- A previously verified product-number mapping noted in history: HP product number `F2A80A` → HP LaserJet Enterprise Flow MFP M527cm (type MFP). Do not infer a broad catalog from this single mapping without verified data.

### History of home-page scanner experiments (historical only)
Versions 1.8.64–1.8.71 iterated on home search barcode behavior, including exact-serial matching, avoiding product-number/OCR fallbacks, native BarcodeDetector, and Quagga with a dedicated target. There were display/recognition issues. These efforts are now moot because **1.8.72 removes the home scanner entirely**. Do not resume that work unless asked.

## Inventory, data, and sync safety

- Inventory is a business-critical dataset. Avoid changes that clear, migrate, overwrite, or silently replace inventory.
- The app has browser-local persistence and a Supabase cloud-sync implementation. Past sync problems included permissions, inventory appearing in one browser but not another, deleted records reappearing, and UX problems around forms/keyboard. Do not claim sync is healthy without evidence from the current implementation/runtime.
- Earlier code specifically attempted to preserve local records if the cloud returned no records and verified cloud deletion to prevent a deleted record from reappearing. Preserve such protections unless a verified bug requires a targeted fix.
- Never ask the user to clear site data or reinstall as a first-line step; this may risk inventory. If a data-related change is needed, recommend exporting a JSON backup first.
- Duplicate serial-number handling was improved in earlier iterations. If modifying save logic, preserve duplicate prevention and its user-visible short-lived message; duplicate attempts previously made the Save button appear dead.
- Default disposition requested: **Not evaluated**.
- Do not add OCR-driven auto-corrections to serial numbers. The user specifically found OCR unreliable.

## Current UI/report requirements to preserve

- Home inventory rows: two-line display per device showing **serial number, model number, asset tag, disposition, and page count** in the approved compact layout; no barcode shown on the home list.
- Add/Edit form: single-column layout was chosen after keyboard/scroll usability issues. Keep fields usable on mobile and avoid reintroducing form jumps or hidden active fields.
- Report defaults to **landscape** and uses clean rows without borders, alternating shading, and no Notes column in the printed report.
- Report field order: **Serial number, Model, Asset tag, Type, Page count, Disposition, Cost, Price, Status**. (The earlier user-level report preference also mentioned Product Number, Acquired from, Rep, Tech and Notes for broader report layouts; inspect the current report configuration before changing fields. The most recent explicit print requirement in this app history was the nine-field order above.)
- Report title/date format: `Printer Inventory Report DDMMMYY`, e.g. `Printer Inventory Report 26OCT26`. Filenames should include title/date and HHMMSS to avoid collisions.
- The Brother Lazer Service logo and existing blue gradient header are intentional. Avoid unrelated visual changes.

## Recommended workflow for any further code change

1. Fetch the latest `main/index.html` and record the current blob SHA. Never edit from an old Library copy or stale chat snippet.
2. Read the exact current implementation around the requested feature and identify dependencies/handlers/styles.
3. Make the smallest possible targeted edit. Preserve data/storage keys and unrelated behavior.
4. Increment the visible app version in the source. Do not skip versions.
5. Run a syntax check where possible. If no browser/device test was run, explicitly say so.
6. Commit the change to `main` only for the explicitly requested small release (this project has been worked directly on main; don't assume a separate test branch exists).
7. Fetch the committed file again and verify version, expected code, and absence of unintended edits.
8. If inventory data, sync, save, scanner, or print behavior changes, be extra conservative and tell the user what exact on-device check is needed.
9. Keep this handoff updated after meaningful changes, including date, version, commit SHA, verification performed, limitations, and next action.

## Current next action

No further code change is currently requested. The last request was to update this handoff so work can resume reliably in a new chat. Do not reopen scanner development; Version 1.8.72's home-scan removal is the intended current behavior. Continue only when the user gives the next instruction.


## Update — 2026-10-28 (v1.8.73)
- Added Fax and Wireless Accessory selects to Add/Edit Printer and values to Machine Details. Values are "Yes"/"No"; blank "Not recorded" preserves unknown legacy values rather than assuming No.
- Both fields use the existing fields loop, local JSON storage, backup data, and Supabase data JSON payload. No Supabase schema or sync-code change; no bulk migration or live inventory writes.
- Report fields Fax and Wireless Accessory immediately follow Page count. Existing print preferences are merged with defaults, enabling the new columns by default without resetting existing choices. CSV, DOC, PDF, and browser print share field order.
- Replaced obsolete print card overrides with a compact table, dynamic colgroup widths keyed by field name, narrower Model/Product Number/Type, and wrapped headers. Browser printing requests Letter landscape; beforeprint scales the entire report to the printable height and afterprint restores the screen view.
- Downloaded PDF uses explicit Letter landscape dimensions and measured row wrapping with adaptive font/row sizing to fit the selected inventory to one page. Large inventories/long notes can become small; use filters or fewer fields for readability. Defensive page continuation remains if exceptional content exceeds the calculated area.
- Add Printer barcode scanner implementation and vendor decoder unchanged. Automated label capture regression checked serial TEST000002 and product K0Q14A.
- Verification: inline JavaScript syntax passed. Headless Microsoft Edge exercised create/edit/reload persistence, read-only detail values, Yes/No report order and CSV headings, mocked Supabase upsert payload, and label capture. Sample 30-row browser-print and jsPDF outputs each verified as one 792 x 612 point landscape Letter page and visually inspected. Test inventory and mocked cloud only; no live Supabase writes.
- Physical camera decoding, physical iPhone, AirPrint, and multi-device live Supabase synchronization were not tested. Safari/AirPrint may override page orientation: use Save report → PDF and print/share the landscape PDF if native web printing stays portrait. Reference: https://github.com/mdn/browser-compat-data/issues/28626
