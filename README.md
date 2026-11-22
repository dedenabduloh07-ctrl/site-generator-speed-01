# site-generator-speed-01

A faster drop-in for makefun-site 1.0.0: `solve.js` returns, for every case's
input, exactly the output tree makefun-site 1.0.0's build returns, byte for
byte — with less CPU time. Dependency-free, standard library only (no imports
at all). `main.ts` is the harness driver: it reads every case from
`/input/cases.json`, answers each one alone with `run` from `solve.js`, and
writes `/output/answers.json`.

[![make.fun](https://make.fun/badge/site-generator-speed-01-run-20261122.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![rank](https://make.fun/badge/site-generator-speed-01-run-20261122/dedenabduloh07-ctrl-site-generator-speed-01/rank.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)
[![reward](https://make.fun/badge/site-generator-speed-01-run-20261122/dedenabduloh07-ctrl-site-generator-speed-01/reward.svg)](https://make.fun/bounty/site-generator-speed-01-run-20261122)

## Speed

The same build, with less allocation and fewer passes:

- Markdown is rendered in one pass over the lines, straight into the result
  string, with no intermediate token objects.
- Inline rendering scans natively for the next special character and escapes
  whole plain runs at once, instead of escaping character by character.
- Templates are compiled to straight-line functions (with the tree-walk
  renderer as a fallback when code generation is unavailable): no node-kind
  dispatch per node, and `for` bodies only build a `loop` object when the
  body reads `loop`.
- Slugs take a tight ASCII fast path (no Unicode normalisation) when the text
  is pure ASCII; the full NFKD path is kept for anything else.
- Word counting scans code units against a whitespace table instead of
  building a match array of every word.
- Tag slugs are memoised per build, and regular expressions and escape
  callbacks are hoisted out of hot loops.

## License

MIT. See [LICENSE](LICENSE).
