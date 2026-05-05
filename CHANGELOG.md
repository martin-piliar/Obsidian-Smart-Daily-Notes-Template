# Changelog
All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.1.0] - 2026-05-05
### Added
- Named configuration block at the top of the template (`SECTION_NAME`, `DAILY_NOTES_FOLDER`, `FILENAME_FORMAT`, `MAX_DAYS_TO_LOOK`) for easier customization without editing the logic
- Configurable `FILENAME_FORMAT` constant to support custom daily note filename patterns
- Not-found message now includes the oldest date checked for better debugging

### Fixed
- Regex now uses `(?=\n# |$)` instead of `(?=\n#|$)`, so sub-headings like `## Tickets` inside the target section no longer truncate the matched content

### Changed
- Entire script is now a single `<%* %>` block; the embed link is emitted via `tR +=` instead of mixed raw markup

## [1.0.0] - 2025-02-24
### Added
- Initial release of the Smart Daily Notes Template
- Previous work item lookup
- Empty note skipping functionality
- Configurable folder path structure
- Maximum lookback period (14 days)
- Safety checks to prevent infinite loops
- Basic documentation in README.md
- MIT License