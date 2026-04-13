# 2. Functional Specification

## 2.1 Clipboard Capture

### REQ-CAP-001: Universal Capture
The system shall detect all clipboard mutations regardless of origin, including:
- Keyboard shortcuts (e.g., `Cmd+C`)
- Context-menu copy actions
- Programmatic writes by other applications
- Inter-application drag-and-drop operations that mutate the system pasteboard

### REQ-CAP-002: Supported Data Types
The system shall capture the following pasteboard types:
- **Plain text** (Unicode string content)
- **Images** (bitmap data, stored in a lossless format)

### REQ-CAP-003: Deduplication
The system shall compute a content signature for each captured item and shall not insert a new history entry if the signature matches the most recent existing entry.

**Signature rules:**
- Text: based on the full string value and length
- Image: based on the raw data byte count and dimensions

### REQ-CAP-004: History Limit
The system shall maintain a maximum history size of **100 items**. When a new item is captured and the limit is exceeded, the oldest item shall be removed (FIFO).

### REQ-CAP-005: Content Fidelity
The system shall preserve the exact raw clipboard payload—including all whitespace, line breaks, and invisible characters—when copying an item back to the system pasteboard. Visual previews may trim whitespace for display purposes only.

---

## 2.2 History Persistence

### REQ-PER-001: Automatic Save
History shall be persisted to local disk automatically upon any of the following events:
- A new item is captured
- An item is deleted by the user
- The entire history is cleared by the user

### REQ-PER-002: Automatic Restore
On application launch, the system shall read the persisted history file and restore the list. If the file is missing, empty, or unreadable, the app shall start with an empty history.

### REQ-PER-003: Storage Location
The persistence file shall be located at:
```
~/.yaccm/history.json
```

### REQ-PER-004: Corruption Recovery
If the persisted file is corrupted (malformed JSON), the system shall silently discard it and initialize a new empty history. No crash or user-facing error shall occur.

---

## 2.3 Popup Interface

### REQ-POP-001: Global Trigger
The popup shall be openable via a **global keyboard shortcut** that functions regardless of the currently active application or Space.

**Default shortcut:** `Cmd + Shift + C`

### REQ-POP-002: Floating Window Behavior
The popup shall appear as a **floating, always-on-top window** that:
- Does not steal focus aggressively from the active application
- Becomes the key window once opened (to accept keyboard input)
- Works across all macOS Spaces and full-screen applications

### REQ-POP-003: Window Positioning
The popup shall be centered on the primary screen by default. It shall remember and restore its last frame/position across application launches.

### REQ-POP-004: Background Agent
The application shall run as a background agent with **no Dock icon**.

### REQ-POP-005: Dismissal Methods
The popup shall close upon any of the following:
- The user copies a selected item
- The user presses `Esc`
- The user explicitly clicks outside the popup (optional, if supported by windowing layer)

---

## 2.4 Search

### REQ-SRC-001: Real-Time Filtering
The popup shall contain a persistent search field at the top. The history list shall filter **instantly on every keystroke** without requiring the user to press `Enter`.

### REQ-SRC-002: Regex Support
The search implementation shall attempt **case-insensitive regular expression** matching first. If the query is an invalid regex, it shall fall back seamlessly to plain substring matching.

### REQ-SRC-003: Auto-Select First Result
Whenever the search query changes, the first item in the filtered list shall be **automatically selected** so that keyboard navigation is always valid.

### REQ-SRC-004: Clear Search
Clearing the search query (empty string) shall restore the full, unfiltered history list.

### REQ-SRC-005: Search Persistence
The search query and selected index shall be **retained** when the popup is closed and reopened, unless the user explicitly modifies the query.

---

## 2.5 Keyboard Navigation

### REQ-KBD-001: Selection Navigation
Arrow keys (`↑` / `↓`) shall move the selection up and down the **filtered** list. These keys shall work even when the search field has first-responder focus.

### REQ-KBD-002: Confirm Action
Pressing `Return` shall:
1. Copy the currently selected item back to the system clipboard
2. Visually flash the selected row for **300 ms**
3. Close the popup

### REQ-KBD-003: Cancel Action
Pressing `Esc` shall close the popup **without** copying anything to the clipboard.

### REQ-KBD-004: Focus on Open
The search field shall automatically receive keyboard focus every time the popup is opened.

---

## 2.6 Mouse Interaction

### REQ-MOU-001: Click to Copy
Clicking any row in the history list shall:
1. Copy that item to the system clipboard
2. Visually flash the row for **300 ms**
3. Close the popup

### REQ-MOU-002: Hover Reveal
Hovering over a row shall reveal a **delete button** (`×`) on that row.

### REQ-MOU-003: Delete Action
Clicking the delete button shall remove that specific item from history **immediately** without copying it or closing the popup.

### REQ-MOU-004: Scroll Tracking
The list scroll view shall automatically scroll to keep the selected item visible during keyboard navigation.

---

## 2.7 Visual Rendering

### REQ-VIS-001: Text Preview
Each row shall display a **single-line text preview**. Text that exceeds the available width shall be truncated with an ellipsis (`…`).

### REQ-VIS-002: Image Preview
Rows containing image data shall render a **thumbnail preview** inline (maximum 32 pt × 32 pt).

### REQ-VIS-003: Selection Highlight
The currently selected row shall have a clearly visible highlight background distinct from unselected rows.

### REQ-VIS-004: Copy Flash
During the 300 ms copy action (triggered by `Return` or click), the selected row shall render with a **stronger accent highlight** to confirm the action to the user.

---

## 2.8 Menu Bar Integration

### REQ-MEN-001: Status Item
The application shall display a status bar item labeled **"yaccm"**.

### REQ-MEN-002: Menu Actions
The status bar menu shall provide the following actions:
- **Show** — opens the popup
- **Quit** — terminates the application

---

## 2.9 Session State

### REQ-SES-001: Persistent Selection
When the popup is reopened, the system shall restore:
- The last search query
- The last selected index

### REQ-SES-002: Smooth Insert Scroll
When a new clipboard item is captured while the popup is already open, the list shall scroll smoothly to reveal the newly inserted top item **without** resetting or jumping the user's current selection unexpectedly.
