# 1. Overview

## Product Vision

**yaccm** is a minimal, always-available clipboard history application for macOS. It runs silently in the background, records every item copied to the system clipboard, and provides a fast, searchable popup interface for quick recall and reuse.

## Value Proposition

- **Never lose a copy.** Anything copied to the clipboard is captured automatically.
- **Retrieve in milliseconds.** A global hotkey opens the history popup from any app or Space.
- **Keyboard-first design.** Search, navigate, and paste without touching the mouse.

## Target Users

- Software developers copying code snippets, terminal commands, and URLs
- Writers and editors moving text between documents
- Designers copying colors, images, and assets
- Any macOS power user who performs more than ~10 copy operations per day

## Goals

1. Provide **zero-friction clipboard history** that works across all applications.
2. Ensure the user can locate and copy a previous item within **2 seconds** of opening the popup.
3. Maintain a **small resource footprint** (memory, CPU, disk).
4. Operate as a **background utility** with no Dock icon or intrusive chrome.

## Non-Goals

1. This is **not** a clipboard automation tool (e.g., no automatic paste-back into other applications).
2. This is **not** a cross-device sync solution.
3. This is **not** a rich-text editor or formatting converter.
4. This release does **not** include user-configurable preferences.

## Success Metrics

| Metric | Target |
|--------|--------|
| Time to open popup | < 200 ms |
| Search latency | < 16 ms for 100 items |
| Clipboard detection latency | < 500 ms |
| Crash-free sessions | > 99.9 % |
| User retention (7-day) | > 60 % |

## Constraints

- Must run on macOS 13.0 or later.
- Must not require administrator privileges to install or run.
- Must store all data locally on the user's machine.
