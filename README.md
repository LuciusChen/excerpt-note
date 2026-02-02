# passages.el

A lightweight Emacs package for seamless PDF and EPUB annotation with Org-mode.

**Version:** 3.0.0

## Philosophy

Academic reading often involves complex annotations: multi-paragraph notes, inline images, LaTeX equations, code blocks, and lengthy excerpts. Traditional annotation tools force you to think about structure—where does the note end? How do I separate my thoughts from the source material?

**passages** eliminates this friction with a clean, compact format:

```
● P42 The extracted text from the source document appears here.
[Your notes below - write freely]

● P43 Another passage continues...
```

Each passage starts with a page marker (`● P42`) followed by the source text. Your notes go below. The page number is derived from metadata and rendered via overlay - ensuring it always stays in sync with the actual location.

## Features

- **Compact visual format**: Clean `● Pxx` markers (page derived from metadata via overlay)
- **Bidirectional navigation**: Jump between PDF/EPUB and notes seamlessly
- **Precise location anchors**: Return to exact scroll positions, not just pages
- **Org-mode native**: Full support for images, LaTeX, tables, and all Org features
- **Smart text extraction**: Handles mixed font sizes, Drop Caps, multi-column layouts, and hyphenated words
- **Smart file resolution**: Works with relative paths, bibliography entries, and configurable search paths
- **Denote & Citar integration**: Automatically links to existing literature notes
- **Visual rendering**: Overlay system with colored page markers and gray excerpt text
- **JIT fontification**: Efficient rendering for large note files

## Installation

Requires Emacs 27.1+, [pdf-tools](https://github.com/vedang/pdf-tools), and [nov.el](https://depp.brause.cc/nov.el/).

```elisp
(use-package passages
  :load-path "path/to/passages"
  :commands (passages-enable passages-disable)
  :config
  (passages-enable))
```

To disable: `M-x passages-disable`

## Usage

### In PDF/EPUB buffers (`passages-doc-mode`)

| Key | Command | Description |
|-----|---------|-------------|
| `e` | `passages-insert` | Extract selected text (or create empty anchor) |
| `i` | `passages-open-notes` | Open/create notes file |
| `I` | `passages-find-this-location-in-notes` | Jump to notes for current page |

### In Org notes buffer (`passages-mode`)

| Key | Command | Description |
|-----|---------|-------------|
| `C-c e j` | `passages-jump-to-source` | Jump to source location |
| `C-c e r` | `passages--refresh-overlays` | Refresh visual rendering |
| `C-c e d` | `passages-debug` | Show detected excerpts |
| `C-c e k` | `passages-delete-at-point` | Delete excerpt at point |
| `C-c e l` | `passages-list-excerpts` | List all excerpts in buffer |

## Workflow

1. Open a PDF or EPUB
2. Select text and press `e` to excerpt
3. Notes file opens in another window with cursor ready for your annotations
4. Write your thoughts below the excerpt
5. Press `C-c e j` on any excerpt to jump back to the source

## Configuration

### Basic Settings

```elisp
;; Where to search for source files
(setq passages-search-paths
      '("~/Documents/papers/"
        "~/Documents/books/"))

;; Use relative paths for portability
(setq passages-use-relative-paths t)

;; Custom directory for note files (nil = next to source files)
(setq passages-directory nil)

;; Auto-parse excerpts when opening files
(setq passages-auto-parse t)

;; Enable smart text extraction for PDFs (default: t)
(setq passages-smart-text-extraction t)
```

### Denote & Citar Integration

```elisp
;; Integrate with Denote
(setq passages-use-denote t)
(setq passages-denote-keyword "excerpt")

;; Integrate with Citar
(setq passages-use-citar t)
```

### Debug Mode

```elisp
;; Enable verbose logging for troubleshooting
(setq passages-debug t)
```

## Note File Structure

Passages support multi-line content:

```org
● The extracted text from the source document.
It can span multiple lines if you manually
edit or reformat the passage. ⟦file.pdf|(15 . 0.2)⟧

Your notes go here. Write as much as you need.
Include images: [[./figures/diagram.png]]
Or LaTeX: $E = mc^2$

● Another passage... ⟦file.pdf|(16 . 0.3)⟧

More notes for this passage...
```

**Displayed as** (with overlays active):
```
● P15 The extracted text from the source document...

Your notes go here...

● P16 Another passage...
```

- `●` marks the start of a passage
- `Pxx` is rendered via overlay (derived from location metadata, not editable)
- Content between `●` and `⟦...⟧` is the passage (displayed in gray)
- `⟦file|location⟧` is metadata (hidden when mode is active)
- Everything after `⟦...⟧` is your notes (normal text)

## Commands Reference

### Interactive Commands

| Command | Description |
|---------|-------------|
| `passages-enable` | Enable passages globally |
| `passages-disable` | Disable passages globally |
| `passages-insert` | Insert excerpt from selected text |
| `passages-open-notes` | Open/create notes file for current document |
| `passages-jump-to-source` | Jump from note to source location |
| `passages-find-this-location-in-notes` | Find notes for current page |
| `passages-delete-at-point` | Delete the excerpt at point |
| `passages-list-excerpts` | List all excerpts in current buffer |

### Debugging Commands

| Command | Description |
|---------|-------------|
| `passages-debug` | Show all detected excerpts |
| `passages-debug-at-point` | Debug excerpt detection |
| `passages-check-conflicts` | Check for conflicting modes or settings |
| `passages-force-refresh` | Force re-render all overlays with logging |
| `passages-preview-extraction` | Preview text extraction for PDF selection |
| `passages-show-selected-text` | Show the currently selected text in PDF |

## Architecture

The package uses two minor modes:

- **`passages-mode`**: For Org buffers - handles overlays, JIT-lock fontification, and note keybindings
- **`passages-doc-mode`**: For PDF/EPUB buffers - provides document interaction keybindings

## Migrating from v2.x

The v3.0 format is more compact and supports multi-line passages:

**Old format (v2.x):**
```
[notes above]
@p.42 | document.pdf | (42 . 0.35)
[excerpt text]
-----
```

**New format (v3.0):**
```
● [passage text - can span
multiple lines] ⟦document.pdf|(42 . 0.35)⟧

[notes below]
```

The page number (P42) is derived from the location metadata and rendered via overlay - it's not stored in the file. This ensures the page marker always matches the actual location.

## License

GPL-3.0
