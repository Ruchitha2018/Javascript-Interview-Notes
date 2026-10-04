# JavaScript Regular Expressions (Regex)

A **regular expression (Regex)** is a pattern used to **search, match, validate, or replace text**.

### 1. Ways to Create a Regex

| Method      | Example               | Description                    |
| ----------- | --------------------- | ------------------------------ |
| Literal     | `/hello/`             | Most common syntax             |
| Constructor | `new RegExp("hello")` | Useful when pattern is dynamic |

```js
const regex1 = /hello/;
const regex2 = new RegExp("hello");
```


## 2. Common Regex Characters

| Pattern | Meaning                               | Example     | Matches       |
| ------- | ------------------------------------- | ----------- | ------------- |
| `.`     | Any character except line terminators | `/a.c/`     | `abc`, `a1c`  |
| `\d`    | Digit `[0-9]`                         | `/\d/`      | `5`           |
| `\D`    | Non-digit                             | `/\D/`      | `a`           |
| `\w`    | Word character `[A-Za-z0-9_]`         | `/\w/`      | `a`, `5`, `_` |
| `\W`    | Non-word character                    | `/\W/`      | `@`, `-`      |
| `\s`    | Whitespace                            | `/\s/`      | space, tab    |
| `\S`    | Non-whitespace                        | `/\S/`      | `a`           |
| `\b`    | Word boundary                         | `/\bcat\b/` | `cat`         |
| `\B`    | Non-word boundary                     | `/\Bcat/`   | `scat`        |

---

## 3. Character Classes

| Pattern    | Meaning                        | Example      |
| ---------- | ------------------------------ | ------------ |
| `[abc]`    | `a`, `b`, or `c`               | `/[abc]/`    |
| `[^abc]`   | Anything except `a`, `b`, `c`  | `/[^abc]/`   |
| `[a-z]`    | Lowercase range                | `/[a-z]/`    |
| `[A-Z]`    | Uppercase range                | `/[A-Z]/`    |
| `[0-9]`    | Digit range                    | `/[0-9]/`    |
| `[a-zA-Z]` | Uppercase or lowercase letters | `/[a-zA-Z]/` |

Example:

```js
/[aeiou]/
```

Matches any vowel.


## 4. Quantifiers

Quantifiers specify **how many times** something should occur.

| Quantifier | Meaning             | Example    |
| ---------- | ------------------- | ---------- |
| `*`        | 0 or more           | `/a*/`     |
| `+`        | 1 or more           | `/a+/`     |
| `?`        | 0 or 1              | `/a?/`     |
| `{n}`      | Exactly `n`         | `/a{3}/`   |
| `{n,}`     | At least `n`        | `/a{3,}/`  |
| `{n,m}`    | Between `n` and `m` | `/a{2,4}/` |

Examples:

```js
/ab+/
// ab, abb, abbb...

/ab*/
// a, ab, abb...

/ab?/
// a, ab
```


## 5. Anchors

| Pattern | Meaning              | Example    |
| ------- | -------------------- | ---------- |
| `^`     | Start of string/line | `/^Hello/` |
| `$`     | End of string/line   | `/world$/` |

Example:

```js
/^Hello$/
```

Matches only the exact string:

```text
Hello
```


## 6. Groups and Alternation

| Pattern        | Meaning               | Example            |       |       |
| -------------- | --------------------- | ------------------ | ----- | ----- |
| `(abc)`        | Capturing group       | `/(abc)/`          |       |       |
| `(?:abc)`      | Non-capturing group   | `/(?:abc)/`        |       |       |
| `a             | b`                    | `a` OR `b`         | `/cat | dog/` |
| `(?<name>...)` | Named capturing group | `/(?<year>\d{4})/` |       |       |

Example:

```js
/(cat|dog)/
```

Matches either:

```text
cat
dog
```


## 7. Lookaround

| Pattern    | Meaning             |
| ---------- | ------------------- |
| `(?=...)`  | Positive lookahead  |
| `(?!...)`  | Negative lookahead  |
| `(?<=...)` | Positive lookbehind |
| `(?<!...)` | Negative lookbehind |

Example:

```js
/\d+(?= dollars)/
```

In:

```text
100 dollars
```

it matches:

```text
100
```

but not `" dollars"`.


## 8. Regex Flags

| Flag | Name        | Purpose                      |
| ---- | ----------- | ---------------------------- |
| `g`  | Global      | Find all matches             |
| `i`  | Ignore case | Case-insensitive matching    |
| `m`  | Multiline   | `^` and `$` work per line    |
| `s`  | DotAll      | `.` matches line terminators |
| `u`  | Unicode     | Unicode-aware matching       |
| `y`  | Sticky      | Match from `lastIndex`       |
| `d`  | Indices     | Provides match indices       |

Example:

```js
/hello/gi
```

Means:

* `g` → find all matches
* `i` → ignore case

---

## 9. Common Regex Methods

### `test()`

Returns `true` or `false`.

```js
const regex = /^\d+$/;

regex.test("123");
// true

regex.test("abc");
// false
```

### `exec()`

Returns match information or `null`.

```js
const regex = /hello/;

regex.exec("hello world");
```

### String methods

| Method                               | Purpose                         |
| ------------------------------------ | ------------------------------- |
| `str.match(regex)`                   | Returns matches                 |
| `str.matchAll(regex)`                | Returns iterator of all matches |
| `str.search(regex)`                  | Returns first match index       |
| `str.replace(regex, replacement)`    | Replaces matches                |
| `str.replaceAll(regex, replacement)` | Replaces all matches            |
| `str.split(regex)`                   | Splits using regex              |

Example:

```js
"hello world".replace(/world/, "JavaScript");

// "hello JavaScript"
```

---

## 10. Common Interview Patterns

### Only digits

```js
/^\d+$/
```

### Only letters

```js
/^[A-Za-z]+$/
```

### Alphanumeric

```js
/^[A-Za-z0-9]+$/
```

### 10-digit number

```js
/^\d{10}$/
```

### Basic email pattern

```js
/^[^\s@]+@[^\s@]+\.[^\s@]+$/
```

### Starts with `Hello`

```js
/^Hello/
```

### Ends with `world`

```js
/world$/
```


### Interview Summary

> **Regular expressions are patterns used to search, match, validate, extract, and replace text. In JavaScript, regex can be created using literals (`/pattern/`) or `RegExp`, and commonly uses character classes, quantifiers, anchors, groups, and flags.**
