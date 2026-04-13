# 7. Roadmap

## 7.1 Release Timeline

| Phase | Version | Theme | Target Window |
|-------|---------|-------|---------------|
| MVP | 1.0 | Core utility | Current |
| Polish | 1.1 | UX refinement | +4 weeks |
| Power | 1.2 | Advanced features | +10 weeks |
| Enterprise | 2.0 | Security & sync | +6 months |

---

## 7.2 Phase 1 — MVP (v1.0)

**Status:** Complete / In Review

**Deliverables:**
- [x] Background clipboard capture (text + images)
- [x] JSON persistence and restore
- [x] Global hotkey popup
- [x] Real-time search with regex fallback
- [x] Keyboard navigation (`↑`, `↓`, `Return`, `Esc`)
- [x] Mouse copy and hover-delete
- [x] Menu bar integration
- [x] 300 ms selection flash feedback
- [x] Persistent search and selection across popup sessions

---

## 7.3 Phase 2 — UX Polish (v1.1)

**Theme:** Make the app feel invisible and effortless.

**Planned Features:**

| ID | Feature | Description | Priority |
|----|---------|-------------|----------|
| R2-1 | Cursor-aware positioning | Open the popup near the mouse cursor or active text caret instead of screen center | P1 |
| R2-2 | Pin / favorites | Allow users to pin frequently used items so they are never evicted | P1 |
| R2-3 | Keyboard delete | Press `Cmd+Delete` or `Shift+Delete` to remove the selected item without using the mouse | P2 |
| R2-4 | Configurable history size | Expose a setting to change the 100-item limit (e.g., 50–500) | P2 |
| R2-5 | Launch at login | Integrate `SMLoginItemSetEnabled` so the app starts automatically | P2 |

---

## 7.4 Phase 3 — Power User (v1.2)

**Theme:** Speed, scale, and customization.

**Planned Features:**

| ID | Feature | Description | Priority |
|----|---------|-------------|----------|
| R3-1 | Automatic paste-back | After copying a selected item, simulate `Cmd+V` in the previous application (requires accessibility permissions) | P1 |
| R3-2 | SQLite backend | Replace JSON with SQLite for faster startup, lazy loading, and larger histories | P1 |
| R3-3 | Fuzzy search | Replace regex/substring with `fzy`-style fuzzy scoring for typo-tolerant search | P1 |
| R3-4 | Configurable hotkey | Allow users to remap the global shortcut from `Cmd+Shift+C` to any combination | P2 |
| R3-5 | Ignore-list for sensitive apps | Detect when a specific app (e.g., 1Password, Keychain Access) owns the frontmost window and suppress clipboard capture | P1 |
| R3-6 | Rich text / HTML support | Capture and preview RTF and HTML clipboard types | P2 |
| R3-7 | Image compression | Compress or downsample large images before persistence to control disk usage | P2 |

---

## 7.5 Phase 4 — Enterprise (v2.0)

**Theme:** Security, compliance, and multi-device workflows.

**Planned Features:**

| ID | Feature | Description | Priority |
|----|---------|-------------|----------|
| R4-1 | Encrypted history | Encrypt the local store at rest using the user's Keychain-derived key | P1 |
| R4-2 | Cloud sync | Optional iCloud (or custom backend) sync across the user's Macs | P2 |
| R4-3 | iOS companion | A lightweight iOS/iPadOS app that can read and write synced clips | P3 |
| R4-4 | Audit log | Log metadata (app origin, timestamp) for compliance environments | P3 |
| R4-5 | Team policies | Admin-configurable max retention, block-list apps, and mandatory encryption | P3 |

---

## 7.6 Decision Gates

Before each phase begins, the following must be true:

1. **Phase 2:** MVP has been in daily use by the core team for 2 weeks with zero critical bugs.
2. **Phase 3:** User feedback from Phase 2 indicates at least one power-user feature is strongly requested.
3. **Phase 4:** There is concrete demand from pro users or organizations for encryption and sync.
