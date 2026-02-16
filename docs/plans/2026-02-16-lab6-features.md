# Lab 6 Features Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Add localStorage persistence and keyboard shortcuts to text-diff-viewer for Lab 6 submission.

**Architecture:** Add two independent features to existing single-file HTML app. localStorage saves/restores textarea content. Keyboard listener handles shortcuts. Both integrate with existing event handlers.

**Tech Stack:** Vanilla JavaScript, localStorage API, KeyboardEvent API

---

### Task 1: localStorage Persistence

**Files:**
- Modify: `/Users/ariunzaya/Downloads/text-diff-viewer/index.html:780-792` (after debounceCompare function)

**Step 1: Add save/load functions after line 792**

Add this code right before the closing `</script>` tag (before line 793):

```javascript
        // localStorage persistence
        const STORAGE_KEYS = {
            original: 'diffViewer_original',
            modified: 'diffViewer_modified'
        };

        function saveToStorage() {
            localStorage.setItem(STORAGE_KEYS.original, originalText.value);
            localStorage.setItem(STORAGE_KEYS.modified, modifiedText.value);
        }

        function loadFromStorage() {
            const savedOriginal = localStorage.getItem(STORAGE_KEYS.original);
            const savedModified = localStorage.getItem(STORAGE_KEYS.modified);
            if (savedOriginal) originalText.value = savedOriginal;
            if (savedModified) modifiedText.value = savedModified;
            if (savedOriginal || savedModified) {
                debounceCompare();
            }
        }

        function clearStorage() {
            localStorage.removeItem(STORAGE_KEYS.original);
            localStorage.removeItem(STORAGE_KEYS.modified);
        }

        // Load saved content on startup
        loadFromStorage();
```

**Step 2: Hook save into existing debounceCompare**

Modify the debounceCompare function at line 782-789 to also save:

```javascript
        function debounceCompare() {
            clearTimeout(debounceTimer);
            debounceTimer = setTimeout(() => {
                saveToStorage();
                if (originalText.value || modifiedText.value) {
                    compare();
                }
            }, 500);
        }
```

**Step 3: Hook clearStorage into clearAllBtn handler**

Modify the clearAllBtn click handler at line 727-733 to also clear storage:

```javascript
        clearAllBtn.addEventListener('click', () => {
            originalText.value = '';
            modifiedText.value = '';
            originalFilename.textContent = '';
            modifiedFilename.textContent = '';
            diffSection.style.display = 'none';
            clearStorage();
        });
```

**Step 4: Test manually**

1. Open index.html in browser
2. Type text in both panels
3. Close and reopen browser tab
4. Verify text is restored
5. Click "Clear All"
6. Refresh - verify text is NOT restored

**Step 5: Commit**

```bash
cd /Users/ariunzaya/Downloads/text-diff-viewer
git add index.html
git commit -m "feat: add localStorage persistence for text inputs"
```

---

### Task 2: Keyboard Shortcuts

**Files:**
- Modify: `/Users/ariunzaya/Downloads/text-diff-viewer/index.html` (toolbar buttons + script section)

**Step 1: Add title attributes to toolbar buttons**

Update the toolbar buttons (lines 372-375) to show keyboard hints:

```html
        <div class="toolbar">
            <button class="btn btn-primary" id="compareBtn" title="Compare texts (Ctrl+Enter)">Compare</button>
            <button class="btn" id="clearAllBtn" title="Clear all (Ctrl+Shift+C)">Clear All</button>
            <button class="btn" id="sampleBtn">Load Sample</button>
        </div>
```

Also update the swap button (line 394):

```html
                <button class="btn btn-swap" id="swapBtn" title="Swap texts (Ctrl+S)">⇄</button>
```

**Step 2: Add keyboard event listener**

Add this code before the closing `</script>` tag (after the localStorage code):

```javascript
        // Keyboard shortcuts
        document.addEventListener('keydown', (e) => {
            const isMac = navigator.platform.toUpperCase().indexOf('MAC') >= 0;
            const modifier = isMac ? e.metaKey : e.ctrlKey;

            if (modifier && e.key === 'Enter') {
                e.preventDefault();
                compare();
            } else if (modifier && e.shiftKey && e.key.toUpperCase() === 'C') {
                e.preventDefault();
                clearAllBtn.click();
            } else if (modifier && e.key.toLowerCase() === 's') {
                e.preventDefault();
                swapBtn.click();
            }
        });
```

**Step 3: Test manually**

1. Open index.html in browser
2. Type text in both panels
3. Press Ctrl+Enter (or Cmd+Enter on Mac) - should compare
4. Press Ctrl+S (or Cmd+S) - should swap texts
5. Press Ctrl+Shift+C - should clear all
6. Hover over buttons - should see keyboard hints

**Step 4: Commit**

```bash
cd /Users/ariunzaya/Downloads/text-diff-viewer
git add index.html
git commit -m "feat: add keyboard shortcuts (Ctrl+Enter, Ctrl+S, Ctrl+Shift+C)"
```

---

### Task 3: Update README

**Files:**
- Modify: `/Users/ariunzaya/Downloads/text-diff-viewer/README.md`

**Step 1: Add new features to README**

Update the Features section to include:

```markdown
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
- **localStorage persistence** - Automatically saves and restores your last comparison
- **Keyboard shortcuts** - Ctrl+Enter (compare), Ctrl+S (swap), Ctrl+Shift+C (clear)
```

**Step 2: Commit**

```bash
cd /Users/ariunzaya/Downloads/text-diff-viewer
git add README.md
git commit -m "docs: add new features to README"
```

---

### Task 4: Push to GitHub

**Step 1: Push all commits**

```bash
cd /Users/ariunzaya/Downloads/text-diff-viewer
git push origin master
```

**Step 2: Verify**

Check GitHub repository to confirm all commits are visible.
