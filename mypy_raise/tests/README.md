# Tests

This directory contains the test suite for `mypy-raise`. The tests are written using `unittest` and run `mypy` against Python files located in `tests/resources`.

## Test Structure

- `test_plugin.py`: The main test runner. It executes `mypy` on specific resource files and asserts the output (stdout/stderr).
- `resources/`: Contains Python files used as test cases. Each file demonstrates a specific scenario (e.g., correct usage, violations, specific edge cases).
- `resources/mypy.ini`: The configuration file used by the test suite to enable the plugin.

## Test Cases

| Test Method | Description | Resource File |
| :--- | :--- | :--- |
| `test_safe_function` | Verifies a simple function with `@raising(exceptions=[])` passes. | `safe_function.py` |
| `test_explicit_raise_in_safe_function` | Verifies error when a function declared as safe explicitly raises an exception. | `explicit_raise_in_safe_function.py` |
| `test_call_unsafe_function` | Verifies that calling an undecorated function (unsafe) is ignored by default (unless strict mode). | `call_unsafe_function.py` |
| `test_call_safe_function` | Verifies that calling a verified safe function produces no errors. | `call_safe_function.py` |
| `test_call_explicit_raiser` | Verifies error when calling a function that is known to raise an exception not declared by the caller. | `call_explicit_raiser.py` |
| `test_no_raise_calls_raiser` | Verifies error when a safe-declared function calls a raising function. | `no_raise_calls_raiser.py` |
| `test_indirect_exception_propagation` | Verifies that exceptions propagate through multiple layers of function calls. | `indirect_exception_propagation.py` |
| `test_stdlib_exception` | Verifies detection of exceptions raised by standard library functions (e.g., `open()` raising `FileNotFoundError`). | `stdlib_exception.py` |
| `test_correct_exception_declaration` | Verifies that correctly declaring all possible exceptions results in success. | `correct_exception_declaration.py` |
| `test_missing_exception_declaration` | Verifies error when a raised exception is missing from the list. | `missing_exception_declaration.py` |
| `test_bare_raise` | Verifies that a bare `raise` statement is flagged. | `bare_raise.py` |
| `test_chained_attributes` | Verifies handling of chained attributes (e.g., `os.path.join`). | `chained_attributes.py` |
| `test_module_exception` | Verifies handling of full module path exceptions (e.g., `builtins.ValueError`). | `module_exception.py` |
| `test_none_tracking` | Verifies that `@raising(exceptions=None)` disables tracking for that function. | `none_tracking.py` |
| `test_async_functions` | Verifies support for `async def` functions. | `async_functions.py` |
| `test_property_methods` | Verifies support for `@property` methods. | `property_methods.py` |
| `test_classmethod_functions` | Verifies support for `@classmethod`. | `classmethod_functions.py` |
| `test_staticmethod_functions` | Verifies support for `@staticmethod`. | `staticmethod_functions.py` |
| `test_instance_methods` | Verifies support for regular instance methods. | `instance_methods.py` |
| `test_custom_config` | Verifies loading custom exception mappings from `mypy.ini` (prefix style). | `custom_config.py` |
| `test_multiline_config` | Verifies loading custom exception mappings using `known_exceptions` multiline syntax. | `multiline_config.py` |
| `test_exception_caught` | Verifies that caught exceptions are not reported as missing. | `exception_caught.py` |
| `test_try_except_handling` | Verifies complex try-except blocks (incorrect catch, re-raise, etc.). | `try_except_handling.py` |
| `test_exception_hierarchy` | Verifies inheritance logic (catching/declaring Base covers Derived) - *Focus of recent refactor*. | `exception_hierarchy.py` |
| `test_strict_mode` | Verifies that strict mode reports errors for missing `@raising` decorators. | `strict_mode.py` |
| `test_declaration_hierarchy` | Verifies declaration of base exception covers derived checks. | `declaration_hierarchy.py` |

## Running Tests

To run the full test suite:

```bash
uv run python -m unittest discover -s tests
```

To run a specific test:

```bash
uv run python -m unittest tests.test_plugin.TestMypyRaisePlugin.test_exception_hierarchy
```

## Internal Unit Tests

In addition to the integration-style plugin tests, `test_internals.py` provides unit tests for internal components to ensure high code coverage and correct handling of edge cases (e.g., TTY detection, error conditions).

| Test Method | Description | Covered Module |
| :--- | :--- | :--- |
| `test_decorator_execution` | Verifies that the `@raising` decorator returns the decorated function correctly at runtime. | `decorator.py` |
| `test_stats` | Verifies the `AnalysisStats` data class and its summary formatting logic. | `stats.py` |
| `test_resolve_exception_errors` | Verifies that valid modules but invalid classes (or invalid modules) return `None` during resolution. | `stdlib_exceptions.py` |
| `test_is_subtype_errors` | Verifies explicit error handling paths in `is_subtype` (e.g., if resolution fails). | `stdlib_exceptions.py` |
| `test_colors_tty` | Verifies that `Colors` helper produces ANSI codes only when attached to a TTY (simulated). | `colors.py` |
| `test_visitor_extract_exception_name` | Verifies the AST extraction logic for exception names, including dotted paths. | `visitor.py` |
