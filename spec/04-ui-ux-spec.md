# 4. UI/UX Specification

## 4.1 Design Principles

1. **Invisible until needed.** The app lives in the menu bar and consumes zero screen real estate.
2. **Keyboard first.** Every primary action shall be reachable without the mouse.
3. **Immediate feedback.** Every user action shall produce a visible response within 100 ms.
4. **Forgiving.** Mistakes (bad regex, accidental deletions) shall be low-cost and recoverable.

## 4.2 Popup Layout

### Structure
```
┌────────────────────────────────────┐
│  🔍 Search...                      │  ← Search field (auto-focused)
├────────────────────────────────────┤
│  📄 First copied item          ×   │  ← Row 0 (selected)
│  📄 Second copied item         ×   │  ← Row 1
│  🖼️ Image [Image]               ×   │  ← Row 2
│  ...                               │
└────────────────────────────────────┘
```

### Dimensions
- **Width:** 400 pt
- **Height:** 500 pt
- **Minimum size:** Same as default (non-resizable in MVP)

### Visual Style
- **Background:** Translucent blur (sidebar material)
- **Borders:** None or 1 pt subtle separator
- **Corners:** System default for floating panels
- **Shadow:** System default drop shadow for floating windows

## 4.3 Search Field

- **Placeholder text:** `"Search..."`
- **Style:** Rounded border text field
- **Padding:** 16 pt on all sides
- **Behavior:**
  - Gains focus automatically on popup open
  - Filters list in real time
  - Does not block arrow-key navigation

## 4.4 List Row Anatomy

Each row shall contain, from left to right:

1. **Thumbnail / Icon** (optional)
   - 32 pt × 32 pt for images
   - Hidden for plain text
   - 8 pt padding from left edge

2. **Preview Text**
   - Single line only
   - Truncated with ellipsis if overflow
   - 8 pt horizontal spacing from thumbnail (if present)

3. **Delete Button** (`×`)
   - 16 pt × 16 pt
   - Right-aligned
   - Visible **only on hover**
   - 8 pt padding from right edge

### Row States

| State | Background | Text Style |
|-------|------------|------------|
| Default | Transparent | Regular |
| Hover | Very subtle gray tint | Regular |
| Selected | Accent color @ 30 % opacity | Regular |
| Flash (copying) | Accent color @ 80 % opacity | Slightly bold (optional) |

## 4.5 Interaction Flows

### Flow A: Copy via Keyboard
1. User presses global hotkey → popup opens
2. Search field is focused
3. User types query → list filters, first item auto-selected
4. User presses `↓` to navigate, `↑` to go back
5. User presses `Return`
6. Selected row flashes strong accent for 300 ms
7. Popup closes
8. Item is on the system clipboard

### Flow B: Copy via Mouse
1. User opens popup
2. User clicks a row
3. Row flashes strong accent for 300 ms
4. Popup closes
5. Item is on the system clipboard

### Flow C: Delete via Mouse
1. User hovers over a row
2. Delete (`×`) button fades in
3. User clicks the `×` button
4. Item is removed immediately
5. Popup remains open; selection index adjusts to stay valid

### Flow D: Dismiss Without Copy
1. User opens popup
2. User presses `Esc`
3. Popup closes immediately
4. Clipboard remains unchanged

## 4.6 Menu Bar Icon

- **Label:** `yaccm`
- **Font:** System menu-bar font
- **Menu items:**
  - `Show` — opens popup, identical to global hotkey
  - `Quit` — terminates app

## 4.7 Empty States

- **No history:** List area is blank; search field still functional
- **No search results:** List area is blank; first result auto-select logic is a no-op

## 4.8 Accessibility Considerations (Future)

- All interactive elements should be navigable via keyboard
- Selected row should expose correct `accessibilitySelected` state
- Search field should expose `accessibilityLabel="Search clipboard history"`
