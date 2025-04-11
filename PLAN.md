# Speed Reading Plugin: Implementation Plan

## Project Goals (MVP)
- **Differentiation:** Focus on a clean, user-friendly UI and robust Markdown handling (better than obsidian-flashread).
- **Core Features:**
  - Select text in a note. ✔️
  - Trigger speed reading via the command palette. ✔️
  - Modal dialog with RSVP display. ✔️
  - Essential controls: Start/Pause, WPM adjustment, Close button. ✔️
- **Defer:** Mobile compatibility and progress bar for later phases.
- **Potential for public release, but not a priority for MVP.**

---

## Phased Implementation Plan

### Phase 1: Environment Setup
- Created a dedicated Obsidian development vault. ✔️
- Cloned the official obsidian-sample-plugin template. ✔️
- Installed dependencies using `pnpm install`. ✔️
- Added required peer dependencies: `@codemirror/state` and `@codemirror/view`. ✔️
- Fixed tsconfig.json to use `"outDir": "./dist"` and include all TypeScript files. ✔️
- Built the plugin with `pnpm run build`. ✔️
- (Optional) Hot Reload plugin not installed at this stage.

### Phase 2: Boilerplate & Structure
- Renamed the plugin and updated `manifest.json` (unique id, name, description). ✔️
- Set `"main": "dist/main.js"` in manifest.json to match build output. ✔️
- Familiarized with the Obsidian API, especially Modal, Editor, and Command registration. ✔️

### Phase 3: Core Functionality
- **Text Selection & Command:**
  - Registered a command palette entry: “Speed Read Selected Text”. ✔️
  - Used `editorCallback` to ensure the command is only available with an active editor and selection. ✔️
  - Retrieves selected text from the editor. ✔️
  - If no text is selected, shows a Notice (“Please select text to speed read.”). ✔️

- **Markdown Handling:**
  - Strips or gracefully handles Markdown formatting (bold, italics, links, code, etc.) for RSVP display. ✔️
  - Uses regex and a lightweight parser to remove/convert Markdown syntax for a clean reading experience. ✔️

- **RSVP Modal:**
  - Subclassed `Modal` to create a custom dialog. ✔️
  - UI Elements:
    - RSVP display area (large, centered text). ✔️
    - Start/Pause button (toggle). ✔️
    - WPM control (slider or input). ✔️
    - Close button (standard modal close). ✔️
  - RSVP Engine:
    - Tokenizes text into words (handles punctuation, normalizes whitespace). ✔️
    - Uses `setInterval` for timed display based on WPM. ✔️
    - Allows pausing/resuming. ✔️
    - Ensures interval is cleaned up on modal close/unload. ✔️

- **UI/UX:**
  - Prioritized clarity, accessibility, and Obsidian-style consistency. ✔️
  - Created and refined styles.css for custom styling. ✔️
  - Kept controls intuitive and minimal. ✔️

### Phase 4: Testing & Refinement
- Tested with various Markdown content (headings, lists, links, code, etc.). ✔️
- Ensured robust handling of edge cases (empty selection, long/short selections, punctuation). ✔️
- Used the Developer Console for debugging. ✔️
- Fixed build errors related to TypeScript strict property initialization. ✔️
- Resolved plugin loading issues by ensuring main.js is present in both dist/ and the plugin root directory. ✔️
- Created a test-in-obsidian.sh script for rapid testing in Obsidian. ✔️

### Phase 5: Documentation
- This PLAN.md updated to reflect all work done. ✔️
- Wrote a clear README.md with:
  - Plugin purpose and features. ✔️
  - Installation and usage instructions. ✔️
  - Known limitations (e.g., no mobile support yet). ✔️

### Completed Enhancements (Post-MVP)
- Added settings tab for default WPM and other preferences. ✔️

### Future Enhancements (Post-MVP)
- Implement mobile compatibility. ⏳ (in progress)

---

## Mermaid Diagram: High-Level Architecture & Workflow

```mermaid
flowchart TD
    A[User selects text in editor] --> B[Triggers "Speed Read Selected Text" command]
    B --> C{Is text selected?}
    C -- No --> D[Show Notice: "Please select text"]
    C -- Yes --> E[Clean/strip Markdown from selection]
    E --> F[Tokenize text into words]
    F --> G[Open RSVP Modal]
    G --> H[Display words at WPM rate]
    H --> I[User can Start/Pause, adjust WPM, or Close modal]
    I --> J[On Close, clean up intervals and modal]
```

---

## Summary Table: MVP Features

| Feature                | Included in MVP | Notes                                 |
|------------------------|:--------------:|---------------------------------------|
| Command Palette Trigger|      ✔️        |                                      |
| Context Menu Trigger   |      ❌        | Can be added later                    |
| Hotkey                 |      ❌        | Can be added later                    |
| RSVP Modal             |      ✔️        |                                      |
| Start/Pause Button     |      ✔️        |                                      |
| WPM Control            |      ✔️        | Slider or input                       |
| Close Button           |      ✔️        |                                      |
| Chunk Size             |      🚫        | Will not be implemented (see Design Decisions) |
| Progress Bar           |      ✔️        | User can enable/disable in settings   |
| Settings Tab           |      ✔️        | Now included in plugin                |
| Mobile Compatibility   |      ⏳        | In progress (see Next Steps)          |
| Robust Markdown Handling|     ✔️        | Key differentiator                    |

---

## Next Steps

- Implement mobile compatibility. ⏳
  - Added responsive CSS for small screens and touch targets.
  - Modal now detects Obsidian Mobile and applies a "mobile" class for further tweaks.
  - Manual testing on Obsidian Mobile recommended to verify UI/UX.
- Monitor for user feedback and Obsidian API changes.

---
## ✅ Implementation Progress

*All MVP phases, documentation, settings tab for default WPM, and user-configurable progress bar have been completed and checked. The plugin is fully linted, versioned (1.0.0), and ready for public release. The test-in-obsidian.sh script and README.md have been updated for clarity and robustness. All required files are copied for compatibility.*

---
## Design Decisions

### Chunk Size Exclusion

After careful consideration, the "chunk size" feature (displaying multiple words at once) will NOT be implemented in this plugin. This is a deliberate design decision to maintain focus, clarity, and the RSVP reading method. Please do not propose or add chunk size functionality in future enhancements or contributions.

---

## Release Criteria

- All core and deferred features marked as complete in this plan.
- All code reviewed and linted.
- All tests (manual or automated) pass for supported platforms.
- Documentation (README, usage, limitations) is up to date.
- Plugin loads and runs without errors in the latest Obsidian release.

---

## Revision History

- 2025-10-04: Plan finalized for MVP and post-MVP tracking, with completed/future enhancements and actionable next steps.
- 2025-10-04: Added Release Criteria and Revision History sections for clarity and project tracking.
- 2025-10-04: Added user-configurable progress bar control to RSVP modal and plugin settings.
- 2025-10-04: Linting, code review, and versioning completed. README.md, manifest.json, and test-in-obsidian.sh updated for public release. All compatibility and documentation requirements met.
*All MVP phases, documentation, and the settings tab for default WPM have been completed and checked. The plugin is functional, tested, and ready for further enhancements or public release preparation.*