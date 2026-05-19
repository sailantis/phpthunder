# Troubleshooting

This page collects the support checks that usually resolve setup and runtime issues. Start with Quick wins first; most problems collapse to one of those checks.

## Quick wins

| Symptom                                 | Fix                                                                                |
| --------------------------------------- | ---------------------------------------------------------------------------------- |
| Completion or hover is missing          | Run `PhpThunder: Reindex Workspace`                                                  |
| Wrong diagnostics for language features | Run `PhpThunder: Select PHP Version` and choose the project level                  |
| Vendor symbols are missing              | Run `composer install`, then `PhpThunder: Reindex Workspace`                         |
| Tests do not appear in Test Explorer    | Confirm `vendor/bin/phpunit` or `vendor/bin/pest` exists, then reindex the project |

If the quick wins do not help, use the more specific sections below.

## Stale index or missing symbols

Completion, hover, references, and renamed files can lag behind after a structural change.

1. Save the affected files.
2. Run `PhpThunder: Reindex Workspace`.
3. Wait for the scan to finish; the status bar shows progress.

This is especially important after large refactors, Composer operations, or include-path changes. Most single-file edits are picked up automatically; reindex is for changes that affect the whole project graph.

## Reset extension state

Use this only when PhpThunder still behaves incorrectly after reindexing and normal configuration checks.

1. Run `PhpThunder: Reset Extension`.
2. Confirm the warning.
3. Let VS Code reload the window.

Reset clears PhpThunder local state on this machine:

- global storage and caches
- local license and trial data plus the installation identity
- interpreter catalog and Composer configuration
- all `phpThunder.*` settings at global, workspace, and workspace-folder scope
- saved PhpThunder view state such as TODO filters and grouping

Reset does not clear:

- project files
- Composer dependencies in the workspace
- non-PhpThunder VS Code settings

If a paid license is active, PhpThunder first tries to deactivate this device before wiping local state. If that deactivation fails and you still continue, you may need to deactivate the old device manually from the license dashboard before reactivating.

## Vendor classes or Composer symbols are missing

When third-party classes show as undefined, work through this checklist:

- Run `composer install` and confirm `vendor/` is populated.
- Open the project root, meaning the folder that contains `composer.json`.
- Check `phpThunder.includePaths` for any non-standard source directories the project depends on.
- Run `PhpThunder: Composer Dump Autoload` or `PhpThunder: Reindex Workspace` after autoload changes.

> **Note:** `phpThunder.diagnostics.enableVendor` is off by default, so vendor files intentionally do not behave like first-party code unless that setting is enabled.

## Wrong PHP version or binary

### Wrong PHP version used for analysis

False-positive diagnostics about features or syntax usually mean the selected language level is too low or too high.

- Run `PhpThunder: Select PHP Version`.
- Choose the version the project actually targets.

### Wrong PHP binary selected

Debugging, tests, or Composer commands can use the wrong interpreter when the workspace is pointed at the wrong catalog entry.

- Run `PhpThunder: Configure PHP Interpreters`.
- Confirm the expected PHP binary exists in the catalog.
- Confirm the intended interpreter is selected for the current workspace.
- Confirm the selected interpreter can actually execute project commands.

## Include-path changes do not take effect

After changing `phpThunder.includePaths`, the indexing graph needs a refresh.

- Run `PhpThunder: Reindex Workspace`, or
- Reload the VS Code window with `Developer: Reload Window`

## Debugging does not start or breakpoints do not bind

If `F5` fails, breakpoints stay unverified, or execution runs straight through, check the basics first:

- Xdebug is installed and loaded for the selected PHP binary (`php -m | grep xdebug`).
- The debug port in `launch.json` matches the Xdebug configuration, usually `9003`.
- `generateIni` is enabled if PhpThunder should generate a helper ini file automatically.
- For Docker or remote paths, `pathMappings` keys are the remote paths and values are the local paths.
- The launch mode (`cli`, `web`, or `attach`) matches the actual workflow.

Start with the simplest possible configuration and add server overrides or path mappings only after the basic setup works.

## Profiling does not produce a capture

When no cachegrind file appears under `.phpthunder/profiles/`, check:

- Pro access or an active trial is available.
- Xdebug is loaded for the selected interpreter.
- `phpThunder.profiling.docRoot` exists in the workspace for web profiling.
- No other process is using the configured web port.
- The `PHP Profiler` output channel shows no startup errors.

## Tests are not discovered

When the Test Explorer stays empty or only shows part of the suite, verify:

- The project contains PHPUnit or Pest test files that follow the expected naming convention.
- `vendor/bin/phpunit` or `vendor/bin/pest` exists.
- `phpThunder.testRunner` matches the project's runner, or is set to `auto`.
- The workspace root is the project root, not a nested source folder.
- The configured PHP interpreter can execute the test binary.

## Activation, trial, and license state

PhpThunder offers a free tier, a 30-day Pro trial, and ongoing Pro access. License and trial flows start from `PhpThunder: Activate License`.

From the activation page, the available flows are:

- sign in with a Sailantis account to fetch and activate an existing license
- enter a license key directly when one is already available

The status bar entry shows the current access level:

| Status bar shows   | What it means                                                       |
| ------------------ | ------------------------------------------------------------------- |
| `PHP Free`         | Base features only; no trial active                                 |
| `PHP Trial`        | Full Pro access for the trial period                                |
| `PHP Pro`          | Active Pro license                                                  |
| `PHP Grace Period` | License recently expired; Pro features remain available temporarily |

If a Pro license lapses, PhpThunder enters Grace Period instead of blocking Pro features immediately. Reactivate or renew from `PhpThunder: Activate License`.

Plan limits still apply to seats and machine count. For current pricing and plan details, see the [Sailantis website](https://sailantis.com).

## Compatibility

PhpThunder supports:

- PHP 5.6 through 8.5
- Windows, macOS, and Linux
- VS Code 1.82.0 or later
- WSL via the Remote - WSL extension
- Remote containers and remote SSH sessions
- Multi-root workspaces with per-folder PHP settings

## Reporting a bug

The LSP trace log is the most useful artifact when a bug report needs more detail.

1. Set `phpThunder.trace.server` to `messages` or `verbose` in the workspace settings.
2. Reproduce the problem.
3. Open the `PhpThunder Language Server` output channel.
4. Copy the relevant portion of the log.
5. Reset `phpThunder.trace.server` back to `off` when finished.

Useful context for a bug report:

- VS Code version
- PhpThunder version
- PHP version and selected interpreter name
- Whether the project uses Composer, Pest, PHPUnit, Xdebug, or custom include paths
- The minimal code or project layout that reproduces the issue

## Related guides

- [Installation and activation](02-installation-and-activation.md) — setup, interpreter selection, and license activation
- [Project configuration](07-project-configuration.md) — settings reference
- [Debugging](04-debugging.md) — Xdebug setup in detail
- [Profiling](05-profiling.md) — profiling requirements and settings
- [Testing](06-testing.md) — test discovery and runner setup
