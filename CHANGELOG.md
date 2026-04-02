# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.0] - 2026-04-02

### Added

- Initial release extracted from ClaudioOS wraith-render crate
- Built-in HTML parser (no external dependencies)
- Block layout: headings (h1-h6), paragraphs, lists (ordered + unordered), blockquotes, horizontal rules, preformatted text
- Inline styling: bold, italic, code, links with clickable regions
- Table rendering with auto-sized columns and box-drawing borders
- Form widget rendering: text inputs, passwords, checkboxes, radio buttons, selects, textareas, submit buttons
- Word wrapping at configurable width
- `LinkRegion` and `InputRegion` structs for interactive handling
- `no_std` support with `alloc`
- `std` feature (enabled by default) for standard library environments
- 13 unit tests covering rendering and parsing
