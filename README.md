# Test ESP-IDF Projects Action

A GitHub Action for testing ESP-IDF projects using pytest with comprehensive test reporting and artifact management.

## Description

This action automates the testing of ESP-IDF projects by:
- Installing required dependencies (`idf-ci`)
- Downloading build artifacts from [build-esp-idf-projects-action](https://github.com/espressif/build-esp-idf-projects-action/) 
- Running `pytest` with configurable parameters
- Generating JUnit XML test reports
- Uploading test results as artifacts

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `nodes` | The pytest nodes to test | ✅ Yes | - |
| `additional_args` | Additional arguments to pass to pytest | ❌ No | - |
| `junit_filepath` | The name of the junit file report | ❌ No | - |
| `debug` | Enable debug mode | ❌ No | `false` |

## Usage

### Basic Usage

```yaml
- name: Test ESP-IDF Projects
  uses: your-org/test-esp-idf-projects-action@v1
  with:
    nodes: "tests/test_basic.py::test_hello_world"
```

### Advanced Usage

```yaml
- name: Test ESP-IDF Projects
  uses: your-org/test-esp-idf-projects-action@v1
  with:
    nodes: "tests/test_basic.py::test_hello_world tests/test_advanced.py"
    additional_args: "--maxfail=1 --tb=short"
    junit_filepath: "test-results.xml"
    debug: "true"
```

### Complete Workflow Example

```yaml
name: ESP-IDF Project Tests

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      
      # Your build steps here
      - name: Build ESP-IDF Project
        run: |
          # Build commands
          
      - name: Upload Build Artifacts
        uses: actions/upload-artifact@v4
        with:
          name: build-artifacts
          path: build/

  test:
    needs: build
    runs-on: ubuntu-latest
    steps:
      - name: Test ESP-IDF Projects
        uses: your-org/test-esp-idf-projects-action@v1
        with:
          nodes: "tests/"
          junit_filepath: "pytest-results.xml"
          debug: "false"
```

## Features

### 🔧 Automatic Dependency Management
- Automatically installs `idf-ci` package
- No need to manually manage Python dependencies

### 📦 Artifact Integration
- Downloads artifacts from previous workflow steps
- Displays downloaded file structure for debugging
- Automatically uploads test results as artifacts

### 🧪 Flexible Testing
- Support for any pytest node specification
- Configurable additional pytest arguments
- Optional JUnit XML report generation
- Debug mode with detailed logging

### 📊 Test Reporting
- Generates JUnit XML reports when `junit_filepath` is specified
- Uploads test results as named artifacts (`test-report-{junit_filepath}`)
- Easy integration with GitHub's test reporting features

## Input Details

### `nodes` (Required)
Specifies which pytest nodes to run. This can be:
- A specific test file: `tests/test_basic.py`
- A specific test function: `tests/test_basic.py::test_function`
- Multiple nodes: `tests/test_basic.py tests/test_advanced.py`
- A directory: `tests/`

### `additional_args` (Optional)
Any additional pytest arguments you want to pass. Common examples:
- `--maxfail=1` - Stop after first failure
- `--tb=short` - Short traceback format
- `--verbose` - Verbose output
- `--capture=no` - Don't capture stdout

### `junit_filepath` (Optional)
When specified, generates a JUnit XML report at the given path. The report will be uploaded as an artifact named `test-report-{junit_filepath}`.

### `debug` (Optional)
When set to `"true"`, enables debug-level logging with `--log-cli-level=DEBUG`.

## Outputs

This action produces the following artifacts:
- **Test Results**: When `junit_filepath` is specified, uploads the JUnit XML report as `test-report-{junit_filepath}`

## Requirements

- The action expects pytest-compatible test files
- Build artifacts should be uploaded in previous workflow steps if your tests depend on them
- Tests should be compatible with the `idf-ci` package

## License

[Your License Here]

## Contributing

[Your contributing guidelines here] 