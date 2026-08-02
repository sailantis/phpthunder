# Project configuration

This page is the complete settings reference for PhpThunder. Each section explains not just what a setting does but also when you'd actually want to change it.

For a quick orientation, the settings fall into five areas:

1. [Intelligence](#intelligence) — PHP version, interpreter, include paths
2. [Formatting](#formatting) — style rules for arrays, parameters, control structures
3. [Diagnostics & code fixes](#diagnostics--code-fixes) — what PhpThunder analyzes and flags
4. [Testing](#testing) — runner selection
5. [Debugging & profiling](#debugging--profiling) — Xdebug port, profiling server, tracing

---

## Intelligence

These settings drive code analysis: what PHP version to target, which PHP binary to use, and which source directories to scan.

### PHP version and interpreter

| Setting                        | What it does                                                                                                |
| ------------------------------ | ----------------------------------------------------------------------------------------------------------- |
| `phpThunder.phpVersion`        | Sets the PHP language level for the current folder. Affects diagnostics, feature checks, and type analysis. |
| `phpThunder.activeInterpreter` | Selects the active interpreter by name from the PhpThunder catalog.                                         |
| `phpThunder.interpreters`      | Machine-scoped catalog of PHP binaries. Managed by the PhpThunder settings UI — don't edit this by hand.    |
| `phpThunder.shortOpenTag`      | Allow PHP short open tags (`<?`) in addition to `<?php` and `<?=`. Matches `short_open_tag` in `php.ini`. Disabled by default. |
| `phpThunder.server.memoryLimit` | Maximum memory the language server may use (e.g. `70%`, `4gb`, `2048m`). Default: `70%` of total RAM. Requires a window reload to take effect. |

**When to change:** Set `phpThunder.phpVersion` as soon as you open a project. Getting this right prevents false-positive diagnostics about language features. Use `phpThunder.activeInterpreter` to pin a workspace to a specific binary when your machine has multiple PHP versions.

Useful commands:

- `PhpThunder: Select PHP Version`
- `PhpThunder: Configure PHP Interpreters`

### Composer and include paths

| Setting                           | What it does                                                                                                                                            |
| --------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `phpThunder.composer.mode`        | How PhpThunder finds Composer: `auto`, `projectPhar`, or `custom`.                                                                                      |
| `phpThunder.composer.projectPhar` | Path to a project-local `composer.phar`, relative to the workspace root.                                                                                |
| `phpThunder.includePaths`         | Extra PHP directories to index beyond what Composer provides. Paths from the global, workspace, and matching folder scopes are merged for each project. |

**When to change:** Most projects work fine with `auto`. Set `projectPhar` when the project ships its own `composer.phar` under a `tools/` or `build/` folder. Use `includePaths` for legacy codebases, internal SDKs, or monorepo packages that aren't registered in `composer.json`.

> **Important:** After changing `includePaths`, run `PhpThunder: Reindex Workspace` or reload the window. Include-path changes are not picked up automatically.

Useful commands:

- `PhpThunder: Configure Composer`
- `PhpThunder: Configure Include Paths`
- `PhpThunder: Reindex Workspace`

### Completion behavior

| Setting                                     | Default | What it does                                                                                                                                                                   |
| ------------------------------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `phpThunder.completion.autoTriggerFallback` | `true`  | When VS Code's `editor.quickSuggestions` settings suppress completions for plain text, PhpThunder can still auto-open suggestions for PHP variables and namespace identifiers. |

**When to change:** Rarely. Leave this at `true` unless you're seeing unwanted suggestion popups.

---

## Formatting

PhpThunder formats PHP files as PSR-12 by default. Standard VS Code editor settings and `.editorconfig` files are honored in all tiers — no license required.

The `phpThunder.formatting.*` customization settings below require a Pro license or an active trial. Most style options default to `preserve`, so they don't change existing style unless explicitly configured.

### Layout settings

These accept `preserve`, `auto`, `force-single-line`, or `force-multi-line`:

| Setting                            | Controls                              |
| ---------------------------------- | ------------------------------------- |
| `phpThunder.formatting.arrays`     | Array literal layout                  |
| `phpThunder.formatting.parameters` | Function/method parameter list layout |
| `phpThunder.formatting.arguments`  | Call-site argument list layout        |
| `phpThunder.formatting.matchArms`  | `match` expression arm layout         |

- **`preserve`** — leave the existing style alone
- **`auto`** — break to multi-line when the line exceeds `maxInlineWidth`, keep single-line otherwise
- **`force-single-line`** — always collapse to one line
- **`force-multi-line`** — always expand to multiple lines

### Syntax style settings

These accept `preserve`, `force-braces`, or `force-alternative`:

| Setting                               | Controls                   |
| ------------------------------------- | -------------------------- |
| `phpThunder.formatting.ifSyntax`      | `if`/`elseif`/`else` style |
| `phpThunder.formatting.foreachSyntax` | `foreach` style            |
| `phpThunder.formatting.forSyntax`     | `for` style                |
| `phpThunder.formatting.whileSyntax`   | `while` style              |
| `phpThunder.formatting.switchSyntax`  | `switch` style             |

- **`preserve`** — keep whichever style is already used
- **`force-braces`** — always use curly-brace blocks
- **`force-alternative`** — always use `if (): ... endif;` alternative syntax (common in view templates)

### Width control

| Setting                                | Default | What it does                                                                               |
| -------------------------------------- | ------- | ------------------------------------------------------------------------------------------ |
| `phpThunder.formatting.maxInlineWidth` | `80`    | Line width at which `auto` layout mode breaks a single-line construct into multiple lines. |

**When to change:** Set `maxInlineWidth` to match the project's line-length rule (120, 100, etc.) and use `auto` for arrays and parameters to get consistent automatic breaking.

### Additional formatting settings

#### Spacing

| Setting                                        | Default     | What it controls                                                             |
| ---------------------------------------------- | ----------- | ---------------------------------------------------------------------------- |
| `phpThunder.formatting.spaceAfterCast`         | `always`    | Space after cast operators: `(int)$x` vs `(int) $x`. Options: `never`, `always`, `preserve`. |
| `phpThunder.formatting.shortArraySyntax`       | `preserve`  | Array bracket style: `force-short` = `[]`, `force-long` = `array()`, `preserve` = keep source form. |
| `phpThunder.formatting.spaceBeforeDeclParens`       | `never`   | Space before `(` in function/method declarations: `function foo($x)` vs `function foo ($x)`. Options: `never`, `always`, `preserve`. |
| `phpThunder.formatting.spaceBeforeCallParens`       | `never`   | Space before `(` in function/method calls: `foo($x)` vs `foo ($x)`. Options: `never`, `always`, `preserve`. |
| `phpThunder.formatting.spaceBeforeCFParens` | `always` | Space before `(` in control-flow headers such as `if`, `foreach`, `for`, `while`, and `switch`. Options: `never`, `always`, `preserve`. |
| `phpThunder.formatting.spacesInsideDeclParens` | `never`     | Spaces inside parentheses in function/method *declarations*. Options: `never`, `always`, `preserve`. |
| `phpThunder.formatting.spacesInsideCallParens` | `never`     | Spaces inside parentheses in function/method *call* argument lists. Options: `never`, `always`, `preserve`. |
| `phpThunder.formatting.spacesInsideCFParens` | `never`     | Spaces inside parentheses in control-flow statements. Options: `never`, `always`, `preserve`. |
| `phpThunder.formatting.spaceAfterComma`        | `always`    | Space after commas in inline (single-line) lists. Options: `always`, `never`, `preserve`. |
| `phpThunder.formatting.spaceAfterDoubleSlash`  | `preserve`  | Space after `//` in line comments. Options: `preserve`, `always`, `never`. |
| `phpThunder.formatting.spaceBeforeReturnTypeColon` | `false` | Insert a space before the `:` of a return type hint: `): void` vs `) : void`. |

#### Syntax style

| Setting                                          | Default      | What it controls                                                              |
| ------------------------------------------------ | ------------ | ----------------------------------------------------------------------------- |
| `phpThunder.formatting.trailingCommaMultilineArrays` | `preserve` | Trailing comma in multi-line array literals. Options: `preserve`, `add`, `remove`. |
| `phpThunder.formatting.trailingCommaMultilineArgs`   | `preserve` | Trailing comma in multi-line call argument lists. Options: `preserve`, `add`, `remove`. |
| `phpThunder.formatting.classBracePosition`       | `new-line`   | Opening `{` of a class, trait, or enum. Options: `new-line` (PSR-12), `same-line` (K&R), `preserve`. |
| `phpThunder.formatting.functionBracePosition`    | `new-line`   | Opening `{` of a function or method. Options: `new-line` (PSR-12), `same-line`, `preserve`. |
| `phpThunder.formatting.namespaceBracePosition`    | `new-line`   | Opening `{` of a bracketed namespace. Options: `new-line`, `same-line`, `preserve`. |
| `phpThunder.formatting.controlFlowBracePosition` | `same-line`  | Opening `{` of control flow statements (`if`, `else`, `for`, `foreach`, `while`, `do`, `switch`). Options: `new-line`, `same-line`, `preserve`. |
| `phpThunder.formatting.tryCatchBracePosition`    | `same-line`  | Opening `{` of `try`/`catch`/`finally` blocks. Options: `new-line`, `same-line`, `preserve`. |
| `phpThunder.formatting.catchFinallyOnNewLine`   | `false`      | Place `catch`/`finally` on a new line instead of following the closing `}`. |
| `phpThunder.formatting.indentSwitchCase`         | `true`       | Indent `case`/`default` labels inside `switch` blocks (PSR-12 style). `false` aligns labels with `switch`. |
| `phpThunder.formatting.elseOnNewLine`            | `false`      | Place `else`/`elseif` on a new line instead of following the closing brace. |
| `phpThunder.formatting.returnTypePosition`       | `same-line`  | Return type hint placement. Options: `same-line`, `new-line`. |
| `phpThunder.formatting.emptyBodyPosition`        | `new-line`   | How to render empty function/method/closure bodies. `new-line` = `{\\n}` (PSR-12); `same-line` = `{}`; `preserve` = keep source. |
| `phpThunder.formatting.maxConsecutiveBlankLines` | `1`          | Max consecutive blank lines between statements. Options: `preserve`, `0`, `1`, `2`, `3`. |

#### Alignment

| Setting                                            | Default | What it controls                                                      |
| -------------------------------------------------- | ------- | --------------------------------------------------------------------- |
| `phpThunder.formatting.alignConsecutiveInlineComments` | `true` | Align consecutive `//` comments to the same column.                  |
| `phpThunder.formatting.alignVariableAssignments`   | `true`  | Align consecutive `=` in variable assignments and constants.          |
| `phpThunder.formatting.alignArrayArrows`           | `true`  | Align consecutive `=>` inside array literals.                         |
| `phpThunder.formatting.alignMatchArrows`           | `true`  | Align consecutive `=>` inside `match` expressions.                    |
| `phpThunder.formatting.alignPropertyHooks`         | `true`  | Align consecutive `=>` inside PHP 8.4 property hook blocks.           |
| `phpThunder.formatting.alignCSSDeclarations`      | `true`  | Align consecutive CSS declarations (`property: value;`) in `<style>` blocks. |
| `phpThunder.formatting.alignJSObjectProperties`    | `true`  | Align consecutive `:` in JS object literals inside `<script>` blocks.  |
| `phpThunder.formatting.alignJSVariableAssignments` | `true`  | Align consecutive `=` in JS variable assignments inside `<script>` blocks. |
| `phpThunder.formatting.htmlNonIndentTags`          | `[thead, tbody, tfoot, caption, colgroup]` | HTML tags whose children stay at the same indentation depth (no extra nesting). |

---

## Diagnostics & code fixes

| Setting                                          | Default | What it does                                                                 |
| ------------------------------------------------ | ------- | ---------------------------------------------------------------------------- |
| `phpThunder.diagnostics.enableVendor`             | `false` | Run diagnostics on files under `vendor/`.                                    |
| `phpThunder.diagnostics.enableVendorForOpenFiles` | `true`  | Show diagnostics for vendor files open in the editor, even when `enableVendor` is `false`. |
| `phpThunder.diagnostics.level`                    | `max`   | Analysis strictness level (`0`–`9` or `max`). Higher levels enable more checks. |
| `phpThunder.diagnostics.disabledRules`            | `[]`    | List of diagnostic rule IDs to disable entirely (e.g. `["UV001", "CF001"]`). |
| `phpThunder.diagnostics.ruleSeverities`          | `{}`    | Map of rule IDs to custom severities (e.g. `{"UV001": "hint", "CF001": "error"}`). |
| `phpThunder.codeFixes.enabled`                   | `true`  | Enable code-fix hints and quick actions.                                      |
| `phpThunder.codeFixes.disabledFixes`             | `[]`    | Disable specific fix IDs. Use the catalog below for the current IDs.          |
| `phpThunder.diagnosticsSummary.enabled`           | `true`  | Show per-file diagnostics summary indicators.                                 |
| `phpThunder.diagnosticsSummary.inlineSummary`     | `true`  | Show a clickable CodeLens summary at the top of each file.                   |

**When to change:** Enable `diagnostics.enableVendor` only when you're actually working on a Composer package or need to trace a problem into a dependency — it increases analysis load and noise. Disable specific fix IDs in `disabledFixes` when a rule doesn't suit the project.

### Code fix IDs

`phpThunder.codeFixes.disabledFixes` accepts the stable code-fix IDs listed below.

| ID       | What it disables                                                         |
| -------- | ------------------------------------------------------------------------ |
| `CF001`  | Short array syntax (`array()` -> `[]`)                                   |
| `CF003`  | `is_null($x)` -> `$x === null`                                           |
| `CF004`  | Loose null comparison (`== null` -> `=== null`, `!= null` -> `!== null`) |
| `CF005`  | `count($x) > 0` -> `!empty($x)`                                          |
| `CF011`  | Redundant `else` after a `return`                                        |
| `CF012`  | Boolean ternary simplification                                           |
| `CF012b` | Boolean `if`/`else` simplification to `return (bool) ...`                |
| `CF014`  | Inline variable (`return $x;` style simplification)                      |
| `CF020`  | String concatenation to interpolation                                    |
| `CF032`  | Single-return closure to arrow function                                  |

---

## Testing

| Setting                 | Default | What it does                               |
| ----------------------- | ------- | ------------------------------------------ |
| `phpThunder.testRunner` | `auto`  | Test runner: `auto`, `phpunit`, or `pest`. |

**When to change:** `auto` works for most projects. Explicitly set `phpunit` or `pest` if the project has both installed but you only want to use one.

---

## Debugging & profiling

| Setting                                 | Default    | What it does                                                           |
| --------------------------------------- | ---------- | ---------------------------------------------------------------------- |
| `phpThunder.profiling.webPort`          | `8190`     | HTTP port for the local web profiling server (Pro).                    |
| `phpThunder.profiling.docRoot`          | `"public"` | Document root for web profiling, relative to the workspace root (Pro). |
| `phpThunder.debug.inlineValues.enabled` | `true`     | Show variable values inline while a debug session is paused (Pro).     |
| `phpThunder.trace.server`               | `off`      | LSP communication tracing: `off`, `messages`, or `verbose`.            |

**When to change:** Adjust `webPort` and `docRoot` to match the project layout. Use `verbose` tracing only when diagnosing extension behavior — it generates significant log output and should be turned off afterward.

---

## Operational commands

The commands you reach for most often after configuration changes:

- `PhpThunder: Reindex Workspace` — rebuild the project index
- `PhpThunder: Configure PHP Interpreters` — add or switch interpreters
- `PhpThunder: Configure Composer` — update Composer resolution strategy
- `PhpThunder: Configure Include Paths` — add extra source directories
- `PhpThunder: Select PHP Version` — change the language level for the current folder

## Next steps

- [Free vs Pro](08-free-vs-pro.md) — which workflows depend on a Pro license
- [Troubleshooting](09-troubleshooting.md) — if the project still behaves unexpectedly after configuration changes
