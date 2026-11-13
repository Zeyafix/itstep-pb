# Code examples

Read [code-example.html](../assets/gold-standard/code-example.html) when adding or editing multiline code. Inspect `.code-example`, `copyCode`, and `createHighlighter` in `itstep-pb-assets/runtime/lesson-v1.css` and `lesson-v1.js` from the repository root. The bundled page demonstrates the appearance; the shared runtime supplies production styles and handlers.

## Structure and placement

A `.code-example` belongs inside either `.section-content` or `.special-block__body`. Use the same component in both locations:

```html
<div class="code-example">
  <div class="code-example__header">
    <span>Assignment</span>
    <button type="button" class="code-example__copy-button"
      aria-label="Copy example: Assignment">Copy</button>
  </div>
  <pre tabindex="0"><code class="language-python">score = 10
score = score + 5
print(score)</code></pre>
  <div class="code-example__output">
    <div class="code-example__output-title">Console</div>
    <pre tabindex="0" aria-label="Example output: Assignment">15</pre>
  </div>
</div>
```

Use a descriptive example title and labels in the lesson language. Escape HTML-sensitive characters in source code and output. Preserve Python indentation and deliberate line breaks without adding HTML-layout indentation inside `pre`. Show the actual output for an executable example; omit the output region when there is no output to display. Output uses `pre` without `code`, so it is not highlighted as Python or copied as source.

## Appearance and behavior

- Preserve the rounded bordered container, compact header, copy button, dark source area, and separate darker console area. Source and console stay dark in both page themes; print rules make them readable on white.
- Use the shared runtime's `--code-background: #1e1e1e`, `pre` and `pre code` rules, output rules, and mobile/print overrides. Retain horizontal scrolling and focusable source/output regions. Do not apply inline-code chips to multiline source or copy the demo's stylesheet into a lesson.
- Mark Python source as `code.language-python` inside `.code-example pre`: the runtime's highlighter selects that class, even though the standalone sample highlights unclassified code. The runtime owns the pinned Shiki import and `dark-plus` theme; do not add a second import or highlighter. Preserve the existing `pre` and replace only the code's children with highlighted tokens when changing that integration. Keep plain source readable and copyable if the module cannot load. This is display-only highlighting, not a Python execution environment.
- Copy only the current block's `pre code` text, excluding the title, button label, and console output. Retain the clipboard API, fallback textarea copy, manual selection fallback, temporary button feedback, and shared `#copy-status` live region.
- Let the shared runtime install block-copy listeners and provide the clipboard helper. Keep the inline-copy exclusions for code inside `pre`, `label`, and `button` so highlighting and controls do not create nested buttons. Do not copy the standalone sample's handlers into a lesson.

## Completion checks

Check rendering and copying in both placement contexts when affected. Verify that copied text matches source text, including whitespace and HTML-sensitive characters, before and after highlighting. Check that console output matches the example and is excluded from copied source. Exercise the unavailable-highlighter and clipboard-failure paths when changing those integrations. Inspect narrow-screen scrolling and print output when changing code styles.
