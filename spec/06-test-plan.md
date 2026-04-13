# 6. Test Plan

## 6.1 Testing Strategy

| Type | Method | Responsibility |
|------|--------|----------------|
| Unit Tests | XCTest / Swift Testing | Engineers |
| Integration Tests | Manual + automated UI tests | QA + Engineers |
| Acceptance Tests | Manual checklist against this spec | QA |
| Performance Tests | Instrument (Time Profiler) | Engineers |
| Dogfooding | Daily use by the dev team | Whole team |

## 6.2 Acceptance Criteria Matrix

### Clipboard Capture

| ID | Test Case | Steps | Expected Result | Status |
|----|-----------|-------|-----------------|--------|
| TC-CAP-001 | Copy text via keyboard | Copy text with `Cmd+C` | Item appears at top of history within 500 ms | ⬜ |
| TC-CAP-002 | Copy image via keyboard | Copy image with `Cmd+C` | Image item appears at top of history | ⬜ |
| TC-CAP-003 | Deduplication | Copy the same text twice | Only one history entry exists | ⬜ |
| TC-CAP-004 | History limit | Copy 101 unique items | Oldest item is evicted; count remains 100 | ⬜ |
| TC-CAP-005 | Content fidelity | Copy text with leading/trailing newlines | Raw clipboard preserves newlines exactly | ⬜ |

### Persistence

| ID | Test Case | Steps | Expected Result | Status |
|----|-----------|-------|-----------------|--------|
| TC-PER-001 | Save on insert | Copy an item and quit app | `history.json` contains the new item | ⬜ |
| TC-PER-002 | Restore on launch | Launch app after copying items | Previous history is restored in correct order | ⬜ |
| TC-PER-003 | Corruption recovery | Replace `history.json` with invalid JSON | App launches with empty history and no crash | ⬜ |
| TC-PER-004 | Save on delete | Delete an item and force-quit | `history.json` no longer contains the deleted item | ⬜ |

### Popup & Search

| ID | Test Case | Steps | Expected Result | Status |
|----|-----------|-------|-----------------|--------|
| TC-POP-001 | Global hotkey | Press `Cmd+Shift+C` from another app | Popup opens within 200 ms | ⬜ |
| TC-POP-002 | Cross-Space | Enter full-screen app, press hotkey | Popup appears in the current Space | ⬜ |
| TC-POP-003 | Search filtering | Type "hello" in search field | List shows only items matching "hello" | ⬜ |
| TC-POP-004 | Regex fallback | Type "[" in search field | Falls back to substring; no crash | ⬜ |
| TC-POP-005 | Auto-select | Type any search query | First result is automatically selected | ⬜ |
| TC-POP-006 | Clear search | Delete all text in search field | Full history list is restored | ⬜ |

### Keyboard Navigation

| ID | Test Case | Steps | Expected Result | Status |
|----|-----------|-------|-----------------|--------|
| TC-KBD-001 | Arrow navigation | Press `↓` three times | Selection moves down three rows; scroll follows | ⬜ |
| TC-KBD-002 | Return to copy | Select an item, press `Return` | Row flashes for 300 ms, popup closes, clipboard updated | ⬜ |
| TC-KBD-003 | Esc to close | Press `Esc` | Popup closes immediately; clipboard unchanged | ⬜ |
| TC-KBD-004 | Focus on open | Open popup | Search field has keyboard focus immediately | ⬜ |

### Mouse Interaction

| ID | Test Case | Steps | Expected Result | Status |
|----|-----------|-------|-----------------|--------|
| TC-MOU-001 | Click to copy | Click a history row | Row flashes for 300 ms, popup closes, clipboard updated | ⬜ |
| TC-MOU-002 | Hover delete | Hover over a row | `×` button appears | ⬜ |
| TC-MOU-003 | Delete action | Click `×` on a row | Item is removed; popup stays open | ⬜ |

### Menu Bar

| ID | Test Case | Steps | Expected Result | Status |
|----|-----------|-------|-----------------|--------|
| TC-MEN-001 | Show from menu | Click status bar icon → Show | Popup opens | ⬜ |
| TC-MEN-002 | Quit from menu | Click status bar icon → Quit | App terminates cleanly | ⬜ |
| TC-MEN-003 | No dock icon | Launch app | No icon appears in the Dock | ⬜ |

### Session State

| ID | Test Case | Steps | Expected Result | Status |
|----|-----------|-------|-----------------|--------|
| TC-SES-001 | Remember selection | Select row 3, close popup, reopen | Row 3 remains selected | ⬜ |
| TC-SES-002 | Remember search | Type "test", close popup, reopen | Search field still contains "test"; list is filtered | ⬜ |
| TC-SES-003 | Smooth insert | Open popup, copy new text from another app | List scrolls to reveal new top item smoothly | ⬜ |

## 6.3 Performance Benchmarks

| Metric | Target | Measurement Method |
|--------|--------|-------------------|
| Popup open latency | < 200 ms | Stopwatch from hotkey to visible window |
| Search latency (100 items) | < 16 ms | Time Profiler during keystroke input |
| Clipboard detection latency | < 500 ms | Stopwatch from `Cmd+C` to list update |
| Memory footprint | < 100 MB | Activity Monitor / Instruments |
| CPU idle usage | < 1 % | Activity Monitor over 60 s |

## 6.4 Sign-Off Criteria

The MVP release is approved for ship when:

1. All **P0** test cases (TC-CAP-001 through TC-MEN-003) pass.
2. No critical or high-severity bugs remain open.
3. Performance benchmarks meet targets on a base-model M1 Mac.
4. The app has been dogfooded continuously for at least 3 business days without crashes.
