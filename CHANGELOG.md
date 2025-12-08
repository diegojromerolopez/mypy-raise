# Change Log
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](http://keepachangelog.com/)
and this project adheres to [Semantic Versioning](http://semver.org/).



## 0.1.0 (2025-12-08)

### Features
- **Core Analysis**:
  - **Exception Hierarchy**: Catch blocks and declarations support inheritance (e.g. `Exception` covers `ValueError`).
  - **Exception Propagation**: Full tracking through function call chains.
  - **Try-Except Analysis**: Smart handling of caught exceptions (passed/failed).
  - **Indirect Propagation**: Detects exceptions raised deep in the call stack.
- **Decorators**: `@raising` with `exceptions=[...]` support.
- **Standard Library**: Built-in support for 100+ stdlib functions.
- **Configuration & Usage**:
  - **Strict Mode**: Enforce decorators on all functions (`strict = true`).
  - **Ignore Patterns**: Exclude files/functions via config.
  - **Custom Exceptions**: Define exceptions for third-party libs in `mypy.ini`.
- **Developer Experience**:
  - **Enhanced Error Messages**: Hints, source tracing ("Raised by"), and colored output.
  - **Statistics**: Run summary showing compliance rate and violation counts.

### Implementation
- **Architecture**: Modular design (Plugin, Visitor, Checker, Stats).
- **Quality**: Type aliases, rigorous type hints.

### Testing
- **Comprehensive Suite**: 23 tests covering direct raises, propagation, correct/incorrect declarations, and strict mode.
- **High Coverage**: ~90% test coverage.

### Documentation
- Full README with strict mode and configuration guides.
- Contributing guide.

## Future Plans
- Integration with popular third-party libraries
- Performance optimizations for large codebases
