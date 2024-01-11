# Host Layerable Shadow Roots Polyfill Exploration

Current work: https://knowler.dev/demos/pmPRuB1

The general technique is to get all the applicable styles (i.e. the
layer and any previous layers) from the host context and to inject them
to the first layer of the shadow root (while ideally respecting any
existing layers that are there).

## Challenges & Limitations

Without a pre-runtime helper (e.g. a PostCSS plugin) to collect all of
the relevant styles, we have to parse through almost all of the existing
styles for a page. CSS Nesting makes this significantly more difficult.
To start, we might just need to limit the injected styles to those set
at the top-level. That is likely how document authors are setting their
styles anyway.

A pre-runtime helper would not be able to account for all the styles on
a page as there are different sources (e.g. style tags or constructed
stylesheets).

One idea to help with the complications created by CSS Nesting is to
take an inventory of the relevant elements in a shadow root prior to use
as a guide for whether to traverse into nested styles to look for nested
`@layer` rules. That has some limitations on its own, namely that a
shadow root can have elements added to it after the fact. Also, complex
selectors can create a false negative about whether it’s worth
traversing into a nest.
