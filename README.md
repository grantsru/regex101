# Regular Expressions 101
All information from a Udemy tutorial. Course can be found here: https://www.udemy.com/learn-regular-expressions-in-online-regex-course/learn/v4/

## What is a regular expression?
It is a sequence of symbols and characters expressing a string or pattern to be searched for within a longer piece of text.

## Notes:

__Literal__ - taking words in their usual or most basic sense
Characters - a single letter or group of letters

`\b` - Boundary: For exact matching of a string

#### MODES
Different modes change the way that searches are conducted.

Syntax: /regex/__mode__
1. Global = `/regex/g` - all occurrences
2. Case Insensitive = `/regex/i`
3. Single Line = `/regex/s`
4. Multi Line = `/regex/m`
5. No Mode = `/regex/` - first occurrence

#### METACHARACTERS
A character that has special meaning, instead of a literal meaning. You use them in regular expressions to define search criteria and find patterns.

12 Basic single character metacharacters with special meanings:
1. Backslash \ : for escaping special characters
2. Caret ^ : 
3. Pipe |
4. Period .
5. Dollar sign $
6. Question mark ?
7. Asterisk
8. Plus sign
9. Open parenthesis (
10. Close parenthesis )
11. Open square bracket [
12. Open curly bracket {

More metacharacters:
13. \d : matches any decimal digit
14. \D : matches any character other than a decimal digit
15. \w : matches any word character (A-Z, a-z, 0-9)
16. \W : matches any non-word character
17. \s : matches any white-space character
18. \S : matches any non-white-space character
19. . : matches all except new line
20. \t : match a tab character
21. \r : for a carriage return
22. \n : for line feed
23. \f : for form feed
24. \v : for a vertical tab

Brackets are used to specify what characters the regex pattern can match with. `[jc]ar` will only match with "jar" and "car." Matches only one character ("cjar" won't be found).

Hyphens are used to specify a range of characters within a character set. Can only define a range if its in brackets, otherwise its recognized as a literal character. It is case-sensitive.
```regexp
[a-z] - matches all lowercase letters
[A-Z] - matches all capital letters
[0-9] - matches all digits
[a-zA-Z0-9] - matches all numerical and alphabetical characters
```

Carets are used to negate a character set. If I were to add a caret to a set, it will match anything but what is listed in that set.
```regexp
[^abcd] - matches anything other than a, b, c, or d
[^0-9] - matches anything other than digits
```

Metacharacters inside a character set - there are 4 of them. `-^\]` All other metacharacters are literal characters within a character set.

#### Quantifiers
1. `?` - matches the previous element zero or one time
2. `*` - matches the previous element zero or more times
3. `+` - matches the previous element one or more times

```regexp
/colou?r/g
```
This will match both "color" and "colour." The `?` following the `u` allows for it to be optional. `*` and `+` act similarly, but `?` will only allow for an occurrence zero or one times.

```regexp
/Jan(uary)?`g
```
This will match Jan or January.

### Limiting Repetition
By using `{}`, we can limit repetition. For example, `{n}`, with _n_ = any positive integer, we can determine how many characters are acceptable for a match.

```regexp
\d{3} will match 123, 400, 999, but not 12 or 4100
\d{2,} will match 12, 400, 9999, but not 5'
\d{3,5} will match 123, 4100, but not 1, or 123456
```

#### Greedy Expressions
`? + *` are all greedy by default.

Greedy - matches the previous element as many times as possible

Lazy - Matches the previous element as few times as possible

```regexp
<.+>
```
`<h1>Heading</h1>` The above code will match that string three times. The h1 tags, and everything between the first < and last >.

```regexp
<.+?>
```
This will make the regex lazy. The above code would then only have the h1 tags seleceted, not the whole thing. This also is the same as this regex: `<[^>]+>` the difference is, this is 3 steps faster.

#### Anchors
1. `^` - the match must occur at the beginning of string or line
2. `$` - the match must occur at the end of a string or line

These anchor the regex to a certain position, either at the beginning or end of a string/line.

```regexp
/^\w+/g
```
This will find the first word in a line.

```regexp
/\w+$/g
```
This will find the last word in a line.

The above code also works with strings, so you can test letters and characters against single words. This pairs well with multi-line mode.

#### Word Boundaries
1. `\b` - the match must occur on a boundary between a \w (alphanumeric) and a \W (non-alphanumeric) character
2. `\B` - the match must not occur on a \b boundary
3. `\w` - `[a-zA-Z0-9_]`
4. `\W` - `[^\w]`

Three positions:
1. Before the first character in a string if it is a word character
2. After the last character in the string if it is a word character
3. Between two characters in the string where one is a word character and the other is not a word character

```regexp
/\b[a-zA-Z]{4}\b/g
```
The above regex matches words with four letters in a row.

#### Groups
For groups, we use `()`

With the help of a group, you may apply a quantifier to the entire group or to restrict alternation to part of the regex.

`[]` or `{}` cannot be used for grouping. You can also create a numbered group and use it later.

```regexp
/(im)?possible/g
```
This will match both `possible` and `impossible`.

```regexp
/\d{3}-\d{3}-\d{4}/g
```
This will match a hyphenated phone number. The whole match is grouped under group 0.

```regexp
/\d{3}-(\d{3})-(\d{4})/g
```
Regex will name groups numerically from left to right. The first occurrence of a grouped expression is group 1, and so on.

```regexp
/\d{3}-(\d{3})-(\d{4})-\1/g
```
This addition to the end of our expression will use the same expression format as group 1 `(\d{3})`. This is used so we don't have to continue to repeat code throughout our expression.

#### Alternation
Alternation is done with the `|` symbol. Its similar to a character class, but you can use it to match a single regex out of two or more possibilities.

Gray vs. Grey
```regexp
/gr(a|e)y/
```
regex.com vs. regex.net vs. etc.
```regexp
/regex\.(com|net|org|info)/
```

Good to check on file extensions, too. Alternations have the ability to be nested.

#### Assertions - Lookaround
When you want to match an element (word, character or group) if a certain word (element) comes after it or before it.

```regexp
Element to match(?=element)
```
This is the structure of lookarounds.

```regexp
/bill(?=\spaid)/
```
This will match the bill of "bill paid" but not the bill of "bill not paid".

```regexp
/\d+(?=\sUSD)/
```
This will match the 100 of 100 USD, but not the 250 of 250 JPY.

For __negative lookaround__, use the following format:
```regexp
Element to match(?!=element)
```

For __positive lookbehind__, use the following format:
```regexp
Element to match(?<=element)
```

For __negative lookbehind__, use the following format:
```regexp
Element to match(?<!element)
```

#### Date Example
Here are two typical date strings:
- 12/1/2018
- 11-12-2010

To match these dates, you could use the following regex:
```regexp
/\b\d{2}[-\/]\d{1,2}[-\/]\d{4}\b/
```
