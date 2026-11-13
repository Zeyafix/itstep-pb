# Special blocks and questions

Read [special-block.html](../assets/gold-standard/special-block.html) when adding or editing a special block or question. Inspect `.special-block`, `.quiz`, `resetQuiz`, and `shuffleOptions` in `itstep-pb-assets/runtime/lesson-v1.css` and `lesson-v1.js` from the repository root. Use the sample for markup and appearance and the shared runtime for production styles and behavior; do not copy the standalone page's scripts or styles into a lesson.

## Structure and styling

Place special blocks inside a section's `.section-content`. Use this hierarchy from the standard:

```html
<aside class="special-block special-block--definition" aria-labelledby="term-title">
  <div class="special-block__header">
    <span class="special-block__label">Definition</span>
  </div>
  <h3 class="special-block__title" id="term-title">A meaningful title</h3>
  <div class="special-block__body">
    <p>The explanation belongs here.</p>
  </div>
</aside>
```

Give each title a unique ID and match `aria-labelledby` to it. The label describes the block type; the title describes its content. Neither creates a sidebar item.

| Modifier | Label meaning | Accent |
| --- | --- | --- |
| `special-block--definition` | Definition | `#74a9fa` |
| `special-block--warning` | Warning | `#f0a24f` |
| `special-block--correct` | Correct approach | `#5dbb86` |
| `special-block--incorrect` | Incorrect approach | `#ee7d7d` |
| `special-block--tip` | Tip | `#b1cf5d` |
| `special-block--question` | Question | `#b399ed` |

Preserve the square border, colored corner marks, uppercase filled label, fading header rule, title spacing, and stacked body. Use the asset's responsive rules and shared theme tokens. A `.code-example` can appear inside `.special-block__body`; for that case also read [code examples](code-examples.md).

## Question forms

Use `special-block--question` with the form inside `.special-block__body`. Keep the form's `aria-labelledby`, `autocomplete="off"`, submit button, and `.quiz__summary` live region. Clone the entire matching form structure from the asset so inputs, feedback, labels, and state icons stay connected. Give cloned inputs and feedback new unique IDs and update their references.

- **Multiple choice:** use `form.quiz[data-quiz="multiple"]`, a fieldset and legend, `.quiz__options`, and checkbox-based `.quiz-option` elements. Each option carries its own `data-correct="true"` or `"false"`, label, icon, and explanation. Correctness means every correct option is selected and every incorrect option is unselected. After submission, reveal explanations for all options, distinguish correct-selected, correct-missed, incorrect-selected, and incorrect-skipped states, disable inputs, and change the action to Reset.
- **Text answer:** use `form.quiz[data-quiz="text"][data-answer][data-hint][data-success]`, an explicitly labelled `.quiz__input`, `.quiz__answer-row`, and feedback. Set `data-hint` and `data-success` on each form to its own plain-text explanations in the lesson language; the shared runtime reads those attributes. It compares the input string exactly with `data-answer`, without trimming, case folding, or numeric conversion. Explain the expected format in the prompt. An incorrect answer shows a hint and allows another attempt; a correct answer disables the input and changes the action to Reset.

After multiple-choice submission, border color indicates correctness and border style indicates selection, alongside the existing state icons and accessible labels:

| Answer state | Border color | Border style |
| --- | --- | --- |
| Correct and selected | `--answer-correct-color` | `solid` |
| Correct and missed | `--answer-correct-color` | `dashed` |
| Incorrect and selected | `--answer-incorrect-color` | `solid` |
| Incorrect and skipped | `--answer-incorrect-color` | `dashed` |

Apply `border-style: dashed` to `.quiz[data-checked="true"] .quiz-option:has(input:not(:checked))`. Keep the base `solid` border before submission and restore it when result state is cleared on reset, reload, or history restoration.

The standalone sample's text-answer handler contains feedback about `score = 10` and `score + 5`. For a lesson, replace those sample explanations with per-form `data-hint` and `data-success` attributes and use the shared runtime's handler. Keep user input out of `innerHTML`.

## Session and reset behavior

Question answers, selections, results, and completion state live only in the current page's memory. Do not persist them in local storage, session storage, cookies, URLs, or a backend. Theme persistence is independent and may remain.

Run the reset initializer on every page load and on `pageshow` when restoring from the back/forward cache, as the asset does. A reload, new visit, or cache restoration begins a fresh attempt. Initial markup must contain unchecked, enabled inputs and no prefilled answer or completed-state attributes; `form.reset()` restores those initial defaults.

Reset clears selections and text input, checked/completed state, visible feedback, icons, status announcements, and invalid-state markers; it enables inputs and restores the Answer action. Preserve the asset's focus behavior after checking and resetting.

Shuffle answer options on initialization and every reset using the asset's Fisher-Yates implementation. Move whole `.quiz-option` nodes so correctness and explanations travel with their inputs. Shuffle choices within each question, not the lesson sections or question order. A random shuffle can legitimately produce the previous order; do not require every result to differ.

## Completion checks

Check every changed question's answer key and explanations. Exercise multiple-choice success and failure, including a missed correct option and a selected incorrect option. For text questions, check an exact answer, an incorrect answer, and whitespace mismatch; verify retry and per-question hints. After reset, reload, and history restoration, confirm that inputs and result state are cleared and that shuffling is invoked for choice questions. Use controlled randomness or repeated runs to assess shuffling rather than declaring failure after one unchanged order. Confirm keyboard operation and feedback announcements.

For multiple-choice styling changes, verify all four border states in light and dark themes on wide and narrow screens. Confirm that unselected options become dashed only after submission, selected options stay solid, and clearing result state restores the base border style.
