# BLS Printer Inventory — Project Handoff

Last updated: 2026-10-21
Repository: https://github.com/BobbyT416/BLS-Printer-Inventory
Live app: https://bobbyt416.github.io/BLS-Printer-Inventory/
Production branch: main
Current version: 1.3.8

## Purpose
Mobile-friendly browser app to track printers received for refurbishment, disposition, costs, sale status, and related inventory details.

## Current home inventory design
- Home page is a selectable list; tapping a row opens read-only Machine Details.
- Edit is done through the separate Edit Printer action.
- Home list columns, in order: Serial Number, Asset Tag, Disposition, Page Count.
- Full serial number must never be visually truncated.
- Model and other details remain available in Machine Details.
- Column labels are visible above the rows.
- On small screens, prioritize fitting Serial Number, Asset Tag, and Disposition. Page Count may require horizontal scrolling.
- Keep the heading and row columns aligned exactly. Avoid large empty gaps; use compact fixed column widths and small gaps/padding rather than large fractional grid columns.
- Current v1.3.8 mobile columns use 150px / 70px / 110px / 85px with 3px gaps. Verify on a real iPhone; adjust only if the first three columns are not legible and visible.
- The current screenshot before v1.3.8 showed only Serial Number and Asset Tag labels/values because a mobile CSS override restored large min-widths and gaps. v1.3.8 removes that override mismatch and uses compact matching widths in the header and rows.

## Record fields / behavior
Fields include Barcode, Serial Number, Asset Tag, Date Received, Type, Model, Page Count, Disposition, Refurbished By, Cost, Selling Price, Sale Status, Salesperson, and Refurbishment Notes.
- Printer Type options: Single function printer and MFP only.
- Serial number should auto-fill from barcode when blank and be uppercase on blur/save.
- Date Received should be blank for a new record.
- After saving, close the edit form and return to inventory.
- Do not claim persistence is confirmed without reload testing. Existing storage code has a possible issue: safeSet falls back to memory storage and suppresses localStorage errors, while persist can still return true. If save issues recur, verify storage persistence and communicate when data is session-only.

## Version history (recent)
- 1.3.3: five home columns; no-wrap/truncation styling.
- 1.3.4: removed Model from home list; order became Serial Number, Asset Tag, Disposition, Page Count; stopped clipping serial text and allowed horizontal scrolling.
- 1.3.5: restored visible column labels.
- 1.3.6: tightened padding and spacing.
- 1.3.7: attempted additional compact spacing, but mobile media rule still applied wider columns and 8px gaps; screenshot showed only first two columns.
- 1.3.8: aligned compact header/row columns and corrected mobile override to 150px / 70px / 110px / 85px with 3px gaps.

## Verification / release discipline
- Latest v1.3.8 committed to main.
- JavaScript syntax check passed; this does not verify visual layout on physical iPhone.
- Always fetch current main index.html before edits, make targeted changes, increment visible version, run JavaScript syntax check, commit to main, and report exact verification level.
- Never claim a real-device test unless one actually occurred.
- Do not alter unrelated repositories or projects.
