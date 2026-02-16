# Lab 6 Features Design - Text Diff Viewer

## Overview

Add 2 features to complete Lab 6 requirements (4+ total features).

## Feature 3: localStorage Persistence

**Purpose:** Remember user's last comparison across sessions.

**Behavior:**
- On text input change, save both textareas to localStorage (debounced)
- On page load, restore saved text if present
- On "Clear All", remove saved data
- Keys: `diffViewer_original`, `diffViewer_modified`

**Implementation:** ~30 lines in existing script section.

## Feature 4: Keyboard Shortcuts

**Purpose:** Power-user efficiency.

**Shortcuts:**
- `Ctrl/Cmd + Enter` → Compare
- `Ctrl/Cmd + Shift + C` → Clear All
- `Ctrl/Cmd + S` → Swap texts (prevent default save dialog)

**Visual hints:** Add tooltip text to toolbar buttons showing shortcuts.

**Implementation:** ~25 lines - single keydown listener + button title attributes.

## Non-goals

- No settings panel for shortcuts
- No export/download features
- No syntax highlighting (beyond diff colors)

## Testing

Manual testing:
1. Type text, refresh page → text should persist
2. Press Ctrl+Enter → should trigger compare
3. Press Ctrl+Shift+C → should clear all
