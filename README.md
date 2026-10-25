# @versemate/bible-search

Bible search helpers for VerseMate, shared by `verse-mate-web` and
`verse-mate-mobile` so the search box understands references the same way in
both apps.

`ReferenceParser` turns what someone types into Bible references, for the
search box's "Go to" row:

- `John 3:16`, `jn 3.16`, `john 3 16`, `1co 13:4-7`, `Gen 1:1-2:3`, `Jude 3`
- `First John 4`, `I John 4`
- `Ps 23; Isa 53:5` (several references at once)
- typos such as `Revelations` and `Philipians`
- the version's own book names (`Ioan 3:16` in VDC, `1. Mose 1:1` in SCH51)

Two-letter abbreviations ("is", "so") only count when a chapter follows them.
Book names that are also words ("Job", "Acts") still parse as books; the apps
show them as a "Go to" row beside the verse-text results, so the word search
is not lost.

## Usage

```ts
import { ReferenceParser } from '@versemate/bible-search';

const parser = new ReferenceParser(
  books.map((b) => ({ bookId: b.id, name: b.name, chapters: b.chapterCount })),
  { 43: 'Ioan' } // the version's own book names, by book id (optional)
);

parser.parse('jn 3.16');
// [{ bookId: 43, bookName: 'John', chapter: 3, verseStart: 16, display: 'John 3:16' }]

parser.parse('Ioan 3:16'); // the same reference
```

Book ids follow the API's `books.book_id` (1 = Genesis … 66 = Revelation). The
parser has no imports and no data of its own: the apps pass in the book list
they already load.

Consumed as a github dependency, pinned to a commit:

```json
"@versemate/bible-search": "github:verse-mate/verse-mate-bible-search#<sha>"
```

Both bundlers (Vite, Metro) resolve the TypeScript entry directly.

## Development

```sh
bun test           # the parser's tests
bun run typecheck  # tsc --noEmit
```

A change here reaches an app only when that app bumps its pin: bump web and
mobile together.
