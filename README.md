# {{PACKAGE_NAME}}

[![CI](https://github.com/alya-lang/{{PACKAGE_NAME}}/actions/workflows/ci.yml/badge.svg)](https://github.com/alya-lang/{{PACKAGE_NAME}}/actions/workflows/ci.yml)
[![License](https://img.shields.io/github/license/alya-lang/{{PACKAGE_NAME}}?color=blue&label=License)](LICENSE)
[![Alya](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2F{{PACKAGE_NAME}}%2Fmain%2Falya.toml&query=%24.package.alya-version&label=Alya&color=orange&prefix=%3E%3D)](https://github.com/alya-lang/alya)
[![Package Version](https://img.shields.io/badge/dynamic/toml?url=https%3A%2F%2Fraw.githubusercontent.com%2Falya-lang%2F{{PACKAGE_NAME}}%2Fmain%2Falya.toml&query=%24.package.version&label=Version&color=brightgreen)](alya.toml)

{{DESCRIPTION}}

---

## 🌟 Features

- ⚡ **Lightweight & High Performance**: Minimal memory overhead, zero runtime bloat, and fast native execution
- 🧩 **Modular Architecture**: Layered multi-module design featuring a clean public facade (`src/lib.alya`), rich data models (`src/types.alya`), a processing engine (`src/core/pipeline.alya`), a structured error model (`src/core/errors.alya`), and encapsulated core formatters (`src/core/formatter.alya`)
- 🔒 **Public/Private Visibility (`pub`)**: Fine-grained export control with `pub` for public functions, structs, and enums, keeping internal helper functions private and encapsulated
- 🎭 **Structural Duck Typing & Interfaces**: Dynamic interface dispatch (`Summarizable`, `Describable`) without brittle inheritance hierarchies
- 📦 **Rich Domain Models & Enums**: Idiomatic `enum` types (`TemplateStatus`, `TemplatePriority`, `TemplateStyle`) and typed data containers (`TemplateConfig`, `TemplateResult`, `TemplateStats`, `TemplateError`)
- 🎯 **Advanced Pattern Matching**: Clean branching with `when` expressions, range matching, and condition guards
- 🛡️ **Defensive Result Pattern**: Structured error handling with `throw` / `try` / `catch err` / `finally`, `when`-based error dispatch, and outcome encapsulation with `ok_result` and `error_result`
- 🧪 **Enterprise Test & Benchmark Suite**: 100% test coverage with standard assertions (`std/test`) and micro-benchmarking (`std/test` bench runner)
- 🚩 **Feature-Gated API Slices**: Optional capability slices via `[features]` in `alya.toml` (`default = ["extras"]`) and `@cfg(feature = "extras")` gating with a `@cfg(not(feature = "extras"))` fallback stub (see `src/core/extras.alya`)
- 🧬 **Modern Type System Showcase**: Nullable types (`string?`), null coalescing (`??`), generic helpers (`pipeline_first[T]`, `Stack[T]`), `defer` cleanup, indexed loops (`for i, x in arr`), and `bool` predicates
- 🔧 **Native C Engine Slice**: Bundled zero-dependency FFI (`c/`, `src/ffi.alya`, `[build]`) behind the `native` feature with fallback stub
- 🌀 **Fibers & Channels Walkthrough**: Deterministic `spawn` + `std/sync` channel pipeline in the demo (argument-passing form)

---

## 📁 Project Architecture

```
{{PACKAGE_NAME}}/
├── .alyalint               # Linter configuration (rules, exclusions, severity overrides)
├── .editorconfig           # Uniform formatting rules across IDEs and editors
├── .gitignore              # Ecosystem standard ignore filters
├── .vscode/                # VS Code workspace settings, DAP launch configurations & tasks
├── alya.toml               # Package manifest with dependencies, [features] and [build]
├── c/                      # Bundled native C engine (`native` feature; delete with ffi.alya+[build] for pure-Alya packages)
├── src/
│   ├── lib.alya            # Public API facade (thin pub wrappers delegating to core/* engines)
│   ├── types.alya          # Data models, pub enums, pub structs (incl. generic Stack[T]), methods & pub consts
│   ├── ffi.alya            # Native extern "C" declarations (`native` feature)
│   └── core/               # Subdirectory module hierarchy (grow into <domain>/ for complex packages)
│       ├── formatter.alya  # Domain formatting routines, salutation builders & pattern matchers
│       ├── collect.alya    # Collection helpers: slicing, membership, generics & closures
│       ├── pipeline.alya   # Processing engine: pipeline, strict/try variants, batching, generics
│       ├── errors.alya     # Structured TemplateError model, validation & catch-site narrowing guide
│       └── extras.alya     # Feature-gated (`extras`) optional API slice with `@cfg` gating
├── examples/
│   └── demo.alya           # Comprehensive runnable walkthrough of all package capabilities
├── tests/
│   ├── test_basic.alya     # Core automated test suite (runner API + one native test block)
│   ├── test_strict.alya    # Per-feature suite (`strict` slice; dual main: suite + skip stub)
│   └── test_native.alya    # Per-feature suite (`native` slice; dual main: suite + skip stub)
└── benches/
    └── bench_basic.alya    # Micro-benchmarks measuring performance and throughput
```

> [!NOTE]
> **Visibility & Modularity:** Symbols annotated with `pub` (`pub function`, `pub struct`, `pub enum`, `pub interface`) are exported to external consumers and re-exporting modules. Symbols without `pub` remain strictly internal to their declaring module, preventing symbol collisions and implementation leakage.

---

## 📦 Installation

Add `{{PACKAGE_NAME}}` to the `[dependencies]` section in your `alya.toml`:

```toml
[dependencies]
{{PACKAGE_NAME}} = { git = "https://github.com/alya-lang/{{PACKAGE_NAME}}", branch = "main" }
```

Or install it directly using the Alya package CLI:

```bash
alya add {{PACKAGE_NAME}} --git https://github.com/alya-lang/{{PACKAGE_NAME}} --branch main
alya install
```

### Package Features

| Feature | Default | Description |
|:---|:---:|:---|
| `extras` | ✅ | Optional capability slice (`extra_greeting` in `src/core/extras.alya`, with a fallback stub when disabled). |
| `strict` | ✅ | Structured-error recovery slice (`TemplateError`, `check_label` in `src/core/errors.alya` plus `process_strict`/`try_process` in `src/core/pipeline.alya`, with fallback stubs when disabled; per-feature tests live in `tests/test_strict.alya`). |
| `native` | ✅ | Bundled C engine slice (`c_add` via `src/ffi.alya`, compiled from `c/` per `[build]`, with a fallback stub when disabled; per-feature tests live in `tests/test_native.alya`). |

```bash
# Full build (default)
alya install
alya test

# Slim build (core only)
alya install --no-default-features
alya test --no-default-features
```

---

## 🚀 Quick Start

```alya
import "{{PACKAGE_NAME}}" as pkg

function main()
    # 1. Basic facade call with default parameter
    let greeting = pkg::hello()
    say f"Greeting:  {greeting}"

    # 2. Struct configuration with priority, style, and methods
    let cfg = pkg::new_config("Community", 5, pkg::TemplatePriority.High, pkg::TemplateStyle.Formal)
    say f"Summary:   {cfg.summary()}"
    say f"Formatted: {pkg::core_format_custom(cfg)}"

    # 3. Processing pipeline returning Result model
    let res = pkg::process("Analytics", 3, pkg::TemplatePriority.Critical)
    say f"Outcome:   {res.message}"

    # 4. Strict pipeline with structured error recovery
    let safe = pkg::try_process("")
    say f"Recovered: {safe.is_error()}"
end

main()
```

---

## 📖 API Reference

| Symbol | Visibility | Description |
|---|---|---|
| `hello(name = "World")` | `pub function` | Returns a formatted greeting string. Defaults to `"World"` if null or empty. |
| `new_config(name, count, priority, style)` | `pub function` | Factory constructing a `TemplateConfig` with sensible defaults. |
| `make_config(name, count, priority, style, enabled, tags)` | `pub function` | Full constructor for `TemplateConfig`. |
| `process(label, count, priority)` | `pub function` | Runs processing pipeline, returning an `ok_result` `TemplateResult`. |
| `process_batch(labels)` | `pub function` | Formats an array of labels in batch, returning an array of strings. |
| `process_strict(label, count, priority)` | `pub function` (`strict` feature, default-on) | Runs strict pipeline, throwing `TemplateError` on empty input. |
| `try_process(label, count, priority)` | `pub function` (`strict` feature, default-on) | Runs strict pipeline, converting errors into failed results. |
| `pipeline_process(label, count, priority)` | `pub function` | Engine behind `process` (`src/core/pipeline.alya`). |
| `pipeline_strict_process(label, count, priority)` | `pub function` (`strict` feature, default-on) | Engine behind `process_strict` (throws `TemplateError`). |
| `try_pipeline_process(label, count, priority)` | `pub function` (`strict` feature, default-on) | Engine behind `try_process` (`try` / `catch` recovery). |
| `pipeline_process_batch(labels)` | `pub function` | Engine behind `process_batch` (indexed `for i, lbl` loop). |
| `pipeline_first(items)` | `pub function` | Generic `function pipeline_first[T]` returning first element or null. |
| `make_template_error(field, message)` | `pub function` (`strict` feature, default-on) | Constructs a `TemplateError` for invalid input. |
| `check_label(label)` | `pub function` (`strict` feature, default-on) | Validates a label, throwing `TemplateError` on empty input. |
| `ok_result(value, message)` | `pub function` | Constructs a successful `TemplateResult` container (`status = 0`). |
| `error_result(message, errors)` | `pub function` | Constructs a failed `TemplateResult` container (`status = 1`). |
| `make_stats(total, passed, failed, skipped)` | `pub function` | Constructs a `TemplateStats` metrics record. |
| `format_summary(cfg)` | `pub function` | Formats summary of a config instance (satisfies `Summarizable`). |
| `format_description(cfg)` | `pub function` | Formats description of a config instance (satisfies `Describable`). |
| `format_config(config)` | `pub function` | Multi-field formatter producing descriptive overview of a `TemplateConfig`. |
| `format_result(result)` | `pub function` | Formats a `TemplateResult` into `[OK]` or `[ERROR]` status line. |
| `format_stats(stats)` | `pub function` | Formats total checked items and success rate percentage. |
| `clamp(n, min_val, max_val)` | `pub function` | Clamps an integer value to the closed range `[min_val, max_val]`. |
| `pluralize(n, singular, plural)` | `pub function` | Pattern-matches count to return singular or plural noun form. |
| `repeat_string(label, count)` | `pub function` | Repeats a string into an array of `count` items. |
| `c_add(a, b)` | `pub function` (`native` feature, default-on) | Adds two integers via the bundled native C engine. |
| `collect_take(items, n)` | `pub function` | Returns the first `n` elements (slicing). |
| `collect_drop(items, n)` | `pub function` | Drops the first `n` elements (slicing). |
| `collect_contains(items, v)` | `pub function` | Reports membership via `in`. |
| `collect_map_each(items, f)` | `pub function` | Generic `[T, U]` elementwise closure map. |
| `combine_stats(a, b)` | `pub function` | Fieldwise sum of two `TemplateStats` records. |
| `extra_greeting(name = "World")` | `pub function` (`extras` feature, default-on) | Enthusiastic greeting slice gated by `@cfg(feature = "extras")`; stub throws a descriptive error when the feature is off. |
| `Summarizable` | `pub interface` | Structural contract requiring `summary(self) -> string`. |
| `Describable` | `pub interface` | Structural contract requiring `describe(self) -> string` and `is_valid(self) -> bool`. |
| `TemplateStatus` | `pub enum` | Lifecycle status codes (`Pending = 0`, `Active = 1`, `Archived = 2`, `Error = 3`). |
| `TemplatePriority` | `pub enum` | Priority tiers (`Low = 0`, `Normal = 1`, `High = 2`, `Critical = 3`). |
| `TemplateStyle` | `pub enum` | Presentation styles (`Standard = 0`, `Formal = 1`, `Casual = 2`). |
| `TemplateConfig` | `pub struct` | Primary configuration model (`name`, `count`, `priority`, `style`, `enabled`, `tags`). |
| `TemplateConfig.summary()` | `pub method` | Single-line formatted summary (satisfies `Summarizable`). |
| `TemplateConfig.describe()` | `pub method` | Detailed multi-field description (satisfies `Describable`). |
| `TemplateConfig.is_valid()` | `pub method` | Validation guard returning true if valid, false otherwise. |
| `TemplateConfig.is_enabled()` | `pub method` | Returns true if active, false if disabled. |
| `TemplateConfig.with_name(new_name)` | `pub method` | Immutable copy with updated name. |
| `TemplateConfig.with_priority(new_prio)` | `pub method` | Immutable copy with updated priority tier. |
| `TemplateResult` | `pub struct` | Operation outcome model (`value`, `status`, `message`, `errors`). |
| `TemplateResult.is_ok()` | `pub method` | Returns true if successful (`status == 0`), false otherwise. |
| `TemplateResult.is_error()` | `pub method` | Returns true if error (`status != 0`), false otherwise. |
| `TemplateResult.unwrap_or(fallback)` | `pub method` | Returns message on success, or fallback on error. |
| `TemplateStats` | `pub struct` | Run statistics model (`total`, `passed`, `failed`, `skipped`). |
| `TemplateStats.total_checked()` | `pub method` | Sum of passed and failed items count. |
| `TemplateStats.success_rate()` | `pub method` | Computed percentage string (e.g. `"95%"`). |
| `TemplateError` | `pub struct` (`strict` feature, default-on) | Structured validation error (`field`, `message`); narrow with `when err is TemplateError` at the catch site. |
| `Stack[T]` | `pub struct` | Generic LIFO container (`items: T[]`). |
| `Stack[T].create()` | `pub function` | Empty-stack constructor. |
| `Stack[T].push(item)` | `pub method` | Pushes an item. |
| `Stack[T].pop()` | `pub method` | Pops and returns the head item. |
| `Stack[T].peek()` | `pub method` | Returns the head item or null without removing. |
| `Stack[T].size()` | `pub method` | Returns the item count. |
| `DEFAULT_COUNT` | `pub const` | Default repetition limit applied by factory constructors. |

> [!TIP]
> **Internal Helpers & Documentation:** Public symbols are documented with `##` Markdown docstrings, enabling automatic API documentation generation via `alya doc`. Private functions such as `build_salutation` and `build_priority_label` in `src/core/formatter.alya` are not annotated with `pub` and remain encapsulated within their respective modules.

---

## 🧪 Running Tests & Benchmarks

Run the automated test suite using `alya test`:

```bash
alya test
```

Exercise feature selection (the `extras` and `strict` slices are default-on):

```bash
alya test --features extras
alya test --features strict
alya test --no-default-features
```

### 🧩 Splitting Test Suites (Complex Packages)

When the suite outgrows one file, split it per area or feature slice
(`tests/test_basic.alya`, `tests/test_strict.alya`, ...). Each
feature-gated file carries two `main()` definitions: the real suite under
`@cfg(feature = "...")` and a skip stub under `@cfg(not(feature = "..."))`,
so every feature combination stays green:

```alya
@cfg(feature = "strict")
function main()
    test_suite("Strict Recovery Tests")
    let r = runner_new()
    # ... gated assertions ...
    let failed = runner_summary(r)
    if failed > 0
        exit(1)
    end
end

@cfg(not(feature = "strict"))
function main()
    test_suite("Strict Recovery Tests (skipped without strict feature)")
    let r = runner_new()
    let failed = runner_summary(r)
    if failed > 0
        exit(1)
    end
end

main()
```

Generate static API documentation:

```bash
alya doc . -o docs --markdown
```

Run the benchmark suite:

```bash
alya run benches/bench_basic.alya
```

Run the example demo:

```bash
alya run examples/demo.alya
```

Check code formatting:

```bash
alya fmt . --check
```

Run static code linter:

```bash
alya lint . --check
```

---

### 💻 Developer Tooling & VS Code Integration

This package comes preconfigured with recommended workspace settings and tasks for **Visual Studio Code**:
- **LSP & Formatting**: Auto-formatting on save and real-time Language Server diagnostics via `alya-lang.vscode-alya`.
- **DAP Debugging**: Launch configurations in `.vscode/launch.json` ready for interactive step-debugging via `F5`.
- **Predefined Tasks**: Press `Ctrl+Shift+B` or run tasks (`Test`, `Lint`, `Format`, `Build Docs`) directly from the Command Palette.

---

## 🤝 Contributing

Contributions are welcome! Please follow these steps:

1. Fork the repository and clone it locally
2. Install dependencies:
   ```bash
   alya install
   ```
3. Create your feature branch (`git checkout -b feature/my-feature`)
4. Verify tests and formatting before opening a PR:
   ```bash
   alya test
   ```
5. Commit your changes (`git commit -m "feat: add feature"`) and open a Pull Request

---

## 📄 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.