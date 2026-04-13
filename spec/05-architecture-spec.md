# 5. Architecture Specification

## 5.1 High-Level Components

```
┌─────────────────────────────────────────────────────────────┐
│                         macOS System                        │
│  ┌─────────────┐  ┌──────────────┐  ┌──────────────────┐  │
│  │ NSPasteboard│  │ Global HotKey│  │   Status Bar     │  │
│  └──────┬──────┘  └──────┬───────┘  └──────────────────┘  │
└─────────┼────────────────┼────────────────────────────────┘
          │                │
          ▼                ▼
┌─────────────────────────────────────────────────────────────┐
│                     ClipboardManager                        │
│  • Polling loop          • History state (max 100)         │
│  • Deduplication         • Search/filter logic              │
│  • Persistence (JSON)    • Selection index                  │
└────────────┬────────────────────────────────────────────────┘
             │
             ▼
┌─────────────────────────────────────────────────────────────┐
│                  PopupWindowController                      │
│  • NSPanel lifecycle       • First-responder management     │
│  • Key-event interception  • SwiftUI view hosting           │
└────────────┬────────────────────────────────────────────────┘
             │
             ▼
┌─────────────────────────────────────────────────────────────┐
│                    ClipboardPopupView                       │
│  • Search TextField        • Scrollable list                │
│  • Row selection           • 300 ms flash animation         │
│  • Scroll tracking         • Hover/delete interactions      │
└─────────────────────────────────────────────────────────────┘
```

## 5.2 Component Responsibilities

### ClipboardManager
**Role:** Core state machine and business logic

**Responsibilities:**
- Poll the system pasteboard at a fixed interval
- Detect changes and create `ClipboardItem` instances
- Enforce deduplication and FIFO limit
- Expose filtered history based on the current search query
- Manage the selected index for keyboard navigation
- Serialize/deserialize history to `~/.yaccm/history.json`

**Invariants:**
- `items.count` shall never exceed 100
- `selectedIndex` shall always be a valid index into `filteredItems` or 0

### PopupWindowController
**Role:** Window manager and input router

**Responsibilities:**
- Create, show, hide, and destroy the popup window
- Ensure the window is floating, cross-Space, and always-on-top
- Restore the previous window frame
- Intercept local key-down events for `↑`, `↓`, `Return`, and `Esc`
- Forward relevant events to `ClipboardManager` or close the popup
- Notify the UI layer when the popup is shown (for focus management)

### ClipboardPopupView
**Role:** User interface rendering

**Responsibilities:**
- Render the search field and history list
- Apply selection backgrounds and flash animations
- Handle row hover and delete-button visibility
- Observe show/selection notifications from the controller layer
- Scroll the list to keep the selected row in view

### StatusBarController
**Role:** System tray integration

**Responsibilities:**
- Create and update the menu-bar status item
- Provide menu actions (Show, Quit)
- Hold a weak reference to the popup controller for the "Show" action

### HotKeyManager
**Role:** Global shortcut registration

**Responsibilities:**
- Register a system-wide hotkey with the OS
- Invoke a callback when the hotkey is pressed
- Clean up registration on deallocation

## 5.3 Data Model

### ClipboardItem

```
ClipboardItem
├── id: UUID           // Unique identifier
├── text: String?      // Plain text payload
├── imageData: Data?   // Binary image payload
└── timestamp: Date    // Capture time
```

**Rules:**
- Exactly one of `text` or `imageData` shall be non-nil
- `text` and `imageData` shall not both be present
- `signature` is computed from the payload for deduplication
- `preview` is a display-safe, trimmed string

### History Store

```
~/.yaccm/history.json
├── Format: JSON array of ClipboardItem
├── Max size: 100 items
└── Auto-saved triggers: insert, delete, clear
```

## 5.4 Data Flow

### Capture Flow
1. `HotKeyManager` / `StatusBarController` triggers `PopupWindowController`
2. `ClipboardManager` polls `NSPasteboard` every 500 ms
3. On change, `ClipboardManager` creates a `ClipboardItem`
4. Deduplication check prevents insertion if signature matches the newest item
5. Item is inserted at index 0; FIFO evicts index 100 if needed
6. `ClipboardManager` writes the updated list to disk

### Search Flow
1. User types in the search field
2. `ClipboardManager` recomputes `filteredItems`
3. `selectedIndex` resets to 0
4. `ClipboardPopupView` re-renders the list
5. `ScrollViewReader` scrolls to the newly selected item

### Copy Flow
1. User presses `Return` or clicks a row
2. `ClipboardManager.copyItem()` writes the raw payload to `NSPasteboard`
3. A 300 ms flash animation is triggered on the selected row
4. `PopupWindowController.hide()` closes the window

## 5.5 Error Handling Strategy

| Failure | Behavior |
|---------|----------|
| Corrupted JSON on load | Discard file, start empty |
| Pasteboard read failure | Skip cycle, retry next poll |
| Invalid regex in search | Fall back to substring match |
| Empty filtered list | Disable `Return` copy action |
| Window creation failure | Log silently, retry on next hotkey |

## 5.6 Boundaries

- **ClipboardManager** knows nothing about windows or UI frameworks
- **PopupWindowController** knows nothing about persistence or pasteboard internals
- **ClipboardPopupView** knows nothing about hotkeys or file I/O
- **StatusBarController** only knows how to show/quit the app
