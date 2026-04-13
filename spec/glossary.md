# Glossary

## A

**Accessibility Permissions**
macOS system permissions that allow an application to control the computer using accessibility APIs, commonly required for simulating keystrokes (e.g., automatic paste-back).

**Always-on-Top Window**
A window that remains visible above all other application windows, typically achieved by setting a high window level (e.g., floating panel).

## C

**Clipboard History**
A chronological list of items previously copied to the system clipboard.

**Cross-Space**
The ability of a window to appear in any macOS Mission Control Space, including full-screen app Spaces.

## D

**Deduplication**
The process of preventing identical or near-identical items from being recorded multiple times in the history.

**Dock Icon**
The application icon that appears in the macOS Dock. yaccm is configured to run as a background agent without one.

## F

**FIFO (First In, First Out)**
An eviction strategy where the oldest item is removed when a capacity limit is reached.

**Focus State**
The UI state indicating which control currently receives keyboard input.

## G

**Global HotKey**
A keyboard shortcut that is recognized system-wide, regardless of which application is currently active.

## H

**Hover State**
The visual change applied to a UI element when the user's mouse pointer is positioned over it.

## I

**IME (Input Method Editor)**
A software component that enables the entry of complex characters (e.g., CJK, emoji) and can interfere with raw keyboard event handling.

## L

**LSUIElement**
An `Info.plist` key that marks an application as a background agent, suppressing its Dock icon and application menu.

## M

**Mission Control Space**
A virtual desktop instance in macOS. Applications can be assigned to specific Spaces or allowed to appear in all Spaces.

## N

**NSPasteboard**
The macOS system service that manages the shared clipboard.

## P

**Pasteboard**
See **NSPasteboard**.

**Paste-Back**
The action of automatically inserting a previously copied item into the active application, typically by simulating a `Cmd+V` keystroke.

**Polling**
A technique where the application repeatedly checks a resource (here, the system pasteboard) at a fixed interval to detect changes.

## R

**Regex (Regular Expression)**
A sequence of characters that defines a search pattern, used here for advanced clipboard history filtering.

## S

**SMLoginItemSetEnabled**
An Apple API used to register a helper application to launch automatically when the user logs in.

**SPM (Swift Package Manager)**
Apple's official tool for managing the distribution of Swift code, including building and resolving dependencies.

**Status Bar**
The system menu bar at the top of the macOS screen. yaccm places a persistent icon there.

## T

**TIFF (Tagged Image File Format)**
A lossless image format used internally for storing captured image clipboard data.

**Truncation**
The visual shortening of text that exceeds the available display width, typically indicated by an ellipsis (`…`).

