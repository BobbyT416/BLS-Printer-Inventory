# BLS Printer Inventory

Mobile-friendly inventory for tracking printers received for refurbishment, condition, costs, and sale status.

## Live app

[Open the BLS Printer Inventory](https://bobbyt416.github.io/BLS-Printer-Inventory/)

## Shared inventory and backups

The live app connects to a shared database so changes can appear for users on other devices. The app also keeps a local browser copy. Check the connection status in the app: when the shared inventory is unavailable, changes may remain only on the current device until the app reconnects.

Use **Backup & Settings → Export Backup** regularly to save a JSON copy. Importing a backup replaces the inventory currently stored on that device. Clearing browser website data can remove its local copy.

The test build at `/test/` has shared sync disabled and should not be used for entering production inventory.
