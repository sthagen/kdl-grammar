 # Upstream Extract
 
 Bottom of the page at https://raw.githubusercontent.com/kdl-org/kdl/0cdda0b711aa8ed52c1ed870cc6ec81cbec89c7b/SPEC.md from repository https://github.com/kdl-org/kdl
 
 ## Final Grammar Part
 
 ### Whitespace

The following characters should be treated as non-[Newline](#newline) [white
space](https://www.unicode.org/Public/UCD/latest/ucd/PropList.txt):

| Name                 | Code Pt |
|----------------------|---------|
| Character Tabulation | `U+0009`  |
| Space                | `U+0020`  |
| No-Break Space       | `U+00A0`  |
| Ogham Space Mark     | `U+1680`  |
| En Quad              | `U+2000`  |
| Em Quad              | `U+2001`  |
| En Space             | `U+2002`  |
| Em Space             | `U+2003`  |
| Three-Per-Em Space   | `U+2004`  |
| Four-Per-Em Space    | `U+2005`  |
| Six-Per-Em Space     | `U+2006`  |
| Figure Space         | `U+2007`  |
| Punctuation Space    | `U+2008`  |
| Thin Space           | `U+2009`  |
| Hair Space           | `U+200A`  |
| Narrow No-Break Space| `U+202F`  |
| Medium Mathematical Space | `U+205F`  |
| Ideographic Space    | `U+3000`  |

### Newline

The following characters [should be treated as new
lines](https://www.unicode.org/versions/Unicode13.0.0/ch05.pdf):

| Acronym | Name            | Code Pt |
|---------|-----------------|---------|
| CR      | Carriage Return | `U+000D`  |
| LF      | Line Feed       | `U+000A`  |
| CRLF    | Carriage Return and Line Feed | `U+000D` + `U+000A` |
| NEL     | Next Line       | `U+0085`  |
| FF      | Form Feed       | `U+000C`  |
| LS      | Line Separator  | `U+2028`  |
| PS      | Paragraph Separator | `U+2029` |

Note that for the purpose of new lines, CRLF is considered _a single newline_.

## Full Grammar

```
nodes := linespace* (node nodes?)? linespace*

node := ('/-' node-space*)? type? identifier (node-space node-space* node-props-and-args)* (node-space* node-children ws*)? node-space* node-terminator
node-props-and-args := ('/-' node-space*)? (prop | value)
node-children := ('/-' node-space*)? '{' nodes '}'
node-space := ws* escline ws* | ws+
node-terminator := single-line-comment | newline | ';' | eof

identifier := string | bare-identifier
bare-identifier := ((identifier-char - digit - sign) identifier-char* | sign ((identifier-char - digit) identifier-char*)?) - keyword
identifier-char := unicode - linespace - [\/(){}<>;[]=,"]
keyword := boolean | 'null'
prop := identifier '=' value
value := type? (string | number | keyword)
type := '(' identifier ')'

string := raw-string | escaped-string
escaped-string := '"' character* '"'
character := '\' escape | [^\"]
escape := ["\\/bfnrt] | 'u{' hex-digit{1, 6} '}'
hex-digit := [0-9a-fA-F]

raw-string := 'r' raw-string-hash
raw-string-hash := '#' raw-string-hash '#' | raw-string-quotes
raw-string-quotes := '"' .* '"'

number := decimal | hex | octal | binary

decimal := integer ('.' [0-9] [0-9_]*)? exponent?
exponent := ('e' | 'E') integer
integer := sign? [0-9] [0-9_]*
sign := '+' | '-'

hex := sign? '0x' hex-digit (hex-digit | '_')*
octal := sign? '0o' [0-7] [0-7_]*
binary := sign? '0b' ('0' | '1') ('0' | '1' | '_')*

boolean := 'true' | 'false'

escline := '\\' ws* (single-line-comment | newline)

linespace := newline | ws | single-line-comment

newline := See Table (All line-break white_space)

ws := bom | unicode-space | multi-line-comment

bom := '\u{FFEF}'

unicode-space := See Table (All White_Space unicode characters which are not `newline`)

single-line-comment := '//' ^newline+ (newline | eof)
multi-line-comment := '/*' commented-block
commented-block := '*/' | (multi-line-comment | '*' | '/' | [^*/]+) commented-block
```
