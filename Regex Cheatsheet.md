# 🧠 **Regular Expressions (Regex) Cheat Sheet**

## 🧱 Basic Syntax

| Symbol  | Meaning                              |                     |
| ------- | ------------------------------------ | ------------------- |
| `/.../` | 📝 Start and end of regex expression |                     |
| \`      | \`                                   | 🔀 Alternation (OR) |
| `()`    | 🧰 Grouping                          |                     |

---

## 🎯 Position Matching

| Symbol | Meaning                            |
| ------ | ---------------------------------- |
| `^`    | 🔝 Start of string/line            |
| `\A`   | ⛳ Start of string only             |
| `$`    | 🔚 End of string/line              |
| `\Z`   | 🧷 End of string only              |
| `\b`   | 🚧 Word boundary                   |
| `\B`   | 🚫 Not a word boundary             |
| `\<`   | 🚪 Start of word (in some engines) |
| `\>`   | 🚪 End of word (in some engines)   |

---

## 🔤 Character Classes

| Symbol | Meaning                               |
| ------ | ------------------------------------- |
| `\s`   | ␣ Whitespace (space, tab, newline)    |
| `\S`   | ❎ Not whitespace                      |
| `\w`   | 🔤 Word character (a-z, A-Z, 0-9, \_) |
| `\W`   | 🚫 Not a word character               |
| `\d`   | 🔢 Digit (0–9)                        |
| `\D`   | ❌ Not a digit                         |
| `\x`   | 🧮 Hexadecimal digit                  |
| `\O`   | 0️⃣ Octal digit                       |

---

## ✨ Special Characters

| Symbol | Meaning                  |
| ------ | ------------------------ |
| `\n`   | ↩️ Newline               |
| `\r`   | ⏎ Carriage return        |
| `\t`   | ↹ Tab                    |
| `\v`   | ⬇ Vertical tab           |
| `\f`   | 📄 Form feed             |
| `\xxx` | 🎲 Octal character (xxx) |
| `\xhh` | 🧬 Hex character (hh)    |

---

## 🧮 Groups and Ranges

| Symbol   | Meaning                        |                      |
| -------- | ------------------------------ | -------------------- |
| `.`      | ❓ Any character except newline |                      |
| \`(a     | b)\`                           | 📌 Either `a` or `b` |
| `(…)`    | 🧩 Capturing group             |                      |
| `(?:…)`  | 🧳 Non-capturing group         |                      |
| `[abc]`  | 🔤 Match a, b, or c            |                      |
| `[^abc]` | 🚫 Not a, b, or c              |                      |
| `[a-z]`  | 🔡 Lowercase a to z            |                      |
| `[A-Z]`  | 🔠 Uppercase A to Z            |                      |
| `[0-9]`  | 🔢 Digits 0 to 9               |                      |

> ✅ *Ranges are inclusive.*

---

## 🧲 Quantifiers

| Symbol  | Meaning                  |
| ------- | ------------------------ |
| `*`     | ♾️ 0 or more times       |
| `+`     | ➕ 1 or more times        |
| `?`     | ❓ 0 or 1 time            |
| `{3}`   | 3️⃣ Exactly 3 times      |
| `{3,}`  | ➕ 3 or more times        |
| `{3,5}` | 🎯 Between 3 and 5 times |

> ⚠️ *Quantifiers are greedy by default. Add `?` to make them non-greedy.*

---

## 🧷 Escape Sequences

| Symbol    | Meaning                                           |
| --------- | ------------------------------------------------- |
| `\`       | 🔐 Escape next character                          |
| `\Q...\E` | 🧾 Quote (treat as literal) between `\Q` and `\E` |

---

## 🔁 String Replacement References

| Symbol  | Meaning                   |
| ------- | ------------------------- |
| `$1`    | 1️⃣ First captured group  |
| `$2`    | 2️⃣ Second captured group |
| `$n`    | 🔢 nth captured group     |
| ``$` `` | ⬅️ Text before the match  |
| `$'`    | ➡️ Text after the match   |
| `$+`    | ⏬ Last captured group     |
| `$&`    | 🔄 Entire matched string  |

> 🧪 *Some engines use `\1`, `\2`, etc. instead of `$1`, `$2`.*

---

## 🔍 Assertions

| Symbol             | Meaning               |                |
| ------------------ | --------------------- | -------------- |
| `(?=...)`          | 👀 Lookahead          |                |
| `(?!...)`          | 🚫 Negative lookahead |                |
| `(?<=...)`         | 🕵️‍♂️ Lookbehind     |                |
| `(?<!...)`         | ❌ Negative lookbehind |                |
| `(?>...)`          | ⛔ Atomic group        |                |
| \`(?(condition)yes | no)\`                 | 🤔 Conditional |
| `(?# comment )`    | 💬 Inline comment     |                |

---

## 📚 POSIX Character Classes

| Symbol       | Meaning                    |
| ------------ | -------------------------- |
| `[:upper:]`  | 🔠 Uppercase letters       |
| `[:lower:]`  | 🔡 Lowercase letters       |
| `[:alpha:]`  | 🅰 All letters             |
| `[:alnum:]`  | 🔢 Letters and digits      |
| `[:digit:]`  | 🔢 Digits only             |
| `[:xdigit:]` | 🧮 Hexadecimal             |
| `[:punct:]`  | ✒️ Punctuation             |
| `[:blank:]`  | ␣ Space and tab            |
| `[:space:]`  | 🧼 All whitespace          |
| `[:cntrl:]`  | 🎛️ Control chars          |
| `[:graph:]`  | 🖼️ Printable (no space)   |
| `[:print:]`  | 🖨️ Printable (with space) |
| `[:word:]`   | 🔠 Word characters (`\w`)  |

---

## ⚙️ Pattern Modifiers / Flags

| Flag | Meaning                              |
| ---- | ------------------------------------ |
| `g`  | 🌍 Global (match all)                |
| `i`  | 🆎 Case-insensitive                  |
| `m`  | 📄 Multiline (`^`/`$` match lines)   |
| `s`  | ☝️ Single-line (`.` matches newline) |
| `x`  | ✍️ Allow comments/spacing            |
| `e`  | 🧠 Evaluate replacement              |
| `U`  | 💤 Make quantifiers lazy (ungreedy)  |

---

## 🆚 Greedy vs Non-Greedy

| Pattern | Type                                       |
| ------- | ------------------------------------------ |
| `.*`    | ⚠️ Greedy – match as much as possible      |
| `.*?`   | ✅ Non-Greedy – match as little as possible |

🔗 [More on Stack Overflow](https://stackoverflow.com/questions/3075130/what-is-the-difference-between-and-regular-expressions)
