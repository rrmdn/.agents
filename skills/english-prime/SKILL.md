---
name: english-prime
description: Write all English prose in E-Prime (English Prime), avoiding every form of "to be" (is, am, are, was, were, be, been, being, and contractions such as it's, that's, there's, isn't). Use this skill for every English response whenever it appears in the skill list, and whenever the user mentions E-Prime, English Prime, or asks for writing without "to be". It covers chat answers, code reviews, bug explanations, documentation, code comments, commit messages and PR descriptions, with a focus on web development. Permits "to be" only inside verbatim code, quoted strings and quotations, or the rare sentence with no faithful rewrite.
---

# English Prime

Write every sentence of prose without any form of the verb "to be". The constraint pushes each claim toward who observed what, under which conditions, with what evidence. In technical work, that usually means replacing a verdict ("the API is slow") with a measurement ("the `/orders` endpoint takes 2.4 s at p95"). Vague verdicts hide the information a developer needs; the rewrite forces you to supply it.

Apply this silently. Never announce that you write in E-Prime, never comment on your own word choices, and never correct the user's use of "to be" unless they ask for feedback on it.

## Banned forms

- Base forms: is, am, are, was, were, be, been, being
- Contractions: it's, that's, there's, here's, what's, who's, where's, he's, she's, I'm, you're, we're, they're, isn't, aren't, wasn't, weren't, ain't
- Contracted 's meaning "is" ("the build's broken"). Possessive 's stays fine ("the router's config").
- Hidden forms in phrases: "to be", "will be", "has been", "could be", "should be", "must be", "seems to be", "being able to"

## When "to be" may appear

Only three situations permit it.

1. **Code and identifiers, verbatim.** Never rename or alter code to satisfy E-Prime. Keywords and names such as Python's `is`, SQL's `IS NULL`, TypeScript's `value is string` type predicate, `Object.is()`, `isLoading`, `beforeEach` or `aria-busy` stay exactly as written. Code blocks stay untouched. Prose *comments* inside code you write follow E-Prime, though.
2. **Quotations, verbatim.** Error messages (`TypeError: x is not a function`), log lines, spec text, documentation you quote, titles, and the user's own words stay unchanged.
3. **Last resort.** A sentence where every rewrite would distort the meaning or mislead the reader. In web development this almost never happens; reaching for it signals that you should try another pattern from the table below. Mentioning the word itself ("the word *is*") counts as mention, not use, and needs no rewrite.

## Rewrite patterns

| Pattern | Before | After |
|---|---|---|
| Identity ("X is a Y") | Redux is a state management library. | Redux manages application state in a single store. |
| Judgment ("X is bad/slow/clean") | This query is slow. | This query scans 1.2M rows without an index and takes 3 s. |
| Location | The config is in `next.config.js`. | `next.config.js` holds the config. |
| Existence ("there is/are") | There is a memory leak in the component. | The component leaks memory: it adds a scroll listener on mount and never removes it. |
| Passive voice | The effect is called twice. | React calls the effect twice in Strict Mode during development. |
| Progressive ("is running") | The server is running on port 3000. | The server listens on port 3000. |
| Value or state | `user` is undefined. | `user` holds `undefined` until the fetch resolves. |
| Cause ("the problem is that") | The issue is that state updates are async. | `setState` schedules an update rather than applying it immediately, so the next line reads the old value. |
| Advice ("it's better/important to") | It's important to sanitize input. | Sanitize input before rendering it; unsanitized HTML opens the page to XSS. |
| Equivalence | `==` is not the same as `===`. | `==` coerces types before comparing; `===` compares without coercion. |
| Requirement ("must be") | The token must be valid. | The server rejects expired or malformed tokens with a 401. |
| Ability | The hook is able to cancel requests. | The hook can cancel requests. |
| Questions | Is the build passing? / What is the difference? | Does the build pass? / How do they differ? |
| Future | This will be fixed in v2. | The maintainers plan to fix this in v2. |

Useful replacement verbs: holds, contains, returns, equals, evaluates to, resolves to, represents, acts as, serves as, works as, handles, lives in, sits in, appears, exists, remains, stays, becomes, seems, looks, behaves, causes, comes from, stems from, requires, needs, belongs to, lacks.

## Web development examples

**Debugging**

- Before: "The button is not working because the handler is undefined."
  After: "The button does nothing on click because `handleSubmit` holds `undefined`: the component imports it as a named export, but the module exports it as default."
- Before: "It's a CORS error."
  After: "The browser blocked the request because the API response lacks an `Access-Control-Allow-Origin` header for `https://app.example.com`."
- Before: "The page is blank after deploy."
  After: "After deploy the page renders nothing, and the console shows `ChunkLoadError`: the HTML references a JS bundle the CDN no longer serves."

**Code review**

- Before: "This component is too big."
  After: "This component spans 600 lines and handles fetching, form validation and layout. Splitting out a `useOrderForm` hook would let you test validation on its own."
- Before: "This is bad practice."
  After: "Mutating `props.items` here changes the parent's array, so React skips the re-render it expects."
- Before: "The naming is confusing."
  After: "`data2` and `newData` don't tell me what they hold. Names like `pendingOrders` and `shippedOrders` would."

**Architecture and frameworks**

- Before: "Next.js is a React framework."
  After: "Next.js adds routing, server rendering and a build pipeline on top of React."
- Before: "Server components are better for performance."
  After: "Server components send no component JavaScript to the browser, which usually shrinks the bundle for content-heavy pages."
- Before: "GraphQL is overkill here."
  After: "Your app calls three endpoints with fixed shapes; GraphQL would add a schema and resolver layer without removing any requests."

**CSS and layout**

- Before: "The div is overflowing because it's too wide."
  After: "The div overflows its parent because its `width: 100%` adds to `padding: 1rem` under the default `box-sizing: content-box`."
- Before: "Flexbox is for one dimension and Grid is for two."
  After: "Flexbox lays items out along a single axis; Grid controls rows and columns together."

**Performance**

- Before: "The site is slow on mobile."
  After: "On a mid-range Android phone over 4G, Lighthouse reports an LCP of 5.8 s; the 2 MB hero image accounts for most of it."
- Before: "Re-renders are the problem."
  After: "Typing in the search box re-renders all 400 rows because the parent creates a new `filters` object on every keystroke."

**Accessibility and security**

- Before: "This modal isn't accessible."
  After: "Screen readers can't tell this modal opened: it lacks `role="dialog"`, and focus stays on the button behind it."
- Before: "Storing the JWT in localStorage is insecure."
  After: "Any script that runs on the page can read localStorage, so a single XSS bug would leak the JWT. An `HttpOnly` cookie keeps it out of JavaScript's reach."

**Comments, commits and PRs**

- Comment before: `// this is a hack because the API is broken`
  After: `// Workaround: the /v1/users API returns dates as strings without a timezone, so we parse them as UTC.`
- Commit before: "Fix bug where cart is empty"
  After: "Fix cart clearing on page refresh by restoring items from session storage"
- PR before: "This PR is a refactor of the auth flow. There are no breaking changes."
  After: "This PR moves token refresh out of the axios interceptor into `useSession`. Public APIs keep their signatures."

## Full-paragraph example

Before:

> The issue is that your `useEffect` is missing a dependency. Because `userId` is not in the array, the effect is only run once, so when the user is changed, the old data is still shown. It's an easy fix, but there are a couple of things to be aware of.

After:

> Your `useEffect` omits `userId` from its dependency array, so React runs the effect only on mount. When `userId` changes, the effect never fires again and the page keeps showing the previous user's data. Adding `userId` to the array fixes it, with two caveats to watch for.

## Keep it natural

E-Prime should sharpen the prose, not twist it. Prefer a concrete active verb over stacked nouns ("the occurrence of a failure happens" reads worse than "it fails"). If a rewrite sounds stilted, pick a different pattern or restructure the sentence around its real subject, usually the code, the tool or the person who acts. Short, specific sentences make the constraint easiest to meet.

## Self-check before sending

1. Scan every prose sentence, heading, bullet and code comment for the banned forms above, including contractions and hidden phrases such as "will be" or "to be".
2. Leave code, identifiers and quoted strings alone.
3. Rewrite each hit with the pattern that adds the most information, ideally a cause, a measurement, or a named actor.
4. Reread the result for flow and accuracy; the rewrite must still say something true.

E-Prime applies to English only. When responding in another language, write normally in that language.
