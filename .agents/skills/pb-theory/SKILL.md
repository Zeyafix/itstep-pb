---
name: pb-theory
description: Use when creating or editing an itstep-pb theory HTML file, including its layout, lesson content, navigation, special blocks, questions, or code examples.
---

# PB Theory

Preserve the accepted theory-page design while adapting the lesson content for `itstep-pb`. The bundled HTML files demonstrate the page shell and components; shared production styles and behavior come from `itstep-pb-assets/runtime/lesson-v1.css` and `lesson-v1.js`. The samples are self-contained demonstrations, not scripts and styles to duplicate in each lesson. Their Russian sample content illustrates the design, not a required lesson topic or language. Keep the requested lesson language, including interface labels and accessibility text.

## Repository context

- Read applicable `AGENTS.md` instructions and [the current course plan](../../../docs/current-roadmap.md). Keep each lesson within its place in the course. Write for students aged 14–50 with no required prior IT knowledge: simple explanations, concrete demonstrations, and practice alongside theory. Use English identifiers in code.
- A pair lasts 80 minutes; a meeting has two pairs separated by a 10-minute break. Follow the repository's allocation of teaching time and the requested lesson scope when planning content.
- This repository is public. Shared CSS and JavaScript are ordinary files in `itstep-pb-assets/runtime/`. Theory pages belong in public lesson submodules, with `pa` and `hw` in their own public submodules. Student work and progress belong only in private group submodules. Read [the repository guide](../../../docs/development/repositories.md) for the structure and update order.
- Preserve a target page's confirmed working runtime URLs. GitHub Pages is not configured yet; do not infer publication from the runtime folder's presence or invent an unverified hosting URL. Until hosting is configured, connect the checked-out runtime with paths relative to the page and report that hosted URLs remain to be configured. Read [the runtime guide](../../../docs/development/runtime.md) for the current connection details. Keep local previews distinct from confirmed published runtime availability.

## Workflow

1. Read the target HTML, if it exists, [base.html](assets/gold-standard/base.html), and the relevant code in the shared runtime. Identify the page shell and components affected by the request. For special blocks or questions, also read [special blocks](references/special-blocks.md) and the linked HTML. For multiline code, also read [code examples](references/code-examples.md) and the linked HTML. This step is complete when every affected component has a matching standard and its CSS and script dependencies are identified.
2. For a new page, adapt the base shell and replace the demonstration sections with lesson content. For an edit, preserve the lesson's content and working integrations outside the requested change. Apply all relevant rules below and in the selected references. This step is complete when the requested content uses the standard's structure, styles, and behavior, with no unrelated demonstration material left in the result.
3. Validate the affected structure and browser behavior using the completion checks below and in each selected reference. This step is complete when every applicable check has a result; fix failures and explicitly report any check that could not be performed.

## Combining the standards

- `base.html` demonstrates the page shell, typography, themes, navigation, paragraphs, lists, tables, images, and inline-code copying.
- `special-block.html` demonstrates special-block styling and question interactions.
- `code-example.html` demonstrates multiline code styling, console output, block copying, and syntax highlighting.
- Merge the needed component markup into one lesson document. Keep one header, sidebar, main element, empty table-of-contents container, and copy-status live region. The runtime builds navigation and installs theme, copy, and quiz handlers; do not also copy the demonstrations' inline scripts or styles into a lesson.
- Load `lesson-v1.js` once synchronously in the head before the shared CSS, without `async`, `defer`, or `type="module"`: it initializes the theme before painting and waits for the DOM before installing handlers. Load `lesson-v1.css` once. Preserve the runtime's theme-storage key rather than replacing it with the standalone demonstrations' key.
- The checked-out runtime is authoritative for production selectors, data attributes, styles, and behavior when a bundled standalone sample differs. Preserve its responsive rules, print styles, and accessibility hooks. Add a missing shared capability in the runtime folder of the main repository only when the task requires it; do not duplicate or refactor unrelated runtime code during a content edit.

## Page shell and appearance

Use the base's fixed `.page-header`, collapsible `aside.sidebar#contents-sidebar`, `.sidebar-backdrop`, and `main#main-content`. Retain the skip link, sidebar toggle, theme toggle, reading-progress line, sidebar attribution, and active-section indication.

Keep the supplied light/dark palette, dotted background, translucent panels, border treatment, spacing, and font roles: Segoe UI/Arial for prose and headings, Consolas/Courier New for code. Read exact values from the shared runtime instead of approximating them. The desktop header is 72px high, sidebar 248px wide, and main content at most 1020px wide. Preserve the 1516px overlay breakpoint in both CSS and JavaScript and the 600px mobile rules.

Theme preference may use local storage, as in the standard. Retain system-theme detection, storage failure handling, keyboard focus, overlay focus management, Escape/backdrop dismissal, reduced-motion support, and print styles.

## Titles, sections, and navigation

- Use one `h1.page-header__title`. The browser `<title>` must have exactly the same text, with no extra suffix or prefix. Update the description and document language to fit the lesson.
- Each lesson section is a direct child of `main`, has a unique stable `id`, and contains a direct `h2` followed by `div.section-content`.
- Use the section heading format `01 / Topic`, `02 / Topic`, and so on in lesson order. Keep numbering in heading text, not in a separate sidebar-only label.
- Build the table of contents from `main > section[id]` and each section's direct `h2`. Every sidebar item links to that section's fragment and duplicates its full heading text, including the number. Preserve the full text and tooltip when CSS visually truncates a long label.
- Only section headings belong in the sidebar. Special-block titles, nested headings, external resources, summaries, and shortened alternative labels do not create sidebar items.
- Use paragraphs, lists, tables, figures, and special blocks inside `.section-content`. Special-block headings are `h3` and remain inside their parent section.

## Prose, lists, tables, and inline code

- Write simple lesson prose, with no em dash. Use `<p>`, `<strong>`, and inline `<code>` as shown in the base.
- Use `ul.content-list` for unordered points and `ol.content-list` for sequences. Preserve native list markers and the standard's spacing.
- Wrap each table in `.table-scroll` with `tabindex="0"`, `role="region"`, and a descriptive `aria-label`. Use a caption, `thead`, `tbody`, and `th scope="col"` as in the base. Keep horizontal scrolling inside the table wrapper.
- Use descriptive link text. When opening a link in a new tab, retain `rel="noopener noreferrer"`.
- Preserve the inline-copy behavior and tooltip. The base skips code inside `pre`, `label`, or `button`; retain those exclusions when combining inline code, questions, and code examples.

## Images

Use `figure.image-block > div.image-block__frame > img`, followed by `figcaption`. Preserve the corner frame, natural aspect ratio, maximum dimensions, `loading="lazy"`, `decoding="async"`, meaningful alt text, and numbered captions in the lesson language.

The user supplies image URLs, either by editing the file or explicitly giving the agent a URL. Preserve existing image sources unless asked to change them. Do not search for, generate, guess, or reuse demonstration image URLs for a new lesson. When a needed URL is missing, leave a clearly named HTML comment at the intended figure position and report the pending image; do not emit a broken or empty `img src`. Continue the rest of the authorized work.

## Completion checks

- Compare the resulting page with the relevant bundled examples in light and dark themes, on a wide desktop and narrow screen. Check headings, panel layout, spacing, component nesting, and contained horizontal overflow.
- Verify title equality, unique IDs, sequential section numbering, and an exact one-to-one match between sidebar links and direct section headings. Follow the links and check the active-section indicator.
- When shell or layout code changed, exercise sidebar controls, keyboard navigation, focus return, reading progress, reduced motion, and print layout.
- When affected, verify inline copying, image captions and supplied sources, table scrolling, and all component-specific checks from the references. Do not claim browser checks based only on reading source.
- Verify that runtime links resolve in the intended local or hosted environment, that each runtime file loads once, and that there are no duplicate inline handlers or copied shared styles. Do not claim a hosted connection works from a local-file check.
- Report changes and validation limits. Apply the repository completion checklist: developer guidance belongs in `docs/development`, teacher guidance in `docs/teacher`, and the current course plan in `docs/current-roadmap.md`. Update the affected documents when lesson scope or authoring workflow changes; homework/practice tests and rubrics apply only when those materials were changed and those checks exist.
