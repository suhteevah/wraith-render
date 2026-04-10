# wraith-render

[![no_std](https://img.shields.io/badge/no__std-yes-green)](https://rust-embedded.github.io/book/)
[![License: MIT OR Apache-2.0](https://img.shields.io/badge/license-MIT%20OR%20Apache--2.0-blue)](LICENSE-MIT)

**`no_std` HTML to text-mode character grid renderer** -- like lynx/links for embedded systems.

Turns HTML into a fixed-width character grid (`Vec<Vec<Cell>>`) with styled characters, tracked link regions, and form input regions. Zero external dependencies. Works in `no_std` environments with a global allocator.

## Features

- **Block layout** -- headings (h1-h6), paragraphs, ordered & unordered lists, blockquotes, horizontal rules, preformatted text
- **Inline styling** -- bold, italic/emphasis, code, hyperlinks
- **Tables** -- auto-sized columns with Unicode box-drawing borders
- **Forms** -- text inputs, password fields, checkboxes, radio buttons, dropdowns, textareas, submit buttons
- **Word wrapping** -- automatic line-break at word boundaries
- **Link & input tracking** -- `LinkRegion` and `InputRegion` structs for building interactive text browsers
- **Built-in HTML parser** -- tokenizer + tree builder included, no extra crate needed
- **`no_std` + `alloc`** -- runs on bare metal, RTOS, or any environment with a heap

## Usage

Add to your `Cargo.toml`:

```toml
[dependencies]
wraith-render = "0.1"
```

For `no_std` environments:

```toml
[dependencies]
wraith-render = { version = "0.1", default-features = false }
```

### Quick start

```rust
use wraith_render::{parse, render};

let doc = parse("<h1>Hello</h1><p>Welcome to <b>wraith-render</b>.</p>");
let page = render(&doc, 80, 24);

// Print the rendered text
for row in &page.cells[..page.content_height] {
    let line: String = row.iter().map(|c| c.ch).collect();
    println!("{}", line.trim_end());
}

// Access link regions
for link in &page.links {
    println!("Link at row {}: {}", link.row, link.url);
}
```

### One-shot convenience

```rust
let page = wraith_render::render_html("<p>Hello world</p>", 80, 0);
```

### Rendering a login form

```rust
use wraith_render::render_html;

let html = r#"
<h1>Sign In</h1>
<form action="/login" method="post">
    <label>Email</label>
    <input type="email" name="email" />
    <label>Password</label>
    <input type="password" name="password" />
    <input type="submit" value="Sign In" />
</form>
"#;

let page = render_html(html, 60, 0);

// 3 input regions: email, password, submit
assert_eq!(page.inputs.len(), 3);
```

## How it works

1. **Parse**: The built-in HTML parser tokenizes the input and builds a flat node arena (`Document`) with parent/child indices. Handles start/end tags, self-closing tags, void elements, comments, and HTML entities.

2. **Render**: A single-pass tree walker converts the DOM into a character grid. Block elements cause line breaks, inline elements apply styles, and form elements produce text-mode widgets.

3. **Output**: A `TextPage` containing the cell grid, link regions, input regions, page title, and content dimensions.

## Supported HTML elements

| Category | Elements |
|----------|----------|
| Block | `div`, `p`, `h1`-`h6`, `ul`, `ol`, `li`, `blockquote`, `pre`, `hr`, `br`, `table`, `form`, `section`, `article`, `nav`, `header`, `footer`, `main`, `aside`, `figure`, `details`, `fieldset` |
| Inline | `a`, `strong`/`b`, `em`/`i`, `code`, `span`, `label` |
| Form | `input` (text/email/password/checkbox/radio/submit/hidden), `textarea`, `select`/`option`, `button` |
| Table | `table`, `tr`, `td`, `th`, `thead`, `tbody`, `tfoot` |
| Skipped | `script`, `style`, `head`, `meta`, `link`, `noscript` |

## License

Licensed under either of

- Apache License, Version 2.0 ([LICENSE-APACHE](LICENSE-APACHE) or <http://www.apache.org/licenses/LICENSE-2.0>)
- MIT license ([LICENSE-MIT](LICENSE-MIT) or <http://opensource.org/licenses/MIT>)

at your option.

## Origin

Extracted from [ClaudioOS](https://github.com/suhteevah), a bare-metal Rust operating system for running AI coding agents.

---

---

---

---

---

---

## Support This Project

If you find this project useful, consider buying me a coffee! Your support helps me keep building and sharing open-source tools.

[![Donate via PayPal](https://img.shields.io/badge/Donate-PayPal-blue.svg?logo=paypal)](https://www.paypal.me/baal_hosting)

**PayPal:** [baal_hosting@live.com](https://paypal.me/baal_hosting)

Every donation, no matter how small, is greatly appreciated and motivates continued development. Thank you!
