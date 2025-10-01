# Test Files for Check Warnings Action

This directory contains test HTML files used to validate the check-warnings action functionality.

## Test Files

### with-warnings.html
- Contains HTML with Python warnings in `cell_output` elements
- Tests detection of DeprecationWarning and SyntaxWarning
- Should be detected by the action

### with-new-warnings.html  
- Contains newer types of warnings for testing
- Tests detection of various warning types

### clean.html
- Contains HTML output without any warnings
- Should pass all warning checks

### exclude-test.html
- Contains warnings that can be used to test exclusion functionality
- Tests the exclude-warning parameter

## Usage in CI

These files are automatically used by the GitHub Actions CI workflow to test:
- Warning detection accuracy
- False positive avoidance (only checks `cell_output` elements)  
- Exclusion functionality
- Clean file handling

## Running Tests Locally

You can test the action locally using these files:

```bash
# Test with warnings (should find issues)
./action.yml --html-path tests --fail-on-warning false

# Test with clean files only (should pass)
./action.yml --html-path tests/clean.html --fail-on-warning true

# Test exclusion functionality
./action.yml --html-path tests --warnings "DeprecationWarning" --exclude-warning "SyntaxWarning"
```