# Automated Release Process

This document explains how to use the automated PyPI publishing system via GitHub Actions.

## Overview

The project supports **two automatic release workflows**:

1. **Production Release** - Publishes to PyPI when you create a GitHub release
2. **Test Release** - Publishes to TestPyPI for testing purposes

## Setup Options

### Option 1: Trusted Publishers (Recommended - Most Secure)

This is the modern, secure approach that doesn't require storing API tokens.

#### PyPI Setup:
1. Go to https://pypi.org/manage/project/markdown-it-accel/settings/publishing/
2. Click "Add a new pending publisher"
3. Fill in these details:
   - **PyPI Project Name**: `markdown-it-accel`
   - **Owner**: `ChungNYCU`
   - **Repository name**: `markdown-it-accel`
   - **Workflow filename**: `ci.yml`
   - **Environment name**: `pypi`

#### TestPyPI Setup:
1. Go to https://test.pypi.org/manage/project/markdown-it-accel/settings/publishing/
2. Click "Add a new pending publisher"
3. Use the same details but set **Environment name**: `testpypi`

### Option 2: API Token (Fallback)

If trusted publishers don't work, the system will automatically fall back to API tokens.

#### Setup:
1. Get your PyPI API token from https://pypi.org/manage/account/#api-tokens
2. Add it as a GitHub repository secret:
   - Go to https://github.com/ChungNYCU/markdown-it-accel/settings/secrets/actions
   - Click "New repository secret"
   - Name: `PYPI_API_TOKEN`
   - Value: Your PyPI API token (starts with `pypi-`)

## How to Release

### Production Release to PyPI

1. **Update version numbers** in both files:
   ```bash
   # Update version in Cargo.toml
   version = "0.2.0"
   
   # Update version in pyproject.toml  
   version = "0.2.0"
   ```

2. **Update CHANGELOG.md** with new features and fixes

3. **Commit and push** the version changes:
   ```bash
   git add Cargo.toml pyproject.toml CHANGELOG.md
   git commit -m "Bump version to 0.2.0"
   git push origin main
   ```

4. **Create a GitHub Release**:
   - Go to https://github.com/ChungNYCU/markdown-it-accel/releases
   - Click "Create a new release"
   - **Tag**: `v0.2.0` (must start with `v`)
   - **Title**: `Release 0.2.0`
   - **Description**: Copy from CHANGELOG.md
   - Click "Publish release"

5. **Automatic Publishing**: GitHub Actions will automatically:
   - ✅ Run all tests on multiple platforms
   - ✅ Build wheels for Windows, Linux, macOS
   - ✅ Build source distribution
   - ✅ Publish to PyPI
   - ✅ Update the release with build artifacts

### Test Release to TestPyPI

For testing the release process without affecting production:

```bash
git commit -m "Test release functionality [test-release]"
git push origin main
```

The `[test-release]` marker in the commit message will trigger publication to TestPyPI.

## Verification

After release, verify the package was published:

- **PyPI**: https://pypi.org/project/markdown-it-accel/
- **TestPyPI**: https://test.pypi.org/project/markdown-it-accel/

Test installation:
```bash
# From PyPI
pip install markdown-it-accel==0.2.0

# From TestPyPI
pip install -i https://test.pypi.org/simple/ markdown-it-accel==0.2.0
```

## Workflow Features

### Robust Publishing
- ✅ **Verification checks**: Ensures wheels and source distribution exist
- ✅ **Dual method support**: Trusted Publishers + API token fallback  
- ✅ **Skip existing**: Won't fail if version already exists
- ✅ **Verbose logging**: Detailed output for debugging
- ✅ **Hash verification**: Publishes with integrity hashes

### Multi-Platform Builds
- ✅ **Windows** (x64)
- ✅ **Linux** (x86_64) 
- ✅ **macOS** (x86_64 and ARM64)
- ✅ **Python 3.8-3.12** support

### Quality Gates
- ✅ **All tests pass** before publishing
- ✅ **Code quality checks** (ruff, black, clippy)
- ✅ **Multi-platform testing**
- ✅ **Rust and Python validation**

## Troubleshooting

### Release Failed
1. Check the GitHub Actions logs: https://github.com/ChungNYCU/markdown-it-accel/actions
2. Common issues:
   - Version already exists on PyPI (use higher version)
   - Trusted publisher not configured correctly
   - Test failures blocking release

### PyPI Authentication Issues
1. **Trusted Publishers**: Verify PyPI configuration matches GitHub repository exactly
2. **API Token**: Ensure `PYPI_API_TOKEN` secret is set correctly
3. **Environment**: Make sure GitHub environment name matches PyPI configuration

### Build Issues
1. **Platform-specific failures**: Check individual platform logs
2. **Rust compilation**: Ensure Cargo.toml dependencies are correct
3. **Python compatibility**: Verify pyproject.toml Python version requirements

## Security Notes

- **Trusted Publishers** are the most secure method (recommended)
- **API tokens** are stored as encrypted GitHub secrets
- **No credentials** are ever exposed in logs or code
- **Environment protection** prevents unauthorized releases
- **Multi-factor verification** through GitHub's security model

## Manual Override

If automation fails, you can still publish manually:

```bash
# Build locally
maturin build --release

# Upload to PyPI
twine upload target/wheels/*
```

But the automated system is more reliable and secure.