# Text Diff Viewer

A single-file HTML tool for comparing two text files/snippets side-by-side with highlighted differences. Useful for code reviews, document comparisons, and config file changes.

## Features

- **Side-by-side comparison** - View original and modified text in parallel panels
- **Syntax highlighting** - Color-coded diff output:
  - Green for added lines
  - Red for removed lines
  - Yellow for changed lines
- **Line numbers** - Easy reference on both sides
- **File upload support** - Upload files via button or drag & drop
- **Auto-compare** - Automatic diff calculation as you type
- **Swap function** - Quickly switch original and modified content
- **Statistics** - View counts of added, removed, changed, and unchanged lines
- **Responsive design** - Works on desktop and mobile
- **Dark theme** - Easy on the eyes

## Supported File Types

.txt, .json, .md, .js, .ts, .css, .html, .xml, .yaml, .yml, .py, .java, .c, .cpp, .h, .go, .rs, .rb, .php, .sh, .sql, .csv

## Usage

1. Open `index.html` in any modern browser
2. Paste text in both panels, or upload/drag files
3. View the diff result below with highlighted changes

Or click **Load Sample** to see a demo comparison.

## Tech Stack

- Vanilla HTML/CSS/JavaScript
- No external dependencies
- Single file (~26KB)
- LCS-based diff algorithm

## Capacity

- Comfortable use: up to ~1,000-2,000 lines per side
- Usable with patience: up to ~5,000 lines
- Algorithm complexity: O(m × n) time and space

## License

MIT
