# Contributing to markdown-it-accel

Thank you for your interest in contributing to markdown-it-accel! This document provides guidelines and instructions for contributors.

## Development Setup

### Prerequisites

- Python 3.8+ 
- Rust 1.70+
- Git

### Environment Setup

1. **Clone the repository**
   ```bash
   git clone https://github.com/ChungNYCU/markdown-it-accel.git
   cd markdown-it-accel
   ```

2. **Set up Python environment**
   ```bash
   python -m venv .venv
   source .venv/bin/activate  # or .venv\Scripts\activate on Windows
   pip install --upgrade pip
   ```

3. **Install dependencies**
   ```bash
   pip install -e .[dev]
   ```

4. **Set up Rust**
   ```bash
   rustup toolchain install stable
   rustup default stable
   rustup component add rustfmt clippy
   ```

## Development Workflow

### Building

```bash
# Development build
maturin develop

# Release build (for benchmarking)
maturin develop --release
```

### Testing

```bash
# Run all tests
pytest tests/ -v

# Run with coverage
pytest tests/ --cov=markdown_it_accel

# Run benchmarks
python bench/scripts/benchmark.py
```

### Code Quality

```bash
# Format code
make format

# Lint code  
make lint

# Or manually:
black .
ruff check .
cargo fmt
cargo clippy -- -D warnings
```

## Contributing Guidelines

### Code Style

- **Python**: Follow PEP 8, use Black formatter
- **Rust**: Follow standard Rust conventions, use rustfmt
- **Line length**: 88 characters for Python, 100 for Rust
- **Type hints**: Required for all public Python APIs

### Commit Messages

Use conventional commits format:
- `feat: add new feature`
- `fix: resolve bug`
- `docs: update documentation`
- `test: add tests`
- `refactor: improve code structure`
- `perf: performance improvement`

### Pull Request Process

1. **Fork and clone** the repository
2. **Create a feature branch** from `main`
   ```bash
   git checkout -b feature/your-feature-name
   ```
3. **Make your changes** following the guidelines
4. **Add tests** for new functionality
5. **Run the test suite** to ensure everything passes
6. **Update documentation** if needed
7. **Commit your changes** with descriptive messages
8. **Push** to your fork and **create a pull request**

### Pull Request Requirements

- [ ] Tests pass locally
- [ ] Code follows style guidelines
- [ ] New functionality has tests
- [ ] Documentation updated if needed
- [ ] CHANGELOG.md updated for user-facing changes
- [ ] PR description explains the changes

## Architecture Notes

### Project Structure
```
markdown-it-accel/
├── src/lib.rs              # Rust core implementation
├── python/markdown_it_accel/  # Python package
│   ├── __init__.py         # Package exports
│   ├── _shim.py           # Monkey-patching logic
│   └── _rust.pyi          # Type hints
├── tests/                  # Test suite
├── bench/                  # Benchmarks
└── .github/workflows/      # CI/CD
```

### Key Components

1. **Rust Core** (`src/lib.rs`): PyO3 module using pulldown-cmark
2. **Python Shim** (`_shim.py`): Monkey-patches markdown-it-py instances
3. **Fallback Logic**: Automatic detection and graceful degradation

### Design Principles

- **Performance**: Rust implementation should be significantly faster
- **Compatibility**: Must maintain API compatibility with markdown-it-py
- **Reliability**: Automatic fallback ensures robustness
- **Ease of use**: Single function call to enable acceleration

## Testing

### Test Categories

- **Unit tests**: Core functionality testing
- **Integration tests**: End-to-end workflow testing  
- **Performance tests**: Benchmark validation
- **Compatibility tests**: Python/Rust equivalence

### Adding Tests

1. Add test cases to appropriate files in `tests/`
2. Use descriptive test names and docstrings
3. Include both positive and negative test cases
4. Test edge cases and error conditions

## Benchmarking

### Running Benchmarks

```bash
# Full benchmark suite
python bench/scripts/benchmark.py

# Compare with other libraries
python bench/scripts/compare_libraries.py

# Generate test data
python bench/scripts/generate_test_data.py
```

### Adding Benchmarks

1. Add test data to `bench/data/`
2. Update benchmark scripts if needed
3. Ensure benchmarks demonstrate clear performance gains

## Documentation

### Requirements

- All public APIs must have docstrings
- Include usage examples where appropriate
- Update README.md for user-facing changes
- Update CHANGELOG.md for releases

### Documentation Format

- Use Google-style docstrings for Python
- Use standard Rust doc comments (`///`)
- Include type information and examples

## Release Process

1. Update version in `Cargo.toml` and `pyproject.toml`
2. Update `CHANGELOG.md` with new features and fixes
3. Create release PR
4. After merge, create GitHub release tag
5. CI will automatically build and publish to PyPI

## Getting Help

- **Issues**: Report bugs and request features via GitHub Issues
- **Discussions**: Ask questions in GitHub Discussions
- **Discord**: Join our development Discord (link in README)

## Code of Conduct

This project follows the [Contributor Covenant](https://www.contributor-covenant.org/).
Please be respectful and inclusive in all interactions.

## License

By contributing, you agree that your contributions will be licensed under the MIT License.