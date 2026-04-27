# Changelog

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
