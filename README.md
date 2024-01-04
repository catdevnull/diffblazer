<h1 align='center'>Diffblazer</h1>

<p align='center'>A super fast diffing utility for HTML and Plaintext</p>

This work is based on [htmldiff.net](https://github.com/Rohland/htmldiff.net). Originally, I forked [htmldiff-javascript](https://github.com/jibin2706/htmldiff-javascript) (which is a fork of [htmldiff-js](https://github.com/dfoverdx/htmldiff-js)), but now the code has almost completely deviated. A summary of the changes:

1. New faster tokenizer based on [htmlparser2](https://github.com/fb55/htmlparser2)
2. Support for "atomic" tags (i.e. tags that should be considered as 1 token) in the tokenizer
3. Support for diffing nodes based on attributes
4. No such thing as "word" anymore. Everything is a token
5. More efficient match finder (no more unnecessary stripping of attributes from tags)
6. Extensive test suite (thanks to [htmldiff.net](https://github.com/Rohland/htmldiff.net) and [htmldiff.js](https://github.com/idesis-gmbh/htmldiff.js))
7. Removed support for block expressions
8. Tags & class names are now fully configurable
9. ~100x faster (no kidding!)

## Installation

```bash
npm install diffblazer
```

## Usage

```ts
import { diff } from 'diffblazer'

const oldHtml = '<p>hello world</p>'
const newHtml = '<p>hello world!</p>'

diff(oldHtml, newHtml)

// Output: <p>hello world<ins class='diffins'>!</ins></p>
```

### Custom markers

Aside from marking differences using HTML tags, you can also specify your own markers. This can be anything:

```ts
import { diff } from 'diffblazer'

const oldText = 'hello world'
const newText = 'hello beautiful world'

diff(oldText, newText, {
	markers: {
		insert: {
			start: '**',
			end: '**',
		},
	},
})

// Output: hello **beautiful** world
```

## Benchmarks

```
 ✓ benches/html-diff.bench.ts (2) 1212ms
   ✓ benchmark (2) 1210ms
     name                 hz     min     max    mean     p75     p99    p995    p999     rme  samples
   · diffblazer     5,980.63  0.1440  2.2266  0.1672  0.1657  0.3724  0.4760  0.9363  ±1.30%     2991   fastest
   · node-htmldiff    615.75  1.3015  3.3126  1.6240  1.6930  3.0620  3.2030  3.3126  ±2.60%      308


 BENCH  Summary

  diffblazer - benches/html-diff.bench.ts > benchmark
    9.71x faster than node-htmldiff
```

To run these benchmarks yourself:

```
npm run bench
```

## Contributing

If you'd like to contribute to this project, you can do so in the following ways:

1. Report an issue
2. Implement a feature
3. Fix a bug

Before you open a PR, make sure to run the tests, like so:

```
npm run test
```

## License

Copyright © 2024 Abdullah Atta under MIT. [Read full text here.](/LICENSE)
