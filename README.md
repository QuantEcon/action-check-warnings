# Check for Python Warnings Action

[![CI](https://github.com/QuantEcon/action-check-warnings/actions/workflows/ci.yml/badge.svg)](https://github.com/QuantEcon/action-check-warnings/actions/workflows/ci.yml)

A GitHub Action that scans HTML files for Python warnings within code cell outputs and optionally fails the workflow if any are found. Perfect for ensuring documentation builds are warning-free.

## 🎯 Features

- **Smart Detection**: Only scans warnings within code cell outputs (`cell_output` class) to avoid false positives
- **Comprehensive Coverage**: Supports all Python warning types by default
- **Flexible Reporting**: Create GitHub issues, workflow artifacts, or PR comments
- **Configurable**: Customize warning types, paths, and behavior
- **CI/CD Ready**: Seamlessly integrates with documentation build workflows

## 🚀 Quick Start

```yaml
    - name: Check for warnings
      uses: QuantEcon/action-check-warnings@v1.0.0
      with:
        html-path: './_build/html'
```

## Features

- Scans HTML files for configurable Python warnings **within code cell outputs only**
- Prevents false positives by only checking warnings in `cell_output` HTML elements
- Supports multiple warning types (all Python warning types by default: UserWarning, DeprecationWarning, PendingDeprecationWarning, SyntaxWarning, RuntimeWarning, FutureWarning, ImportWarning, Uni[...]
- Provides detailed output about warnings found
- Optionally fails the workflow when warnings are detected
- **Creates GitHub issues** with detailed warning reports
- **Generates workflow artifacts** containing warning reports
- **Posts PR comments** with warning reports when failing on warnings
- Configurable search path and warning types

## Usage

### Basic Usage

```yaml
- name: Check for Python warnings
  uses: QuantEcon/action-check-warnings@v1.0.0
```

### Advanced Usage with PR Comments

```yaml
- name: Check for Python warnings with PR feedback
  uses: QuantEcon/action-check-warnings@v1.0.0
  with:
    html-path: './_build/html'
    # Uses comprehensive default warnings (all Python warning types)
    fail-on-warning: 'true'  # This will post a comment to the PR if warnings are found
```

### Advanced Usage with Issue Creation

```yaml
- name: Check for Python warnings with issue creation
  uses: QuantEcon/action-check-warnings@v1.0.0
  with:
    html-path: './_build/html'
    # Uses comprehensive default warnings (all Python warning types)
    fail-on-warning: 'false'
    create-issue: 'true'
    issue-title: 'Python Warnings Found in Documentation Build'
```

### Advanced Usage with Issue Creation and User Assignment

```yaml
- name: Check for Python warnings with assigned issue
  uses: QuantEcon/action-check-warnings@v1.0.0
  with:
    html-path: './_build/html'
    # Uses comprehensive default warnings (all Python warning types)
    fail-on-warning: 'false'
    create-issue: 'true'
    issue-title: 'Python Warnings Found in Documentation Build'
    notify: 'username1,username2'  # Assign issue to multiple users
```

### Advanced Usage with Artifact Creation

```yaml
- name: Check for Python warnings with artifact
  uses: QuantEcon/action-check-warnings@v1.0.0
  with:
    html-path: './_build/html'
    # Uses comprehensive default warnings (all Python warning types)
    fail-on-warning: 'true'
    create-artifact: 'true'
    artifact-name: 'python-warning-report'
```

### Complete Advanced Usage

```yaml
- name: Check for Python warnings in build output
  uses: QuantEcon/action-check-warnings@v1.0.0
  with:
    html-path: './_build/html'
    # Uses comprehensive default warnings (all Python warning types)
    fail-on-warning: 'false'
    create-issue: 'true'
    issue-title: 'Python Warnings Detected in Build'
    notify: 'maintainer1,reviewer2'  # Assign to specific team members
    create-artifact: 'true'
    artifact-name: 'detailed-warning-report'
```

### Excluding Specific Warning Types

Sometimes you may want to temporarily exclude certain warning types (e.g., when dealing with upstream warnings that take time to fix):

```yaml
- name: Check for Python warnings excluding upstream warnings
  uses: QuantEcon/action-check-warnings@v1.0.0
  with:
    html-path: './_build/html'
    exclude-warning: 'UserWarning'  # Exclude single warning type
    fail-on-warning: 'true'
```

```yaml
- name: Check for Python warnings excluding multiple warning types
  uses: QuantEcon/action-check-warnings@v1.0.0
  with:
    html-path: './_build/html'
    exclude-warning: 'UserWarning,RuntimeWarning,ResourceWarning'  # Exclude multiple warnings
    fail-on-warning: 'true'
```

### Custom Warning Types with Exclusions

You can combine custom warning lists with exclusions:

```yaml
- name: Check for specific warnings but exclude problematic ones
  uses: QuantEcon/action-check-warnings@v1.0.0
  with:
    html-path: './_build/html'
    warnings: 'UserWarning,DeprecationWarning,RuntimeWarning,ResourceWarning'
    exclude-warning: 'ResourceWarning'  # Check all above except ResourceWarning
    fail-on-warning: 'true'
```

### Using Outputs

```yaml
- name: Check for Python warnings
  id: warning-check
  uses: QuantEcon/action-check-warnings@v1.0.0
  with:
    fail-on-warning: 'false'

- name: Report warnings
  if: steps.warning-check.outputs.warnings-found == 'true'
  run: |
    echo "Found ${{ steps.warning-check.outputs.warning-count }} warnings:"
    echo "${{ steps.warning-check.outputs.warning-details }}"
```

## New Features

### GitHub Issue Creation

When `create-issue` is set to `true`, the action will automatically create a GitHub issue when warnings are detected. The issue includes:

- Detailed warning information with file paths and line numbers
- Repository and workflow context
- Direct links to the failing workflow run
- Suggested next steps for resolution
- Automatic labeling (`bug`, `documentation`, `python-warnings`)

#### Automatic User Assignment

When the `notify` parameter is provided, the created issue will be automatically assigned to the specified GitHub users. This feature supports:

- Single user assignment: `notify: 'username'`
- Multiple user assignment: `notify: 'user1,user2,user3'`
- Robust error handling: If assignment fails, the issue is still created successfully

This ensures that the right team members are immediately notified about warnings and can take action to resolve them.

Additionally, when issues are created in pull request contexts, a simple notification comment is posted to the PR thread containing:

- List of files with warnings
- Direct link to the created issue for detailed information

This provides immediate awareness to PR authors without cluttering the conversation with full warning details.

### Workflow Artifacts

When `create-artifact` is set to `true`, the action generates a detailed Markdown report as a workflow artifact. This report includes:

- Complete warning details in a readable format
- Repository and workflow metadata
- Timestamp and commit information
- Downloadable for offline review

### Pull Request Comments

When `fail-on-warning` is set to `true` and warnings are found in a pull request, the action automatically posts a detailed comment to the PR containing:

- Complete warning information formatted for easy reading
- Direct links to the failing workflow run
- Suggested next steps for fixing the warnings
- Repository and commit context

This feature helps developers quickly identify and fix warnings without digging through workflow logs.

### Using Both Features Together

You can enable both issue creation and artifact generation simultaneously:

```yaml
- name: Comprehensive warning check
  uses: QuantEcon/action-check-warnings@v1.0.0
  with:
    html-path: './_build/html'
    fail-on-warning: 'false'  # Don't fail, just report
    create-issue: 'true'      # Create issue for tracking
    create-artifact: 'true'   # Create artifact for detailed review
```

## How It Works

This action specifically searches for Python warnings within HTML elements that have `cell_output` in their class attribute. This approach prevents false positives that would occur if warnings li[...]

### Example HTML Structure

The action will detect warnings in this structure:
```html
<div class="cell_output">
    <pre>
    /path/to/file.py:10: FutureWarning: This feature will be deprecated
      result = old_function()
    </pre>
</div>
```

But will **ignore** warnings mentioned in regular content:
```html
<div class="content">
    <p>In this tutorial, we'll discuss FutureWarning messages.</p>
</div>
```

This ensures that educational content about warnings doesn't trigger false positives in the check.

## Permissions

For the action to work correctly with all features, ensure your workflow has the appropriate permissions:

```yaml
permissions:
  contents: read          # For checking out the repository
  issues: write          # For creating GitHub issues (if create-issue is enabled)
  actions: read          # For creating workflow artifacts (if create-artifact is enabled)
  pull-requests: write   # For posting PR comments (when fail-on-warning is true OR create-issue is true in PRs)
```

If you're only using the basic warning check functionality, only `contents: read` is required. Add `pull-requests: write` when you want PR comments on warnings or when using issue creation in PR [...]
