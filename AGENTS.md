# Repository Guide

## Commands
- Install dependencies with `npm ci` (the committed lockfile is npm lockfile v3).
- Run the full test suite with `npm test`.
- Run a focused test by name with `npx mocha test/test.js --grep "pattern"`.
- There are no configured lint, format, build, or typecheck scripts.

## Structure And Behavior
- This is a single CommonJS package: `index.js` is both the package entrypoint and the complete parser implementation. Its public exports include `getPodcastFromFeed`, `getPodcastFromURL`, `DEFAULT`, `GET`, `CLEAN`, `buildOptions`, and `ERRORS`.
- `test/test.js` is the sole test file; XML fixtures in `test/testfiles/` define parsing, custom-field, ordering, and malformed-feed coverage.
- Remote-feed tests replace the cached `isomorphic-fetch` export and reload `index.js`; keep this cache-reset pattern when changing URL-fetch behavior so tests remain network-free.
- `getPodcastFromURL` follows an `itunes:new-feed-url` redirect recursively. `getPodcastFromFeed` instead parses the supplied feed and emits a warning when that tag is present.
- Parsed fields are cleaned by default; options can select fields, mark required fields, or preserve raw XML-derived values through `uncleaned`.
