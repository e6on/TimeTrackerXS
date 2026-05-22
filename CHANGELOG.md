# Changelog

## 2.4

### New
- **"Rate Time Tracker XS" menu item** — a star-icon entry in the status-bar menu opens the App Store review page directly.
- **Automatic review prompt** — after 15 minutes of active use across 3 qualifying sessions, the system review dialog is shown via `SKStoreReviewController`.

### Changed
- **Sidebar rendered with plain gradients** — `NSVisualEffectView` blur/vibrancy removed from both the sidebar list and its header; each now uses a standalone `LinearGradient` overlay, giving a consistent appearance across active and inactive window states.
- **Sidebar header has a dedicated gradient** — the header area uses its own two-stop accent fade (`headerGradient`) separate from the sidebar list gradient, replacing the previous vibrancy + gradient composite.

## 2.3

### Changed
- **Running-timer color unified to orange** — all running-state indicators (row strip, play button, elapsed text, row background, sidebar count badge, and toolbar count) now use a single `runningOrange` semantic color (`#FF8C1A` dark / `#D15400` light) instead of a mix of `accentColor` and an inline-computed blue. The selected+running row background uses a more vibrant orange tint (`0.6` opacity) to clearly distinguish it from a merely selected row.
- **Sidebar row metadata hidden when empty** — the timer count, elapsed total, and running-count badge no longer render for groups (or the Ungrouped row) that contain zero timers, reducing visual clutter for unused groups.
- **Running count badge larger in sidebar** — the play icon and count text in group rows and the Ungrouped row increased from 8 pt to 12 pt, matching the toolbar badge size and improving legibility.

## 2.2

### New
- **Ungrouped elapsed total in sidebar** — the "Ungrouped" sidebar row now shows a live running total alongside grouped rows, ticking in real time while any ungrouped timer is active.
- **New app icon** — redesigned application logo across all sizes.
- **Dedicated menu-bar icon** — the status item uses a purpose-built `MenuIcon` asset instead of the app icon, giving a cleaner look at menu-bar resolution.
- **`ExportHelpers` module** — export logic (save panel, error alert) extracted into a standalone file shared by the menu-bar and window code paths.
- **Unit test suite** — `TimerStoreTests` added covering store operations.

### Changed
- **Observer registration before data load** — `TimerStore.init` no longer calls `load()` directly; `AppDelegate` now calls `store.loadData()` after registering the `dataLoadFailed` observer, ensuring the corrupt-file alert is never silently dropped on first launch.
- **`hidePanel` saves frame lazily** — panel frame is saved only when the panel is actually visible, avoiding a stale write when the panel has not been shown yet.
- **Menu-bar hero banner layout** — icon size increased to 48 pt and left-aligned at a fixed offset instead of centered, for a more balanced layout with longer version strings.

### Fixed
- **Ungrouped sidebar total not ticking** — the ungrouped row's elapsed display now updates every second while a timer is running, matching the behavior of named group rows.

## 2.1

### New
- **Export from menu bar** — CSV and JSON export are now available directly from the right-click status-bar menu, alongside Stop All Timers.
- **Edit menu / keyboard shortcuts** — Cut, Copy, Paste, Undo, Redo, and Select All (⌘X/C/V/Z/⇧Z/A) now work in all text fields.
- **Build number in version banner** — the menu-bar hero banner shows the full version string including build number (e.g. `2.1 (7)`).
- **Auto-named timers** — clicking the add button creates a timer with a generated name ("New Timer", "New Timer 2", …) instead of requiring a name up front.
- **Corrupt-file recovery** — if the save file cannot be decoded, it is copied to a `.json.corrupt` backup and a clear alert is shown; the app starts with an empty list rather than hanging or silently discarding data.
- **Sleep/wake crash recovery** — suspended timer IDs are now persisted so timers that were running before a sleep-wake cycle are correctly resumed even if the app was force-quit during sleep.

### Changed
- **Sidebar restyled** — uses `NavigationSplitView` with a resizable column (180–320 pt), an accent-tinted gradient background, and active/inactive window states.
- **Sidebar row detail** — each group row now shows timer count, running-timer indicator, and the group's elapsed total.
- **Duplicate name scoped to group** — timers in different groups may share the same name; uniqueness is only enforced within a group.
- **`add()` / `addGroup()` return ID** — both methods now return the new item's UUID on success (or `nil` on failure) instead of a Bool.
- **Status ticker is idle-aware** — the 1-second status-bar ticker only runs while at least one timer is active; the app is fully idle otherwise.
- **`setElapsed` stops running timer first** — manually setting elapsed time now correctly records any in-progress time before overwriting, preventing silent time loss.
- **Transparent title bar** — the main window uses a hidden title with a unified toolbar style for a cleaner look.
- **Window center fallback** — if the status-bar button's window is not yet available at startup, the panel centers on the main screen instead of failing silently.

### Fixed
- **DurationPicker field focus** — Tab and Shift-Tab navigate between H/M/S fields without triggering the ghost-window flash that appeared on first focus.
- **Ghost-window flash on first text field focus** — the shared field editor is pre-warmed at launch so the first `NSTextField` activation no longer causes a transient window to flicker.
- **Sidebar selection resets on group deletion** — if the selected group is deleted, selection falls back to the next available group or "Ungrouped".

## 2.0

### New
- **Time Tracker XS** — app renamed; window title and menu bar reflect the new name.
- **Two-column layout** — groups sidebar on the left, timer list on the right; drag the divider to resize.
- **Regular window** — replaced the borderless popover with a standard titled, resizable, movable window; position and size restored between sessions.
- **Group sidebar** — dedicated sidebar for navigating groups; click a group to filter its timers; ungrouped timers listed under "No Group".
- **New timers inherit current group** — adding a timer while a group is selected assigns it to that group automatically.
- **Menu bar hero banner** — right-clicking the status item shows a banner with the app name and version.

### Changed
- **Panel → Window** — the overlay panel is now a standard `NSWindow`; it no longer closes when you click outside and can be moved freely.
- **Sidebar width persisted** — the sidebar resize position is saved and restored on next launch.
- **Export no longer closes the window** — CSV/JSON save dialogs open without dismissing the app window.

## 1.2

### New
- **Timer Groups** — add named groups via the folder button in the header; assign timers via right-click context menu; each group shows a running total. Ungrouped timers are listed separately.
- **Resizable popover** — drag the bottom-edge handle to resize the panel; height remembered between sessions.
- **Version in header** — app version number displayed next to the "Time Tracker" title.
- **Sleep/wake awareness** — running timers automatically pause on system sleep and resume on wake.
- **Group name in exports** — CSV and JSON exports now include each timer's group.

### Fixed
- **Rename validation** — duplicate names (case-insensitive) and blank names are rejected with inline error feedback.
- **Edit buttons disabled while running** — reset, edit time, and delete are greyed out on a running timer.
- **Escape cancels edits** — pressing Escape abandons a name or time edit without saving.
- **Only one edit active at a time** — opening an edit in one row closes any open edit in another.
- **Edit time stops the timer** — manually setting elapsed time now stops the timer; previously it kept running from a new start point, risking double-counting.
- **Time input validation** — invalid minute/second values (e.g. `01:99:00`) are now rejected.
- **Export save panel** — file type filter now correctly set by extension.
- **CSV line endings** — now uses RFC 4180-compliant CRLF.
- **Crash recovery clock-skew guard** — recovered elapsed time can't go negative if the system clock is skewed.
