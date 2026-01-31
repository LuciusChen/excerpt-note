# excerpt-note.el

A lightweight Emacs package for seamless PDF and EPUB annotation with Org-mode.

**Version:** 3.0.0

## Philosophy

Academic reading often involves complex annotations: multi-paragraph notes, inline images, LaTeX equations, code blocks, and lengthy excerpts. Traditional annotation tools force you to think about structure—where does the note end? How do I separate my thoughts from the source material?

**excerpt-note** eliminates this friction with a clean, compact format:

```
● P42 The extracted text from the source document appears here.
[Your notes below - write freely]

● P43 Another excerpt continues...
```

Each excerpt starts with a page marker (`● P42`) followed by the source text. Your notes go below. Simple, readable, and non-intrusive.

## Features

- **Compact visual format**: Clean `● Pxx` markers with styled content
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
(use-package excerpt-note
  :load-path "path/to/excerpt-note"
  :commands (excerpt-note-enable excerpt-note-disable)
  :config
  (excerpt-note-enable))
```

To disable: `M-x excerpt-note-disable`

## Usage

### In PDF/EPUB buffers (`excerpt-note-doc-mode`)

| Key | Command | Description |
|-----|---------|-------------|
| `e` | `excerpt-note-insert` | Extract selected text (or create empty anchor) |
| `i` | `excerpt-note-open-notes` | Open/create notes file |
| `I` | `excerpt-note-find-this-location-in-notes` | Jump to notes for current page |

### In Org notes buffer (`excerpt-note-mode`)

| Key | Command | Description |
|-----|---------|-------------|
| `C-c e j` | `excerpt-note-jump-to-source` | Jump to source location |
| `C-c e r` | `excerpt-note--refresh-overlays` | Refresh visual rendering |
| `C-c e d` | `excerpt-note-debug` | Show detected excerpts |
| `C-c e k` | `excerpt-note-delete-at-point` | Delete excerpt at point |
| `C-c e l` | `excerpt-note-list-excerpts` | List all excerpts in buffer |

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
(setq excerpt-note-search-paths
      '("~/Documents/papers/"
        "~/Documents/books/"))

;; Use relative paths for portability
(setq excerpt-note-use-relative-paths t)

;; Custom directory for note files (nil = next to source files)
(setq excerpt-note-directory nil)

;; Auto-parse excerpts when opening files
(setq excerpt-note-auto-parse t)

;; Enable smart text extraction for PDFs (default: t)
(setq excerpt-note-smart-text-extraction t)
```

### Denote & Citar Integration

```elisp
;; Integrate with Denote
(setq excerpt-note-use-denote t)
(setq excerpt-note-denote-keyword "excerpt")

;; Integrate with Citar
(setq excerpt-note-use-citar t)
```

### Debug Mode

```elisp
;; Enable verbose logging for troubleshooting
(setq excerpt-note-debug t)
```

## Note File Structure

Each excerpt uses a compact single-line format:

```org
● P15 The extracted text from the source document appears here,
preserving original formatting and handling line breaks intelligently.

Your notes go here. Write as much as you need.
Include images: [[./figures/diagram.png]]
Or LaTeX: $E = mc^2$

● P16 Next excerpt starts with another marker...

More notes for this excerpt...
```

The `● Pxx` marker is displayed with a larger, colored font. The excerpt content appears in gray to visually distinguish it from your notes.

## Commands Reference

### Interactive Commands

| Command | Description |
|---------|-------------|
| `excerpt-note-enable` | Enable excerpt-note globally |
| `excerpt-note-disable` | Disable excerpt-note globally |
| `excerpt-note-insert` | Insert excerpt from selected text |
| `excerpt-note-open-notes` | Open/create notes file for current document |
| `excerpt-note-jump-to-source` | Jump from note to source location |
| `excerpt-note-find-this-location-in-notes` | Find notes for current page |
| `excerpt-note-delete-at-point` | Delete the excerpt at point |
| `excerpt-note-list-excerpts` | List all excerpts in current buffer |

### Debugging Commands

| Command | Description |
|---------|-------------|
| `excerpt-note-debug` | Show all detected excerpts |
| `excerpt-note-debug-at-point` | Debug excerpt detection |
| `excerpt-note-check-conflicts` | Check for conflicting modes or settings |
| `excerpt-note-force-refresh` | Force re-render all overlays with logging |
| `excerpt-note-preview-extraction` | Preview text extraction for PDF selection |
| `excerpt-note-show-selected-text` | Show the currently selected text in PDF |

## Architecture

The package uses two minor modes:

- **`excerpt-note-mode`**: For Org buffers - handles overlays, JIT-lock fontification, and note keybindings
- **`excerpt-note-doc-mode`**: For PDF/EPUB buffers - provides document interaction keybindings

## Migrating from v2.x

The v3.0 format is more compact:

**Old format (v2.x):**
```
[notes above]
@p.42 | document.pdf | (42 . 0.35)
[excerpt text]
-----
```

**New format (v3.0):**
```
● P42 [excerpt text on same line]
[notes below]
```

Old format files are still partially supported for reading, but new excerpts use the compact format.

## License

GPL-3.0
