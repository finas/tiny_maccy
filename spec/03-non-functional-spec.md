# 3. Non-Functional Specification

## 3.1 Performance

### REQ-NF-PERF-001: Search Latency
Search and filtering operations shall complete in **< 16 ms** for the default history size of 100 items, ensuring a 60 fps UI experience.

### REQ-NF-PERF-002: Clipboard Detection Latency
The system shall detect and capture clipboard changes within **500 ms** of the mutation occurring.

### REQ-NF-PERF-003: Popup Open Latency
The popup shall become visible and interactive within **200 ms** of the global hotkey being pressed.

### REQ-NF-PERF-004: Memory Footprint
The application shall consume **< 100 MB** of resident memory under normal operation (100 history items, mixed text and images).

### REQ-NF-PERF-005: CPU Usage
Background polling shall use **< 1 %** CPU on average when idle.

---

## 3.2 Reliability

### REQ-NF-REL-001: Crash-Free Sessions
The application shall maintain a crash-free session rate of **> 99.9 %**.

### REQ-NF-REL-002: Corruption Recovery
If the persistence file (`history.json`) is corrupted, truncated, or unreadable, the application shall silently recover by discarding the file and initializing an empty history. No user-facing error dialog shall be shown.

### REQ-NF-REL-003: Graceful Degradation
If the system pasteboard is temporarily inaccessible (e.g., locked by another process), the application shall skip the polling cycle and retry on the next cycle without crashing or logging excessive errors.

---

## 3.3 Security & Privacy

### REQ-NF-SEC-001: Local-Only Storage
All clipboard history data shall be stored **exclusively on the local machine**. No network transmission of clipboard data shall occur.

### REQ-NF-SEC-002: Plain-Text Storage Disclosure
Because history is persisted as **plain JSON**, the specification explicitly acknowledges that:
- Passwords, API tokens, and other sensitive data may be written to disk unencrypted
- No automatic detection or exclusion of sensitive content is implemented in this release

### REQ-NF-SEC-003: No Elevated Privileges
The application shall not require administrator privileges, `sudo`, or system extensions to install or run.

---

## 3.4 Portability & Build

### REQ-NF-BLD-001: Swift Package Manager Support
The project shall be buildable using the Swift Package Manager (`swift build`, `swift run`).

### REQ-NF-BLD-002: Xcode Support
The project shall also be importable and buildable within Xcode for developers who prefer IDE-based workflows.

### REQ-NF-BLD-003: Platform Target
The minimum supported macOS version shall be **macOS 13.0 (Ventura)**.

---

## 3.5 Maintainability

### REQ-NF-MAINT-001: Single Source of Truth
All Swift source files shall reside in a single shared directory (`Sources/yaccm/`) so that both SPM and Xcode builds consume the same code.

### REQ-NF-MAINT-002: Framework-Agnostic Specs
Functional and UX specifications shall be expressed without reference to specific UI frameworks (e.g., SwiftUI, AppKit) so they remain valid if the implementation stack changes.
