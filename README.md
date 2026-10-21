# BLS Printer Inventory

This folder contains the BLS Printer Inventory web app, prepared for GitHub Pages.

## Upload to GitHub

1. On GitHub, create a **new repository** named `BLS-Printer-Inventory`.
2. Keep it separate from the Copier-Wizard repository.
3. Open the new repository and choose **Add file → Upload files**.
4. Unzip this package on your computer, then upload `index.html` and `README.md` from this folder.
5. Commit the uploaded files to the repository's `main` branch.

## Enable GitHub Pages

1. In the repository, open **Settings → Pages**.
2. Under **Build and deployment**, choose **Deploy from a branch**.
3. Choose branch `main` and folder `/(root)`, then save.
4. Wait for GitHub Pages to publish. The site URL should look like:
   `https://YOUR-GITHUB-USERNAME.github.io/BLS-Printer-Inventory/`

Send ChatGPT the actual published URL once it appears, and it can help check the site and troubleshoot.

## Important data note

This app currently stores inventory in the browser on the device where it is used. It does not currently synchronize inventory to other devices or users. Export backups regularly. Clearing browser website data can remove locally stored records.
