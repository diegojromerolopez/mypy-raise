# Contributing to mypy-raise

Thank you for your interest in contributing to mypy-raise! This document provides guidelines and instructions for contributing.

## Ways to Contribute

### 1. Expand the Standard Library Exception Mappings

The most valuable contribution is adding more standard library functions and their exceptions to the mappings.

**What to add:**
- Standard library functions with their possible exceptions
- Common exception patterns
- Edge cases and less common stdlib functions

**How to add:**

1. Edit `mypy_raise/stdlib_exceptions.py`
2. Add the function to the `STDLIB_EXCEPTIONS` dictionary with its fully qualified name
3. List all exceptions it can raise as a set
4. Group it with similar functions and add comments
5. Run tests to ensure nothing breaks

Example:
```python
# JSON module
'json.loads': {'JSONDecodeError', 'ValueError'},
'json.dumps': {'TypeError', 'ValueError'},
```

### 2. Report Bugs

Found a bug? Please open an issue with:
- A clear description of the problem
- Minimal code example that reproduces the issue
- Expected vs actual behavior
- Your Python and mypy versions

### 3. Suggest Features

Have an idea? Open an issue describing:
- The feature and why it's useful
- Example use cases
- Potential implementation approach (if you have one)

### 4. Improve Documentation

Help make the docs better:
- Fix typos or unclear explanations
- Add more examples
- Improve the README
- Add docstrings to code

## Development Setup

### Prerequisites

- Python 3.10 or higher
- Git
- uv (recommended) or pip

### Setup

```bash
# Clone your fork
git clone https://github.com/YOUR_USERNAME/mypy-raise.git
cd mypy-raise

# Install uv if you don't have it
pip install uv

# Install dependencies
uv sync --all-groups
```

## Running Tests

```bash
# Run all tests
uv run python -m unittest discover -s tests

# Run specific test
uv run python -m unittest tests.test_plugin.TestMypyRaisePlugin.test_stdlib_exception

# Run with coverage
uv run coverage run --source=mypy_raise -m unittest discover -s tests
uv run coverage report
uv run coverage report --show-missing  # See which lines aren't covered
```

## Code Style

We use:
- **black** for code formatting (line length 120)
- **isort** for import sorting
- **flake8** for linting
- **mypy** for type checking
- **ruff** for additional linting

Run before committing:

```bash
# Format code
uv run black mypy_raise tests

# Sort imports
uv run isort mypy_raise tests

# Check linting
uv run flake8 mypy_raise tests
uv run ruff check mypy_raise tests

# Type check
uv run mypy mypy_raise/
```

## Pull Request Process

1. **Fork** the repository
2. **Create a branch** for your changes: `git checkout -b feature/your-feature-name`
3. **Make your changes** following the code style guidelines
4. **Add tests** if applicable
5. **Run tests** to ensure everything passes
6. **Run code quality checks** (black, isort, flake8, mypy)
7. **Commit** with a clear message describing your changes
8. **Push** to your fork
9. **Open a Pull Request** with:
   - Clear description of changes
   - Why the changes are needed
   - Any related issues

## Adding Tests

When adding new stdlib exception mappings or features, add test cases:

1. Create a test resource file in `tests/resources/`
2. Add a test method in `tests/test_plugin.py`
3. Ensure the test fails without your changes
4. Ensure the test passes with your changes

Example test resource:
```python
from mypy_raise import raising

@raising(exceptions=[])
def use_new_function():
    # This should be detected as raising exceptions
    return new_stdlib_function()
```

Example test method:
```python
def test_new_stdlib_function(self):
    """Test new stdlib function exception detection."""
    output = self.run_mypy('new_function_test.py')
    self.assertIn('may raise', output)
    self.assertIn('not declared', output)
```

## Project Structure

```
mypy-raise/
├── mypy_raise/
│   ├── __init__.py          # Public API
│   ├── decorator.py         # @raising decorator
│   ├── visitor.py           # AST visitor for exception tracking
│   ├── checker.py           # Exception propagation logic
│   ├── plugin.py            # Mypy plugin integration
│   ├── types.py             # Type aliases
│   └── stdlib_exceptions.py # Standard library exception mappings
├── tests/
│   ├── test_plugin.py       # Test suite
│   └── resources/           # Test resource files
├── .github/workflows/       # CI/CD workflows
├── pyproject.toml           # Project configuration
├── README.md
├── CHANGELOG.md
└── CONTRIBUTING.md
```

## Questions?

Feel free to open an issue for any questions about contributing!

## License

By contributing, you agree that your contributions will be licensed under the MIT License.
