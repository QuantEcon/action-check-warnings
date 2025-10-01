# Changelog

All notable changes to the Check Python Warnings action will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

### Added
- Initial release of the Check Python Warnings action
- Smart detection of warnings within HTML code cell outputs only
- Support for all Python warning types by default
- Configurable warning types and exclusions
- GitHub issue creation with detailed reports
- Workflow artifact generation
- PR comment integration when failing on warnings
- Comprehensive CI testing

### Features
- Scans HTML files for Python warnings in `cell_output` elements
- Avoids false positives by ignoring warnings in text content
- Supports custom warning lists and exclusions
- Optional workflow failure on warning detection
- Detailed JSON output with warning locations and types
- Integration with GitHub Issues API for automated reporting
- Artifact creation for warning analysis
- Assignable GitHub issue notifications

## [1.0.0] - 2025-10-01

### Added
- Initial stable release migrated from QuantEcon/meta repository
- Full compatibility with existing workflows
- Enhanced documentation and examples
- Comprehensive test suite
- GitHub Marketplace listing