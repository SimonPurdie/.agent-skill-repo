---
name: powershell
description: Write reliable PowerShell commands and scripts with guardrails for common agent failure modes (quoting, paths, object pipelines, native command invocation, exit semantics, and module/version drift). Use when the user asks for PowerShell snippets, .ps1 scripts, debugging, refactoring, or linting with PSScriptAnalyzer.
---

# PowerShell Commands and Scripts

This skill is scoped to PowerShell 7.

Use this workflow when authoring commands, one-liners, or script files.

## Workflow

1. Confirm intent, inputs, and side effects (read-only vs mutating).
2. Prefer cmdlets and object pipelines over shell-style text pipelines.
3. Use named parameters and splatting for anything non-trivial.
4. Validate command/module availability before relying on it.
5. Lint with PSScriptAnalyzer before finalizing. If PSScriptAnalyzer is not available, pause and explicitly ask the user whether to install it or waive linting.

## Common Failure Modes Checklist

- Shell mismatch:
  - Do not use Bash syntax (`&&`, `||`, `$VAR`, `export`, `grep | awk`) in PowerShell code.
  - Use PowerShell equivalents (`; if (...) {}`, `$env:VAR`, cmdlets, object pipeline).
  - In `npm` scripts, `powershell` invokes Windows PowerShell 5.1. Use `pwsh -NoLogo -NoProfile ...` when PS7 behavior is required.
- Quoting and interpolation:
  - Single quotes are literal; double quotes interpolate.
  - Prefer single quotes for fixed strings.
  - Use `${name}` when interpolation touches adjacent characters.
  - When punctuation follows a variable in a double-quoted string (especially `:`), prefer `${name}` or `-f` formatting.
- Path handling:
  - Prefer `-LiteralPath` for user-provided paths.
  - Use `Join-Path` and avoid manual separator stitching.
  - Quote paths with spaces.
  - `-LiteralPath` does not expand wildcards (`*`, `?`); use `-Path` when wildcard expansion is intentional.
- Native command invocation:
  - Do not build one giant command string.
  - Use argument arrays and call operator `&`.
  - Example:
    ```powershell
    $args = @('--flag', $value, '--out', $outPath)
    & some-native-tool @args
    $code = $LASTEXITCODE
    ```
- Exit semantics:
  - `$?` is success of last PowerShell pipeline step.
  - `$LASTEXITCODE` is for native executables; check it after native calls.
- Error behavior:
  - Many cmdlets emit non-terminating errors by default.
  - Use `-ErrorAction Stop` surgically on operations where failure must halt logic.
- Pipeline type confusion:
  - Pipeline passes objects, not plain text, between cmdlets.
  - Use `Select-Object`, `Where-Object`, and `ForEach-Object` instead of text parsing.
- Backtick misuse:
  - Avoid backtick line continuation where possible; prefer splatting or parentheses.
  - Backticks in trailing-whitespace contexts are fragile.
- Module/version drift:
  - Check availability with `Get-Command <Name>` and `Get-Module -ListAvailable`.
  - Provide fallbacks when cmdlets differ between environments.
- Encoding surprises:
  - `Set-Content -Encoding utf8NoBOM` is PowerShell 7+ only.
  - For cross-version UTF-8 (no BOM), prefer:
    ```powershell
    $utf8NoBom = New-Object System.Text.UTF8Encoding($false)
    [System.IO.File]::WriteAllText($path, $text, $utf8NoBom)
    ```
- Logging robustness:
  - Prefer stable templates (`'Step {0}: {1}' -f $step,$msg`) over fragile inline interpolation in complex strings.

## Cross-Version and Invocation Guardrails

- Detect runtime before using version-specific features:
  ```powershell
  $psVersion = $PSVersionTable.PSVersion
  $psEdition = $PSVersionTable.PSEdition
  ```
- Prefer script files (`-File`) over heavily escaped inline `-Command` strings.
- When `-Command` is required, minimize nested quoting and test from the same host shell that will execute it.

## Optional Strictness Toggles (Off by Default)

These improve correctness but can make exploratory snippets brittle. Enable only when requested or when hard-fail behavior is required.

```powershell
Set-StrictMode -Version Latest
$ErrorActionPreference = 'Stop'
```

Pragmatic default: keep both off, and use targeted `-ErrorAction Stop` on critical operations.

## PSScriptAnalyzer Workflow

Source of truth for analyzer behavior and rules is the GitHub repository:

- https://github.com/PowerShell/PSScriptAnalyzer
- https://github.com/PowerShell/PSScriptAnalyzer/blob/master/docs/Cmdlets/Invoke-ScriptAnalyzer.md
- https://github.com/PowerShell/PSScriptAnalyzer/blob/master/docs/Cmdlets/Get-ScriptAnalyzerRule.md
- https://github.com/PowerShell/PSScriptAnalyzer/blob/master/docs/Cmdlets/Invoke-Formatter.md
- https://github.com/PowerShell/PSScriptAnalyzer/blob/master/docs/Rules/README.md

## Delivery Standard

Before finalizing PowerShell output:

1. Re-check the failure-mode checklist.
2. Ensure paths, quoting, and native-exit handling are explicit.
3. Run analyzer and resolve or explicitly justify suppressions.
