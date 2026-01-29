# excerpt-note.el

A lightweight Emacs package for seamless PDF and EPUB annotation with Org-mode.

**Version:** 2.9.0

## Philosophy

Academic reading often involves complex annotations: multi-paragraph notes, inline images, LaTeX equations, code blocks, and lengthy excerpts. Traditional annotation tools force you to think about structure—where does the note end? How do I separate my thoughts from the source material?

**excerpt-note** eliminates this friction. Each annotation unit has a clear, consistent structure:

```
[Your notes - write freely here]

@p.42 | document.pdf | (42 . 0.35)
[Extracted text from source]
-----
```

The `-----` separator cleanly delimits each unit. Write as much as you want above the marker line—multiple paragraphs, images, equations, whatever your thinking requires. The excerpt lives below the marker, preserving the original text. No need to manage boundaries manually; the structure handles it for you.

## Features

- **Bidirectional navigation**: Jump between PDF/EPUB and notes seamlessly
- **Precise location anchors**: Return to exact scroll positions, not just pages
- **Org-mode native**: Full support for images, LaTeX, tables, and all Org features
- **Smart paragraph detection**: Uses PDF layout analysis (Y gaps, X indentation, line lengths) to intelligently detect paragraph boundaries
- **Smart text extraction**: Automatically merges soft line breaks from PDFs while preserving paragraph structure
- **Smart file resolution**: Works with relative paths, bibliography entries, and configurable search paths
- **Denote & Citar integration**: Automatically links to existing literature notes
- **Visual rendering**: Clean overlay system hides metadata, shows friendly "p.42 ➜" badges
- **JIT fontification**: Efficient rendering for large note files
- **Protected metadata**: Modification hooks prevent accidental edits to excerpt markers

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

This removes all hooks and disables the minor modes in all active buffers.

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
4. Write your thoughts above the marker line
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
```

### Denote & Citar Integration

```elisp
;; Integrate with Denote
(setq excerpt-note-use-denote t)
(setq excerpt-note-denote-keyword "excerpt")

;; Integrate with Citar
(setq excerpt-note-use-citar t)
```

### Smart Paragraph Detection

```elisp
;; Enable smart paragraph detection (default: t)
;; Uses position information to detect paragraph boundaries
(setq excerpt-note-smart-paragraph-detection t)

;; Gap threshold: multiplier for average line gap to detect paragraph breaks
;; A gap larger than (avg-gap * threshold) indicates a paragraph break
(setq excerpt-note-paragraph-gap-threshold 1.5)

;; Indent threshold: minimum X offset (relative coords 0-1) to detect first-line indent
(setq excerpt-note-indent-threshold 0.02)

;; Short line threshold: ratio below which a line is considered short
;; May indicate paragraph end
(setq excerpt-note-short-line-threshold 0.7)

;; List item markers for detecting list items in PDF text
(setq excerpt-note-list-markers
      '("▶" "►" "•" "●" "○" "◆" "◇" "■" "□" "▪" "▫" "–" "—" "-" "\\*"))
```

### Debug Mode

```elisp
;; Enable verbose logging for troubleshooting
(setq excerpt-note-debug t)
```

## Note File Structure

Each excerpt unit follows this structure:

```org
Your notes go here. Write as much as you need.

Include images: [[./figures/diagram.png]]

Or LaTeX: $E = mc^2$

Multiple paragraphs are fine.

@p.15 | paper.pdf | (15 . 0.42)
The extracted text from the source document
appears here, preserving original formatting
across multiple lines if needed.
-----

Next excerpt starts after the separator...
```

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
| `excerpt-note-fix-file-paths` | Fix broken file paths in notes |
| `excerpt-note-update-source-file` | Update the SOURCE_FILE header |

### Debugging Commands

| Command | Description |
|---------|-------------|
| `excerpt-note-debug` | Show all detected excerpts |
| `excerpt-note-debug-at-point` | Debug excerpt detection at current position |
| `excerpt-note-check-conflicts` | Check for conflicting modes or settings |
| `excerpt-note-force-refresh` | Force re-render all overlays with logging |
| `excerpt-note-preview-smart-extraction` | Preview smart paragraph detection for PDF selection |
| `excerpt-note-show-selected-text` | Show the currently selected text in PDF |

## Architecture

The package uses two minor modes:

- **`excerpt-note-mode`**: For Org buffers - handles overlays, JIT-lock fontification, and note keybindings
- **`excerpt-note-doc-mode`**: For PDF/EPUB buffers - provides document interaction keybindings

## License

GPL-3.0
