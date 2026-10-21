# BLS Printer Inventory — Project Handoff

Last updated: 2026-10-21
Repository: https://github.com/BobbyT416/BLS-Printer-Inventory
Live app: https://bobbyt416.github.io/BLS-Printer-Inventory/
Production branch: main
Current version: 1.3.9

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
