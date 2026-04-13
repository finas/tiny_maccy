# AI Agent Skill: Spec-Driven Development for yaccm

This document teaches AI agents how to autonomously implement, build, and test the **yaccm** clipboard manager by reading its specification documents and working entirely from the command line.

---

## 1. Core Workflow

Never write code before reading the spec. Follow this exact loop:

```
READ  spec/02-functional-spec.md    → What to build
READ  spec/05-architecture-spec.md  → Where it belongs
READ  spec/04-ui-ux-spec.md         → How it looks and feels (if UI-related)
WRITE code in yccam/Sources/yaccm/  → Implement
BUILD swift build / swiftc          → Verify compilation
TEST  manual smoke tests            → Verify behavior
REPORT results                      → Success or retry
```

---

## 2. How to Read the Specification

### 2.1 Document Hierarchy

| Order | Document | Read When |
|-------|----------|-----------|
| 1st | `spec/02-functional-spec.md` | Always. Every requirement has an ID (`REQ-XXX`). |
| 2nd | `spec/05-architecture-spec.md` | Always. Determines which component owns the change. |
| 3rd | `spec/04-ui-ux-spec.md` | If the change involves the popup, search, list, or visual feedback. |
| 4th | `spec/06-test-plan.md` | After implementation, to verify acceptance criteria. |

### 2.2 Requirement Translation

Each requirement ID maps directly to code. For example:

- `REQ-POP-001` (global hotkey opens popup) → needs a global shortcut component and a window controller.
- `REQ-KBD-002` (Return copies + 300 ms flash) → needs key-event handling and a timed UI state change.
- `REQ-CAP-003` (deduplication) → needs a content-signature check in the history manager.

**Rule:** If you cannot trace your code change back to at least one requirement ID, either the spec is incomplete or your change is out of scope. Ask before proceeding.

### 2.3 Architecture Boundaries

Respect these boundaries when generating code:

- **ClipboardManager** — owns state, persistence, pasteboard polling, search filtering, and selection index. Must not know about windows or UI frameworks.
- **PopupWindowController** — owns the popup window lifecycle, focus management, and key-event interception. Must not touch persistence.
- **ClipboardPopupView** — owns SwiftUI rendering, animations, hover/click interactions, and scroll tracking. Must not touch the pasteboard directly.
- **HotKeyManager** — owns global shortcut registration. Must not know about UI.
- **StatusBarController** — owns the menu bar icon. Must only show/quit.

---

## 3. How to Generate Code from Spec

### 3.1 Step-by-Step

1. **Identify the requirement IDs** you are implementing.
2. **Determine the target component** using `05-architecture-spec.md`.
3. **Generate the implementation** in the corresponding source file under `yccam/Sources/yaccm/`.
4. **Add only what the spec requires.** Do not add extra abstractions, configuration systems, or unused error handling.
5. **Preserve existing behavior.** If your change modifies an existing requirement, ensure all other requirements in the same area still pass.

### 3.2 File Placement Rules

- All Swift source files must live in `yccam/Sources/yaccm/`.
- If the spec requires a new data type, add it as a new `.swift` file in that directory.
- If the spec requires a new component, create a new file and wire it up in `yaccmApp.swift`.
- Never modify `yccam/yaccm/` directly — that directory is for Xcode resources only.

### 3.3 Coding Style

- Use 4-space indentation.
- Use `guard let` / `if let`; avoid force-unwrapping.
- Prefer explicit access control (`private`, `internal`).
- Keep functions small and single-purpose.
- Add comments only when the logic is non-obvious relative to the spec.

---

## 4. How to Install Dependencies

This project has **no external Swift package dependencies**. The only toolchain requirements are:

- macOS 13.0 or later
- Full Xcode.app (provides `swift`, `swiftc`, and `xcodebuild`)

### 4.1 Check Toolchain

Run this diagnostic first:

```bash
swift --version
xcodebuild -version 2>/dev/null || echo "xcodebuild missing"
```

### 4.2 If `swift` or `xcodebuild` Is Missing

**Install Xcode** from the Mac App Store or [Apple Developer](https://developer.apple.com/download/all/).

> There is no reliable fully-automated headless installer for Xcode. If the machine does not have Xcode.app, inform the user and stop.

### 4.3 If `xcode-select` Points to CommandLineTools

Fix it automatically:

```bash
sudo xcode-select -s /Applications/Xcode.app/Contents/Developer
sudo xcodebuild -license accept
```

---

## 5. How to Build

### 5.1 Primary Build: Swift Package Manager

```bash
cd yccam
swift build
```

If `Package.swift` is missing or corrupted, regenerate it automatically:

```swift
// swift-tools-version:5.7
import PackageDescription

let package = Package(
    name: "yaccm",
    platforms: [.macOS(.v13)],
    targets: [
        .executableTarget(
            name: "yaccm",
            linkerSettings: [
                .linkedFramework("Carbon"),
                .linkedFramework("AppKit")
            ]
        )
    ]
)
```

Then rerun `swift build`.

### 5.2 Fast Build: `swiftc`

For rapid iteration when SPM overhead is undesirable:

```bash
cd yccam/Sources/yaccm
swiftc -framework Carbon -framework AppKit *.swift -o yaccm
./yaccm
```

### 5.3 Xcode Build

If the user explicitly asks for an Xcode build:

```bash
cd yccam
xcodebuild -project yaccm.xcodeproj -target yaccm -configuration Debug CODE_SIGN_IDENTITY="" CODE_SIGNING_REQUIRED=NO
```

---

## 6. How to Test

### 6.1 Compilation Check

A successful `swift build` is the minimum gate.

### 6.2 Manual Smoke Tests

After every code change, run these tests in order. Any failure blocks the task.

| # | Test | Command / Action | Expected Result |
|---|------|------------------|-----------------|
| 1 | Launch app | `./yccam` or `.build/debug/yaccm` | App starts; status bar icon appears |
| 2 | Global hotkey | Press `Cmd+Shift+C` | Popup opens within 200 ms |
| 3 | Search filter | Type "hello" in search field | List filters instantly; first item selected |
| 4 | Keyboard copy | Press `↓` then `Return` | Row flashes; popup closes; clipboard updated |
| 5 | Mouse copy | Click a row | Row flashes; popup closes; clipboard updated |
| 6 | Delete item | Hover row → click `×` | Item removed; popup stays open |
| 7 | Persistence | Copy item → quit app → relaunch | History restored |
| 8 | Corruption recovery | Replace `~/.yaccm/history.json` with `{}` → relaunch | App starts with empty history; no crash |

### 6.3 Performance Check

While the app is running:

```bash
# CPU should be < 1% when idle
ps -o %cpu -p $(pgrep -f yaccm)

# Memory should be < 100 MB
ps -o rss= -p $(pgrep -f yaccm) | awk '{print $1/1024 " MB"}'
```

### 6.4 Test Plan Compliance

For larger features, cross-check against `spec/06-test-plan.md`. Each requirement ID in the spec maps to one or more test cases (`TC-XXX`). Ensure the relevant test cases pass before declaring the task complete.

---

## 7. Error Handling & Iteration

### 7.1 If Build Fails

1. Read the exact compiler error.
2. Fix the root cause in the source file.
3. Rebuild.
4. Do not apply speculative fixes unrelated to the error.

### 7.2 If Behavior Is Wrong

1. Identify which requirement ID is not satisfied.
2. Re-read the relevant section of `02-functional-spec.md`.
3. Trace the code path from user action to system response.
4. Fix the smallest possible change that satisfies the requirement.
5. Re-test.

### 7.3 If the Spec Is Ambiguous

Stop. Ask for clarification. Do not guess. A spec-driven project depends on the spec being the single source of truth.

---

## 8. Example Autonomous Session

**User:** "Implement REQ-KBD-002: when the user presses Return, flash the selected row for 300 ms before closing."

**AI actions:**

1. `READ spec/02-functional-spec.md` — confirms `REQ-KBD-002` details.
2. `READ spec/05-architecture-spec.md` — identifies `PopupWindowController` and `ClipboardPopupView` as the target components.
3. `READ yccam/Sources/yaccm/ClipboardPopupView.swift` — understands current row rendering.
4. `READ yccam/Sources/yaccm/PopupWindowController.swift` — understands current Return handling.
5. `EDIT` — adds a `selectingId` state to the view, triggers a 300 ms highlight, and updates the controller to delay close.
6. `BUILD` — runs `swift build`.
7. `TEST` — launches app, opens popup, presses `Return`, verifies flash and close.
8. `REPORT` — "Implemented REQ-KBD-002. Build passes. Smoke test verified."

---

## 9. Constraints & Rules

1. **Spec first, code second.**
2. **One requirement at a time.** Prefer small, verifiable changes.
3. **No external dependencies.** Do not add Swift Package Manager dependencies without explicit user approval.
4. **Preserve existing tests.** If smoke tests start failing, fix them before moving on.
5. **Update the spec if behavior changes permanently.** If the user asks for a change that contradicts the spec, edit the spec first, then the code.
