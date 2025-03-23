# Regex Cookbook – Copyable Patterns for Real-World Use

Who likes writing regex? No one. Who needs regexp? Everyone!
So stop googling regex. Start pasting them. 🧠

Regex Cookbook is a practical, copy-paste-ready cheat sheet of real-world RegExp patterns, organized by category. From emails and dates to URLs, binary formats, colors, and beyond — no theory, just patterns that work.

## Index
- [🧱 Basics](#-basics)
- [📅 Date](#-date)
- [🕒 Time](#-time)
- [⌚ DateTime](#-datetime)
- [📧 Email](#-email)
- [🔒 Password](#-password)
- [📱 Phone Numbers](#-phone-numbers)
- [🌐 URLs](#-urls)
- [📜 File Extensions](#-file-extensions)
- [🧬 Syntax](#-syntax)
- [💻 HTML](#-html)
- [🎨 Colors](#-colors)
- [📦 Data Structures](#-data-structures)
- [🌍 IPs](#-ips)
- [🧍 Text](#-text)
- [🔢 Numbers](#-numbers)
- [💰 Currency & Symbols](#-currency--symbols)
- [💳 Finance](#-finance)
- [📊 Separated Values](#-separated-values-formats)
- [🔖 Slugs & Identifiers](#-slugs--identifiers)
- [🧹 Shortcodes](#-shortcodes--custom-placeholders)
- [🔁 Repetition & Structure](#-repetition--structure)
- [🖥️ Binary & Low-Level](#-binary--low-level-patterns)
- [🇫🇷 France-Specific](#-france-specific)

## 🧱 Basics

### Match numbers
```
/\d+/
```
Matches one or more digits.

- ✅ 123
- ✅ 007
- ❌ abc

---

### Match lowercase letters
```
/[a-z]+/
```
Matches one or more lowercase English letters.

- ✅ hello
❌ HELLO
- ❌ Hello123

---

### Match uppercase letters
```
/[A-Z]+/
```
Matches one or more uppercase English letters.

- ✅ ABC
- ✅ HELLO
- ❌ Hello
- ❌ abc

---

### Match punctuation
```
/[.,;:!?'"(){}\[\]-]+/
```
Matches one or more punctuation characters.

- ✅ ,!?
- ✅ (hello)
- ❌ hello

---

### Match any Unicode or accented character
```
/[\p{L}]/u
```
Matches any letter from any language (including accents and scripts like Arabic, Cyrillic, etc.).

- ✅ é
- ✅ ç
- ✅ æ
- ✅ أ
- ❌ 123

Requires the Unicode flag `/u`.

---

### Match emojis
```
/[\p{Emoji}]/u
```
Matches any emoji character.

- ✅ 😊
- ✅ 💡
- ✅ 🏄‍♂️
- ❌ Hello

Requires modern engines (JS ES2018+). Not all engines support `\p{Emoji}`.

---

### Match specific word or phrase
```
/\b(hello|hi|hey)\b/i
```
Matches exactly “hello”, “hi”, or “hey” as whole words, case-insensitive.

- ✅ hello
- ✅ Hi
- ✅ HEY
- ❌ helloo
- ❌ ohhey

Add or remove words inside `(word1|word2|...)` to adjust.

## 📅 Date

### Date in format YYYY-MM-DD
```
/^\d{4}-\d{2}-\d{2}$/
```
Matches a date formatted as year-month-day (ISO 8601).

- ✅ 2023-12-25
- ✅ 1999-01-01
- ❌ 12-25-2023
- ❌ 2023/12/25

Change `-` to `/` or `.` if your date format uses different separators.

---

### Date in format MM/DD/YYYY
```
/^(0[1-9]|1[0-2])\/(0[1-9]|[12]\d|3[01])\/\d{4}$/
```
Matches dates like month/day/year.

- ✅ 12/25/2023
- ✅ 01/01/2000
- ❌ 25/12/2023
- ❌ 1/1/2000

This forces 2-digit month and day. Use `\/?` if the slash is optional.

## 🕒 Time

### 24-hour time (HH:MM)
```
/^([01]\d|2[0-3]):[0-5]\d$/
```
Matches a valid time in 24-hour format.

- ✅ 09:30
- ✅ 23:59
- ❌ 24:00
- ❌ 9:5

To allow optional leading zeros, change `[01]\d` to `\d{1,2}`.

---

### 12-hour time with AM/PM
```
/^(0?[1-9]|1[0-2]):[0-5]\d\s?(AM|PM)$/i
```
Matches 12-hour time with AM or PM suffix.

- ✅ 10:45 AM
- ✅ 1:30 pm
- ❌ 13:00 PM
- ❌ 10:75 AM

The `i` at the end makes it case-insensitive.

## ⌚ DateTime

### ISO 8601 datetime
```
/^\d{4}-\d{2}-\d{2}T\d{2}:\d{2}:\d{2}(?:\.\d+)?(?:Z|[\+\-]\d{2}:\d{2})$/
```
Full ISO datetime with timezone.

- ✅ 2023-10-12T14:48:00Z
- ✅ 2023-10-12T14:48:00+02:00
- ❌ 2023-10-12 14:48:00

---

### Match Unix timestamp (10 digits)
```
/^\d{10}$/
```
Matches a 10-digit timestamp in seconds.

- ✅ 1617181723
- ❌ 161718 (too short)
- ❌ 2023-01-01

## 📧 Email

### Basic email pattern
```
/^[\w.-]+@[\w.-]+\.[a-zA-Z]{2,}$/
```
Matches standard email addresses.

- ✅ user@example.com
- ✅ john.doe@mail.co.uk
- ❌ user@@example.com
- ❌ user@.com

To restrict domains (e.g. only .com), replace `\.[a-zA-Z]{2,}` with `\.com`.

## 🔒 Password

### Complex password
```
/^(?=.*[0-9])(?=.*[A-Z])(?=.*[a-z])(?=.*\W*)(?!.*\s).{8,16}$/
```
Accepts at least one number, one uppercase letter, one lowercase letter, and zero or more special characters, with a length between 8 to 16 characters.

- ✅ Password1?
- ✅ *pwD123!:?+
- ❌ password
- ❌ my super secure password

Change the numbers in `{8,16}` to adjust length. Use `{8,}` for "8 or more characters".

## 📱 Phone Numbers

### Basic international phone number
```
/^\+\d{1,3}[\s.-]?\(?\d+\)?[\s.-]?\d+[\s.-]?\d+$/
```
Matches common international formats.

- ✅ +33 6 12 34 56 78
- ✅ +1 (123) 456-7890
- ❌ 06.12.34.56.78 (without +)

### Match international prefix (e.g. +33, +1, +867)
```
/^\+(\d{1,4})/
```
Extracts the international prefix from a phone number starting with `+`.

- ✅ +33 6 12 34 56 78 → captures `33`
- ✅ +1 (123) 456-7890 → captures `1`
- ❌ 0033 6 12 34 56 78

---

### Match phone numbers with either + or 00 prefix
```
/^(?:\+|00)\d{1,4}[\s.-]?\d+([\s.-]?\d+)*$/
```
Matches international numbers that start with either `+` or `00`, followed by digits.

- ✅ +33 6 12 34 56 78
- ✅ 0033 6 12 34 56 78
- ✅ +1-123-456-7890
- ❌ 06 12 34 56 78

Use `(?:\+|00)` to support both styles for international dialing.

## 🌐 URLs

### Generic URL (http/https with subdomains and path)
```
/https?:\/\/([\w.-]+)\.[a-z]{2,}(\/[\w\-._~:\/?#[\]@!$&'()*+,;=]*)?/i
```
Matches URLs with http or https, optional subdomain and path.

- ✅ https://example.com
- ✅ http://blog.example.co.uk/path/to/page
- ❌ ftp://example.com

To allow ports (e.g. `:3000`), add `(:\d+)?` after the domain.

---

### Domain only (no protocol, no path)
```
/^[\w-]+\.[a-z]{2,}$/
```
Matches only the domain.

- ✅ google.com
- ✅ example.co.uk
- ❌ http://google.com
- ❌ google

---

### HTTPS domain only (no subdomain)
```
/^https:\/\/[a-z0-9-]+\.[a-z]{2,}$/
```
Matches only https full domains without subdomains.

- ✅ https://example.com
- ❌ https://sub.example.com
- ❌ http://example.com

---

### FTP URL
```
/^ftp:\/\/[\w.-]+\.[a-z]{2,}(\/.*)?$/
```
Matches FTP links with optional path.

- ✅ ftp://files.example.com
- ✅ ftp://example.com/folder/file.txt
- ❌ http://example.com

---

### IP address with http/https
```
/^https?:\/\/(\d{1,3}\.){3}\d{1,3}(\/.*)?$/
```
Matches an IP-based URL.

- ✅ http://192.168.0.1
- ✅ https://127.0.0.1/index.html
- ❌ 192.168.0.1

---

### Localhost with port
```
/^https?:\/\/localhost:\d{2,5}(\/.*)?$/
```
Matches localhost with port and optional path.

- ✅ http://localhost:3000
- ✅ https://localhost:8080/admin
- ❌ localhost:3000

## 📜 File Extensions

### File with extension
```
/^[\w,\s-]+\.[A-Za-z]{2,4}$/
```
Basic filename with extension.

- ✅ file.txt
- ✅ my photo.jpeg
- ❌ file

---

### Image file
```
/^.+\.(jpg|jpeg|png|gif|bmp|svg)$/i
```
Common image formats.

- ✅ logo.png
- ✅ photo.JPG
- ❌ file.pdf

---

### PDF file
```
/^.+\.pdf$/i
```

- ✅ document.pdf
- ❌ document.docx

---
## 🧬 Syntax

### flatcase
```
/^[a-z]+$/
```
All lowercase, no separator.

- ✅ hello
- ❌ Hello
- ❌ helloWorld

---

### UPPERFLATCASE
```
/^[A-Z]+$/
```
All uppercase, no separator.

- ✅ HELLO
- ❌ Hello
- ❌ HELLO_WORLD

---

### camelCase
```
/^[a-z]+(?:[A-Z][a-z]*)+$/
```
Starts lowercase, words joined with uppercase letters.

- ✅ helloWorld
- ❌ HelloWorld
- ❌ hello_world

---

### UpperCamelCase (PascalCase)
```
/^(?:[A-Z][a-z]+)+$/
```
Each word starts with a capital letter.

- ✅ HelloWorld
- ❌ helloWorld
- ❌ Hello_World

---

### snake_case
```
/^[a-z]+(_[a-z]+)+$/
```
Lowercase words separated by underscores.

- ✅ hello_world
- ✅ foo_bar_baz
- ❌ Hello_World
- ❌ helloWorld

---

### SCREAMING_SNAKE_CASE
```
/^[A-Z]+(_[A-Z]+)+$/
```
All uppercase with underscores.

- ✅ HELLO_WORLD
- ❌ Hello_World
- ❌ hello_world

---

### Camel_Snake_Case
```
/^([A-Z][a-z]+_)+[A-Z][a-z]+$/
```
PascalCase segments separated by underscores.

- ✅ Hello_World_Example
- ❌ hello_world
- ❌ HelloWorld

---

### kebab-case
```
/^[a-z]+(-[a-z]+)+$/
```
Lowercase words separated by hyphens.

- ✅ hello-world
- ❌ Hello-World
- ❌ helloWorld

---

### SCREAMING-KEBAB-CASE
```
/^[A-Z]+(-[A-Z]+)+$/
```
Uppercase words separated by hyphens.

- ✅ HELLO-WORLD
- ❌ hello-world

---

### Train-Case
```
/^([A-Z][a-z]+-)+[A-Z][a-z]+$/
```
PascalCase segments separated by hyphens.

- ✅ Hello-World-Example
- ❌ HelloWorld
- ❌ hello-world

## 💻 HTML

### Match content inside HTML tags
```
/>([^<]+)</
```
Captures the content between tags.

- ✅ `<b>Hello</b>` → `Hello`
- ❌ `<b></b>`

---

### Match full HTML tag with content
```
/<(\w+)([^>]*)>(.*?)<\/\1>/
```
Captures entire tag with content.

- ✅ `<div class="x">Hello</div>`
- ❌ `<img src="x.jpg"/>`

---

### Match self-closing tag
```
/<\w+[^>]*\/>/
```
Matches self-closing HTML tags.

- ✅ `<img src="image.jpg" />`
- ✅ `<br/>`
- ❌ `<div>Hello</div>`

---

### Match HTML comment
```
/<!--[\s\S]*?-->/
```
Matches HTML comments.

- ✅ `<!-- This is a comment -->`

---
### HTML tag pair without nesting
```
/<(\w+)[^>]*>([^<]*)<\/\1>/
```
Simple tag pair with content, not nested.

- ✅ `<b>Hello</b>`
- ❌ `<b><i>Bold</i></b>`

## 🎨 Colors

### Hexadecimal color (#RGB or #RRGGBB)
```
/^#(?:[0-9a-fA-F]{3}){1,2}$/
```
Matches 3- or 6-digit hexadecimal color codes.

- ✅ #fff
- ✅ #FFFFFF
- ✅ #123abc
- ❌ #abcd
- ❌ 123456

---

### rgb(r, g, b)
```
/^rgb\(\s*(\d{1,3})\s*,\s*(\d{1,3})\s*,\s*(\d{1,3})\s*\)$/
```
Matches an `rgb()` color with values 0–255.

- ✅ rgb(255, 0, 127)
- ✅ rgb( 0 , 255 , 64 )
- ❌ rgb(300,0,0)
- ❌ rgba(255,0,0,0.5)

---

### rgba(r, g, b, a)
```
/^rgba\(\s*(\d{1,3})\s*,\s*(\d{1,3})\s*,\s*(\d{1,3})\s*,\s*(0|1|0?\.\d+)\s*\)$/
```
Matches an `rgba()` color with alpha 0–1.

- ✅ rgba(255, 0, 127, 0.5)
- ✅ rgba(0, 0, 0, 1)
- ❌ rgba(0, 0, 0)
- ❌ rgba(255,255,255,1.5)

---

### hsl(hue, sat%, light%)
```
/^hsl\(\s*(\d{1,3})\s*,\s*(\d{1,3})%\s*,\s*(\d{1,3})%\s*\)$/
```
Matches `hsl()` values where hue is 0–360, sat/light are percentages.

- ✅ hsl(120, 100%, 50%)
- ✅ hsl(0,0%,0%)
- ❌ hsl(400,100,50)
- ❌ hsl(120, 50, 50)

---

### oklch(L C H)
```
/^oklch\(\s*([\d.]+)\s+([\d.]+)\s+([\d.]+)(?:\s*\/\s*([\d.]+))?\s*\)$/
```
Matches OKLCH format with optional alpha (`/0.5`).

- ✅ oklch(0.628 0.12 281.07)
- ✅ oklch(0.628 0.12 281.07 / 0.5)
- ❌ oklch(0.628, 0.12, 281.07)
- ❌ oklch(0.6 0.2)

---

### cmyk(c, m, y, k)
```
/^cmyk\(\s*(\d{1,3})%\s*,\s*(\d{1,3})%\s*,\s*(\d{1,3})%\s*,\s*(\d{1,3})%\s*\)$/
```
Matches CMYK color values with percentages for cyan, magenta, yellow, and black.

- ✅ cmyk(0%, 100%, 100%, 0%)
- ✅ cmyk(25%, 10%, 0%, 80%)
- ❌ cmyk(100, 0, 0, 0) (missing `%`)
- ❌ cmyk(0%, 0%, 0%)

---

### Pantone code (e.g. Pantone 300 C)
```
/^Pantone\s?(\d{3,4})(\s?[A-Z]{1,2})?$/
```
Matches Pantone codes with optional suffix like `C`, `U`, etc.

- ✅ Pantone 300
- ✅ Pantone 300 C
- ✅ Pantone 485U
- ❌ Pantone red
- ❌ Pantone 30A

---

### RAL Classic (e.g. RAL 3020)
```
/^RAL\s?(\d{4})$/
```
Matches classic RAL color codes.

- ✅ RAL 3020
- ✅ RAL3003
- ❌ RAL red
- ❌ RAL 123

💡 You can extend this to RAL Design (e.g. `RAL 210 50 20`) with:
```
/^RAL\s(\d{3})\s(\d{2})\s(\d{2})$/
```

- ✅ RAL 210 50 20
- ❌ RAL 2105020
- ❌ RAL 210-50-20

---

## 📦 Data Structures

### Match basic JSON object
```
/^\{\s*"[^"]+"\s*:\s*.+\}$/
```
Matches a flat JSON object with a single key-value pair.

✅ {"name": "Jean"}
❌ {"name": "Jean", "age": 30} (multi-pair, nested)
❌ name: "Jean"

Use a proper parser for nested or valid JSON.

### Match XML tag with content
```
/<(\w+)[^>]*>(.*?)<\/\1>/
```
Matches any XML element with opening and closing tags.

- ✅ `<data>Hello</data>`
- ✅ `<tag attr="x">Value</tag>`
- ❌ `<data />`

---

### Match YAML key-value line
```
/^\s*[\w.-]+\s*:\s*.+$/
```
Matches a basic YAML key and value on a single line.

- ✅ name: Jean
- ✅ user_id: 12345
- ❌ - item: value (list format)
- ❌ name "Jean"

## 🌍 IPs

### IPv4
```
/^(\d{1,3}\.){3}\d{1,3}$/
```
Matches standard IPv4 format.

- ✅ 192.168.0.1
- ✅ 8.8.8.8
- ❌ 999.999.999.999 (invalid range, but matches)

For stricter check, validate each octet is <= 255 programmatically.

---
### IPv4 with port
```
/^(\d{1,3}\.){3}\d{1,3}:\d+$/
```
Matches an IPv4 address followed by a port (e.g. `:8080`).

- ✅ 127.0.0.1:3000
- ✅ 192.168.1.10:80
- ❌ 192.168.1.10
- ❌ localhost:3000

---
### Match IPv4 CIDR block
```
/^(\d{1,3}\.){3}\d{1,3}\/\d{1,2}$/
```
Matches IPv4 blocks in CIDR notation.

- ✅ 192.168.0.0/24
- ✅ 10.0.0.0/8
- ❌ 192.168.0.1
- ❌ /24

---

### IPv6
```
/^([a-fA-F0-9]{1,4}:){7}[a-fA-F0-9]{1,4}$/
```
Matches full-form IPv6 addresses.

- ✅ `2001:0db8:85a3:0000:0000:8a2e:0370:7334`
- ❌ `::1` (shortened form not supported by this regex)

---

## 🧩 Text

### No unicode characters (letters, numbers, basic punctuation)
```
/^[\x00-\x7F]+$/
```
Accepts only basic ASCII characters.

- ✅ Bank Statement 10
- ❌ Déclaration bancaire
- ❌ Statement💸

---

### No unicode + no digits
```
/^[A-Za-z\s.,;:!?'"()-]+$/
```
Only allows ASCII letters and punctuation.

- ✅ Bank Statement
- ❌ Statement 10
- ❌ Résumé

---

### No emojis
```
/^(?!.*[\u{1F600}-\u{1F64F}]).*$/u
```
Rejects any string containing emojis in the emoticon range.

- ✅ Hello world
- ❌ Hello 😊

Use different unicode ranges for other emoji types.

---
## 🔢 Numbers
### Only digits
```
/^\d+$/
```
Only numbers.

- ✅ 123456
- ❌ 123 456
- ❌ 12a34

---
### Decimal with dot
```
/^\d+(\.\d+)?$/
```
Matches numbers with optional decimal.

- ✅ 12
- ✅ 12.34
- ❌ 12,34

To use comma instead of dot, replace `\.` with `,`.

---
### Positive or negative integer
```
/^-?\d+$/
```
Matches whole numbers, including negative ones.

- ✅ 42
- ✅ -15
- ❌ 3.14
- ❌ 1,000

---

### Decimal with optional minus sign
```
/^-?\d+(\.\d+)?$/
```
Matches decimal numbers with optional minus sign.

- ✅ -12.34
- ✅ 5.0
- ✅ 42
- ❌ 5,0

---

### Two decimals (basic, no symbols)
```
/^\d+(?:\.\d{2})?$/
```
Matches prices with optional 2 decimal places.

- ✅ 10
- ✅ 10.00
- ❌ 10.0
- ❌ 10.000

Change `\.` to `,` for locales using comma separators.

### Negative numbers in accounting style with parentheses
```
/^\(\d{1,3}(?:[.,]?\d{3})*(?:[.,]\d+)?\)$/
```
Matches negative numbers written in parentheses, like `(1,234.56)` or `(1234,56)`. Accepts both `.` and `,` for decimal or thousand separators.

- ✅ (123)
- ✅ (1,234.56)
- ✅ (1234,56)
- ✅ (1.234,99)
- ❌ -123
- ❌ ( 123 )
- ❌ (1234.567.89)

💡 To force dot or comma for decimal, replace `[.,]` by `\.` or `,`.
💡 If you don’t use thousands separators, simplify to:
```
/^\(\d+(?:[.,]\d+)?\)$/
```

## 💰 Currency & Symbols

### Match currency symbol only
```
/[\$\€\£\¥\₽\₹]/
```
Matches standalone currency symbols.

- ✅ $
- ✅ €
- ✅ £
- ✅ ¥
- ❌ 100
- ❌ 10€

Add or remove symbols inside the brackets to customize.

---

### Match currency with leading symbol (e.g. $100, £ 100.83)
```
/[\$\€\£\¥\₽\₹]\s?\d+(?:[.,]\d{1,2})?/
```
Matches currencies like `$100`, `£ 100.83`, with or without space between symbol and number. Accepts comma or dot as decimal.

- ✅ $100
- ✅ € 100,50
- ✅ £99.99
- ❌ 100€

Change `[.,]` to force one decimal separator.
Use `[.,]\d{2}` to force 2 decimals.

---

### Match currency with trailing symbol (e.g. 100€, 1.500,00 €)
```
/\d+(?:[.,]\d{1,2})?\s?[\$\€\£\¥\₽\₹]/
```
Matches formats like `100€`, `1.500,00 €`, with optional space.

- ✅ 10€
- ✅ 99.99€
- ✅ 1 000,00 €
- ❌ $100

---

### Match percentage (e.g. 20%, 20 %, 20.5%, 20,5 %)
```
/\d+(?:[.,]\d+)?\s?%/
```
Matches percentages with or without decimal and space.

- ✅ 20%
- ✅ 20 %
- ✅ 20.5%
- ✅ 20,5 %
- ❌ %20

## 💳 Finance

### Match IBAN (general structure)
```
/^[A-Z]{2}\d{2}[A-Z0-9]{11,30}$/
```
Matches IBAN format (country code, checksum, BBAN).

- ✅ FR7630006000011234567890189
- ✅ DE89370400440532013000
- ❌ FR76 3000 6000 0112... (spaces)

Remove spaces before testing. Specific formats vary by country.

---

### Match BIC/SWIFT
```
/^[A-Z]{6}[A-Z0-9]{2}([A-Z0-9]{3})?$/
```
Matches standard 8 or 11-character SWIFT/BIC codes.

- ✅ BNPAFRPP
- ✅ DEUTDEFF500
- ❌ BICFRPP123456

---

### Match credit card number (basic)
```
/^\d{4}[- ]?\d{4}[- ]?\d{4}[- ]?\d{4}$/
```
Matches 16-digit credit card number with optional spaces or dashes.

- ✅ 4111 1111 1111 1111
- ✅ 4111-1111-1111-1111
- ✅ 4111111111111111
- ❌ 4111-1111-1111

This does not validate via the Luhn algorithm.

---

## 📊 Separated values formats
### CSV (comma-separated values)
```
/^([^,\n]+,)*[^,\n]+$/
```
Matches comma-separated values on one line.

- ✅ a,b,c
- ❌ a,,c
- ❌ a,b,\nc

---

### TSV (tab-separated values)
```
/^([^\t\n]+\t)*[^\t\n]+$/
```
Matches tab-separated values on one line.

- ✅ a\tb\tc
- ❌ a\t\tc

---

### Semi-colon-separated values
```
/^([^;\n]+;)*[^;\n]+$/
```
Matches semicolon-separated values.

- ✅ a;b;c
- ❌ a;;c
- ❌ a;b;\nc

## 🔖 Slugs & Identifiers

### URL slug
```
/^[a-z0-9]+(?:-[a-z0-9]+)*$/
```
Lowercase, hyphenated, SEO-friendly string.

- ✅ hello-world
- ✅ my-article-123
- ❌ Hello_World
- ❌ hello--world

---

### UUID v4
```
/^[0-9a-f]{8}-[0-9a-f]{4}-4[0-9a-f]{3}-[89ab][0-9a-f]{3}-[0-9a-f]{12}$/i
```
Matches a version 4 UUID.

- ✅ 550e8400-e29b-41d4-a716-446655440000
- ❌ 550e8400e29b41d4a716446655440000

---

## 🧩 Shortcodes & Custom Placeholders

### Match content inside `< >` (e.g. `<shortcode-identifier>`)
```
/<([^<>]+)>/
```
Matches anything between `<` and `>`, excluding nested or malformed tags.

- ✅ `<shortcode>`
- ✅ `<user_name>`
- ❌ <<shortcode>>
- ❌ <shortcode

---

### 🔧 How to adapt to other symbols

#### Match `(shortcode)`
```
/\(([^()]+)\)/
```

#### Match `[shortcode]`
```
/\[([^\[\]]+)\]/
```

#### Match `{shortcode}`
```
/\{([^{}]+)\}/
```

#### Match `{{shortcode}}` (double braces)
```
/\{\{([^{}]+)\}\}/
```

#### Match `<-shortcode->`
```
/<\-([^<>]+)\->/
```

---

- `[^...]` ensures the content does not include the delimiters themselves.
- Use `g` (global flag) if you want to match multiple shortcodes in one string.
- Escape characters like `(`, `[`, `{` using `\` inside the regex.

## 🔁 Repetition & Structure

### Repeated character
```
/(.)\1{2,}/
```
Matches 3 or more of the same character in a row.

- ✅ aaa
- ✅ 1111
- ❌ ababab
- ❌ aa

Change `{2,}` to `{N-1,}` to match N repetitions.
Example: `{6,}` matches 7 or more.

## 🖥️ Binary & Low-Level Patterns

### Binary octets (8 bits each)
```
/([01]{8})/g
```
Matches binary data in groups of 8 bits (octets).

- ✅ 0101010101010101 → `01010101`, `01010101`
- ✅ 1111000010101010
- ❌ 0101010 (only 7 bits)

Use with the global flag `/g` to match multiple octets in a stream.

---

### MAC address
```
/^([0-9A-Fa-f]{2}[:-]){5}([0-9A-Fa-f]{2})$/
```
Matches standard MAC addresses with `:` or `-` as separator.

- ✅ 00:1A:2B:3C:4D:5E
- ✅ 00-1A-2B-3C-4D-5E
- ❌ 001A2B3C4D5E
- ❌ 00:1A:2B:3C:4D

---

### Hexadecimal number (without `#`)
```
/\b(?:0x)?[0-9a-fA-F]+\b/
```
Matches raw hex numbers, optionally starting with `0x`, but no `#`.

- ✅ 0x1A3F
- ✅ a4f3
- ✅ DEADBEAF
- ❌ #FF00FF

---

### File size strings (e.g. 5.6 MB)
```
/^\d+(\.\d+)?\s?(B|KB|MB|GB|TB)$/i
```
Matches common file size formats.

- ✅ 100KB
- ✅ 5.6 MB
- ✅ 2048B
- ❌ 5,6MB (comma instead of dot)

---

### Windows file path
```
/^[A-Z]:\\(?:[^\\\/:*?"<>|\r\n]+\\)*[^\\\/:*?"<>|\r\n]*$/i
```
Matches a full Windows-style path.

- ✅ C:\Users\MyFolder\File.txt
- ✅ D:\Backup\2024\Logs\log.txt
- ❌ /usr/local/bin

---

### Unix file path
```
/^\/(?:[^\/\n]+\/)*[^\/\n]+$/
```
Matches a Unix-like full path.

- ✅ /usr/local/bin/python
- ✅ /var/log/nginx/access.log
- ❌ C:\Program Files

---

### Base64-encoded string
```
/^(?:[A-Za-z0-9+\/]{4})*(?:[A-Za-z0-9+\/]{2}==|[A-Za-z0-9+\/]{3}=)?$/
```
Matches a valid Base64-encoded block.

- ✅ TWFu
- ✅ U29tZSBkYXRhIHdpdGggZXF1YWwgcGFkZGluZw==
- ❌ This is not base64

---

### MD5 hash (32 hex chars)
```
/^[a-f0-9]{32}$/i
```
- ✅ d41d8cd98f00b204e9800998ecf8427e
- ❌ too_short_hash

---

### SHA1 hash (40 hex chars)
```
/^[a-f0-9]{40}$/i
```
- ✅ da39a3ee5e6b4b0d3255bfef95601890afd80709
- ❌ 123abc

---

### SHA256 hash (64 hex chars)
```
/^[a-f0-9]{64}$/i
```
- ✅ e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855
- ❌ short_hash

---

### JWT token format
```
/^[A-Za-z0-9-_]+\.[A-Za-z0-9-_]+\.[A-Za-z0-9-_]+$/
```
Matches the 3-part JWT format (header.payload.signature), base64-url encoded.

- ✅ eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0…
- ❌ abc.def
- ❌ ey... (missing part)

This doesn't validate the signature or payload content.

## 🇫🇷 France-Specific

### Match SIREN (9 digits)
```
/^\d{9}$/
```
- ✅ 732829320
- ❌ 732 829 320
- ❌ 1234567890

---

### Match SIRET (14 digits)
```
/^\d{14}$/
```
- ✅ 73282932000074
- ❌ 732 829 320 00074

---

### Match TVA intracommunautaire (French VAT)
```
/^FR[\dA-Z]{2}\d{9}$/
```
- ✅ FR40303265045
- ✅ FRXX732829320
- ❌ FR40303265045123
- ❌ FR40 3032 6504 5

Check rules per country for prefix and control key logic.

## 🤝 Contributing

You're welcome to contribute new patterns to this cheat sheet!

To keep things accessible and consistent, please follow these guidelines:

- Use the existing format for each entry:
  - A **clear title** (`##` level)
  - A **brief, plain-language explanation** of the use case
  - The **regular expression** inside a code block
  - ✅ **Working examples** and ❌ **non-matching examples**
  - A short **tip to adapt or tweak** the expression (when applicable)

Example:

```markdown
### Match UPPERCASE words
```
```
/^[A-Z]+$/
```
```markdown
Matches strings with only uppercase letters.

- ✅ HELLO
- ✅ REGEXP
- ❌ Hello
- ❌ hello123

To allow numbers too, change to `/^[A-Z0-9]+$/`

---
```

- Keep it **simple and free of regex jargon** – this is meant to be readable by everyone
- Make sure every pattern is **tested and working** before submitting
- Pull requests are reviewed and tested before merging – no untested expressions please 🙏

## 📝 License

This project is licensed under the **MIT License**.
Feel free to use, copy, modify, and distribute – contributions welcome!
