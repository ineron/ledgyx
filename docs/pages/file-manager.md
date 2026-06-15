# File Manager

The File Manager gives you a visual browser for files stored on your tenant disk — images, CSS, JavaScript, JSON data files, and anything else your platform serves.

<p align="center">
  <img src="../../images/file-manager-page.png" alt="File Manager — folder and file grid" width="90%">
</p>

## Layout

**Left sidebar:**
- **All Files** — browse everything
- **Recent** — recently uploaded or accessed files
- **Recovery** — files marked for recovery
- **Deleted** — soft-deleted files (can be restored or permanently removed)
- Storage progress bar showing used vs. available space

**Main area:**
- When inside a folder: files displayed as a grid with image thumbnails for images, type icons for other files
- Hover any file to reveal **View** (eye icon) and **Download** buttons

## Browsing folders

Click a folder card to enter it. The **← Back** button at the top takes you to the parent folder. Breadcrumb navigation is shown in the header of the content area.

## Uploading files

Click **Upload** to select files from your computer. Multiple files can be uploaded at once. Uploaded files appear in the current folder immediately.

Supported: images, CSS, JavaScript, HTML, JSON, text files, archives (ZIP).

## Creating folders

Click **+ New Folder** and enter a folder name. The folder appears in the grid immediately.

## Drag-and-drop file moves

Move a file to a different folder by dragging it onto a folder card. A violet highlight indicates the drop target. You can also drag files onto the **← Back** button to move them to the parent folder.

## Viewing files

For text files (HTML, CSS, JS, JSON, markdown), click the **Eye icon** on hover to open a **read-only viewer** powered by Monaco:

<p align="center">
  <img src="../../images/file-manager-page.png" alt="File Manager Monaco viewer" width="90%">
</p>

The viewer shows:
- The filename and detected language
- Full file content with syntax highlighting
- A **Download** link
- **← Back** to return to the file grid

## Deleting files

Click the **trash icon** on a folder or file card. Files go to the **Deleted** filter — they're soft-deleted and can be recovered.

In the Deleted view:
- **Delete All** — soft-deletes all visible items
- **Clear Trash** — permanently removes all deleted files (irreversible)

## Serving files to the web

Files on your tenant disk are available at:
```
https://{config-id}.ledgyx.com/cdn/{path/to/file}
```

Reference them in your HTML templates:
```html
<link rel="stylesheet" href="/cdn/css/style.css">
<img src="/cdn/images/hero.jpg" alt="Hero">
<script src="/cdn/js/app.js"></script>
```

## Tips

- Organize CSS and JS into subfolders (`css/`, `js/`) to keep the root clean.
- The AI Builder uses File Manager's disk when writing CSS/JS files with `write_disk_file` — files it creates appear here automatically.
- Image thumbnails are shown inline for `image/*` MIME types — hover to download or view details.
- Storage usage is shown in the left sidebar — check it before uploading large assets.
