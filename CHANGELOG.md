# Changelog

All notable changes to this project will be documented in this file, in reverse chronological order by release.

The format is based on [Keep a Changelog](http://keepachangelog.com/en/1.0.0/)
and this project adheres to [Semantic Versioning](http://semver.org/spec/v2.0.0.html).

## [Unreleased](https://github.com/orisai/github-workflows/compare/1.1.1...HEAD)

## [1.1.2](https://github.com/orisai/github-workflows/compare/1.1.1...1.1.2) - 2023-02-15

### Fixed

- Coveralls PHP Upload: fix flag name to differentiate jobs and matrix variations
- Infection PHP: better cache isolation, support cache updates
- PHP_CodeSniffer: better cache isolation, support cache updates
- PHPStan: better cache isolation, support cache updates
- PHPUnit: better cache isolation, support cache updates
- Setup PHP: add (non) thread-safe PHP version to extensions cache key

## [1.1.1](https://github.com/orisai/github-workflows/compare/1.1.0...1.1.1) - 2022-12-07

### Changed

- Update all dependencies to latest version (as available at 2022-12-07)
- Update documentation examples

### Fixed

- Fix `set-output` deprecation

## [1.1.0](https://github.com/orisai/github-workflows/compare/1.0.0...1.1.0) - 2022-07-04

### Added

- Actions
  - Status Check

## [1.0.0](https://github.com/orisai/github-workflows/releases/tag/1.0.0) - 2022-02-15

### Added

- Workflows
	- Lock closed threads
- Actions
	- Setup PHP
	- Setup Composer
	- PHPUnit
	- PHPStan
	- PHP_Codesniffer
	- Infection PHP
	- Setup NodeJS
	- Coveralls PHP Upload
	- Coveralls finish
- Examples
	- PHPUnit and Coveralls
	- PHPStan
	- PHP_Codesniffer
	- Infection PHP
