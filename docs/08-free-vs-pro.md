# Free vs Pro

PhpThunder's free tier is genuinely capable. You get a full PHP language server, Xdebug-based debugging, test discovery, diagnostics, formatting, and Composer integration — everything you need to be productive in a PHP project without spending a cent.

Pro adds the features that remove repetitive friction at scale: renaming things confidently, generating boilerplate you'd otherwise type by hand, profiling slow code, and seeing variable values inline while debugging.

This page reflects the current access boundary in the extension.

## Included in Free

The full base editing experience — everything in this list is available without a license:

- **Code intelligence:** completion, hover, go-to-definition, find references, import suggestions, and PHPDoc-backed inference for templates, array shapes, dynamic return types, magic methods, fluent APIs, aliases, and higher-order callbacks
- **Diagnostics:** parse errors, type analysis, PHPDoc issues, unused imports, unused private members
- **Quick fixes and source actions:** organize imports, generate PHPDoc stubs, pick import candidates
- **Formatting:** PSR-12 formatter with EditorConfig support and all standard VS Code editor settings (indentation, line endings, final newline, trailing whitespace)
- **PHPUnit and Pest:** full Test Explorer integration including discovery, run, and debug
- **Xdebug debugging:** CLI launch, web server launch, and attach mode
- **Composer helpers:** install, update, require, dump-autoload, validate
- **Include-path and interpreter management**
- **TODO panel tools**

## Requires Pro

These workflows unlock with Pro access or an active trial:

| Feature                             | Why it matters                                                                                                                                    |
| ----------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Rename Symbol**                   | Safely renames a class, method, function, or variable everywhere it's used across the whole project. No search-and-replace, no missed references. |
| **Generate Getter / Setter**        | Creates typed accessor methods for properties without writing boilerplate.                                                                        |
| **Generate missing accessors**      | Scans a class and fills in all missing getters and setters in one action.                                                                         |
| **Implement Missing Methods**       | Generates stubs for all abstract and interface methods a class is required to implement.                                                          |
| **Pro code fixes (CF diagnostics)** | A set of extra quick fixes beyond the free tier that fix code-quality patterns automatically.                                                     |
| **Formatting style customization**  | Controls array layout, parameter style, brace positions, spacing, and more via `phpThunder.formatting.*` settings.                               |
| **Profile Current File**            | Captures a Xdebug cachegrind profile for the active PHP script in one command.                                                                    |
| **Start Web Profiling**             | Starts a local PHP server with profiling enabled so you can profile real HTTP requests.                                                           |
| **Open Profiler**                   | Reviews and compares captured cachegrind profiles in a built-in panel.                                                                            |
| **Inline debug values**             | Shows current variable and expression values right in the editor while a debug session is paused — no need to switch to the Variables panel.      |

Pro also adds **starred completion suggestions** — the strongest match is pre-selected and top candidates are flagged — so the completion list requires fewer keystrokes.

## Feature matrix

| Workflow                         | Free | Pro                     |
| -------------------------------- | ---- | ----------------------- |
| Code intelligence & navigation   | ✓    | ✓ + starred suggestions |
| Diagnostics & basic code actions | ✓    | ✓ + Pro code fixes      |
| Formatting                       | ✓    | ✓ + style customization |
| PHPUnit & Pest (Test Explorer)   | ✓    | ✓                       |
| Xdebug debugging                 | ✓    | ✓ + inline debug values |
| Composer helpers & TODO tools    | ✓    | ✓                       |
| Rename Symbol                    |      | ✓                       |
| Accessor generation              |      | ✓                       |
| Implement Missing Methods        |      | ✓                       |
| Profiling (CLI & web)            |      | ✓                       |

## Trial and activation

PhpThunder offers a **30-day Pro trial** — full access to every Pro feature. The trial starts automatically when you first install the extension. No sign-in or activation step is needed to begin the trial.

The status bar shows your current state:

| Status             | Meaning                                                           |
| ------------------ | ----------------------------------------------------------------- |
| `PHP Free`         | Base features only                                                |
| `PHP Trial`        | Full Pro access during the trial period                           |
| `PHP Pro`          | Active Pro license                                                |
| `PHP Grace Period` | License recently expired; Pro features stay available temporarily |

For pricing plans and subscription details, visit [sailantis.com](https://sailantis.com).

## Related guides

- [Installation and activation](02-installation-and-activation.md) — activate a Pro license via `PhpThunder: Activate Pro`
- [Profiling](05-profiling.md) — Pro profiling workflows in detail
- [Troubleshooting](09-troubleshooting.md) — activation, trial, and compatibility notes
