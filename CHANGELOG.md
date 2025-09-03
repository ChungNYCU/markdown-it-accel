# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.0] - 2025-09-03

### Added
- Initial release of markdown-it-accel
- Rust-accelerated backend using pulldown-cmark
- PyO3 bindings for Python integration
- Automatic fallback to markdown-it-py
- Support for CommonMark and GFM features including:
  - Headers, emphasis, code blocks
  - Lists (ordered and unordered)
  - Tables, links, images
  - Blockquotes, strikethrough
- Performance improvements: 50-200x speedup over pure Python
- Cross-platform binary wheels (Windows, Linux, macOS)
- Comprehensive test suite
- Performance benchmark suite
- CI/CD pipeline with GitHub Actions
- Environment variable control (`MARKDOWN_IT_ACCEL`)

### Features
- `use_rust_core()` function for easy acceleration
- `is_available()` to check Rust availability
- `get_version()` for version information
- Graceful fallback mechanism
- Memory-efficient Rust implementation
- Type hints for better IDE support

### Performance
- 79.88x speedup on BIG.md (8.0KB)
- 46.56x speedup on CODE_HEAVY.md (8.7KB) 
- 64.68x speedup on HUGE.md (434KB)
- 76.36x speedup on TABLE_HEAVY.md (2.1KB)
- Scales excellently with document size

[0.1.0]: https://github.com/ChungNYCU/markdown-it-accel/releases/tag/v0.1.0