# linebreak
A browser-friendly implementation of the Unicode Line Breaking Algorithm (UAX #14)

This is a [fork](https://github.com/CraigMorton/linebreak) of the [original linebreak package](https://github.com/devongovett/linebreak) which removes the need for node's 'fs' module at build-time and produces bundled code, in both UMD and es2015 modules format, that is ready for use in client-side projects.

## Installation

This fork is published on npm as [@craigmorton/linebreak](https://www.npmjs.com/package/@craigmorton/linebreak). You can install the forked version using `npm install @craigmorton/linebreak`

> Line breaking, also known as word wrapping, is the process of breaking a section of text into lines such that it will fit in the
> available width of a page, window or other display area. The Unicode Line Breaking Algorithm performs part of this process.
> Given an input text, it produces a set of positions called "break opportunities" that are appropriate points to begin a new line.
> The selection of actual line break positions from the set of break opportunities is not covered by the Unicode Line Breaking Algorithm,
> but is in the domain of higher level software with knowledge of the available width and the display size of the text.

This is a JavaScript/CoffeeScript implementation of the
[Unicode Line Breaking Algorithm](http://www.unicode.org/reports/tr14/#SampleCode) for Node.js
(and browsers I guess).  It is used by [PDFKit](http://github.com/devongovett/pdfkit/) for
line wrapping text in PDF documents, but since the algorithm knows nothing about the actual
visual appearance or layout of text, it could be used for other things as well.

## Example

```javascript
var LineBreaker = require('@craigmorton/linebreak');

var lorem = 'lorem ipsum...';
var breaker = new LineBreaker(lorem);
var last = 0;
var bk;

while (bk = breaker.nextBreak()) {
  // get the string between the last break and this one
  var word = lorem.slice(last, bk.position);
  console.log(word);

  // you can also check bk.required to see if this was a required break...
  if (bk.required) {
    console.log('\n\n');
  }

  last = bk.position;
}
```

## Development Notes

In order to use the library, you shouldn't need to know this, but if you're interested in
contributing or fixing bugs, these things might be of interest.

* The `src/classes.trie` file is automatically generated from `LineBreak.txt` in the Unicode
  database by `tools/generate_data.coffee`. It should be rare that you need to run this, but
  you may if, for instance, you want to change the Unicode version. You can run it with `npm run generate-data`

* You can run the tests using `npm test`. They are written using `mocha`, and generated from
  `LineBreakTest.txt` from the Unicode database, which is included in the repository for performance
  reasons while running them. About 150 of the over 6000 tests are currently skipped due to
  implementation differences. It appears that some of the tests may be wrong or use different
  tailoring from the spec.

## License

MIT
