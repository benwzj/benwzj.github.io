---
layout: post
title: Regular Expression (RE)
date: 2024-12-27
categories: Python
tags: Python JavaScript RE
toc: 
  - name: What is RE
  - name: RE in JavaScript
  - name: RE in Python
---

## What is RE

A regular expression (shortened as RE or regex or regexp) is essentially a tiny, highly specialized programming language. It is a sequence of characters that specifies a match pattern in text. Usually such patterns are used by string-searching algorithms for "find" or "find and replace" operations on strings, or for input validation. 

The concept of regular expressions began in the 1950s, when an American mathematician formalized the concept of a regular language. They came into common use with Unix text-processing utilities. Different syntaxes for writing regular expressions have existed since the 1980s, one being the POSIX standard and another, widely used, being the **Perl syntax**. 

Regular expressions are supported in many programming languages, including Pythong and JavaScript.

### Why need RE
Regular expressions are particularly useful for defining filters!
Regular expressions are used in search engines, in search and replace dialogs of word processors and text editors, in text processing utilities and in lexical analysis. 


## RE in JavaScript

In JavaScript, regular expressions are objects. 

### JavaScript RegExp Object

JavaScript use RegExp object to implement Regular Expression.
There are some String methods which support regExp. But all the actual implementations come from corresponsed method of RegExp. 

#### create a RegExp Object in two ways:
1. Using a regular expression literal: `let re = /ab+c/;`
2. Or calling the constructor function: `let re = new RegExp('ab+c');`

#### Instance property - lastIndex 
lastIndex is a read/write integer property of RegExp instances that specifies the index at which to start the next match.

Note that lastIndex is not a property of the RegExp prototype but is instead only exposed from RegExp instances.

#### Methods
test() and exec()

### RegExp related Methods

#### RegExp Method - test()
The test() method executes a search for a match between a regular expression and a specified string. Returns true or false.
```js
const str = 'hello world!';
const result = /^hello/.test(str); // true
```

#### RegExp Method - exec()

The `exec()` method executes a search for a match in a string. Returns a result array, or null.

- JavaScript RegExp objects are stateful when they have the global or sticky flags set. They store a lastIndex from the previous match. Using this internally, exec() can be used to iterate over multiple matches in a string of text (with capture groups)
- If the match succeeds, the exec() method returns an array (with extra properties index, input, and if the d flag is set, indices;) and updates the lastIndex property of the regular expression object. 
The returned array has the matched text as the first item, and then one item for each parenthetical capture group of the matched text.
- If your regular expression uses the "g" flag, you can use the exec() method multiple times to find successive matches in the same string. (Using string.matchAll() looks easier)

```js
let myRe = /ab*/g;
let str = 'abbcdefabh';
let myArray;
while ((myArray = myRe.exec(str)) !== null) {
  let msg = 'Found ' + myArray[0] + '. ';
  msg += 'Next match starts at ' + myRe.lastIndex;
  console.log(msg);
}
//Found abb. Next match starts at 3
//Found ab. Next match starts at 9
```

#### String method - match() - matchAll()

#### match()
- The match() method retrieves an array containing matchings in the string against a regular expression.
- Return null if no matches are found.
- Return an Array whose contents depend on the presence or absence of the global (g) flag. 
  - If the g flag is used, all results matching the complete regular expression will be returned, but capturing groups will not. If you want to obtain capture groups and the global flag is set, you need to use RegExp.exec() or String.prototype.matchAll() instead.
  -  If the g flag is NOT used, only the first complete match and its related capturing groups are returned. In this case, the returned item will have additional properties, like group, index, input.  

(with no g flag, str.match() return the same result as RegExp.exec().)

- The implementation of `String.prototype.match` comes from `RegExp.prototype[@@match]`.

#### matchAll()
- The matchAll() method returns an iterator of all results matching a string against a regular expression, including capturing groups.
- The actual implementation comes from `RegExp.prototype[@@matchAll]`.

#### String method - replace() - replaceAll()

replace()
The replace() method returns a new string with some or all matches of a pattern replaced by a replacement. 

- The original string is left unchanged.
- The pattern can be a string or a RegExp, and the replacement can be a string or a function called for each match. 
- If pattern is a string, only the first occurrence will be replaced.
- If pattern is RegExp: 

```js
let re = /(\w+)\s(\w+)/;
let str = 'John Smith';
let newstr = str.replace(re, '$2, $1');
console.log(newstr);  // Smith, John
```

- If a function as the second parameter. the function will be invoked after the match has been performed. The function's result (return value) will be used as the replacement string. 
The arguments to the function are as follows: `match, p1, p2.., offset, string, groups`.

```js
function replacer(match, p1, p2, p3, offset, string) {
  // p1 is non-digits, p2 digits, and p3 non-alphanumerics
  return [p1, p2, p3].join(' - ');
}
let newString = 'abc12345#$*%'.replace(/([^\d]*)(\d*)([^\w]*)/, replacer);
console.log(newString);  // abc - 12345 - #$*%
```

#### replaceAll()
The replaceAll() method returns a new string with all matches of a pattern replaced by a replacement. The pattern can be a string or a RegExp, and the replacement can be a string or a function to be called for each match.

#### String method - search()
The search() method executes a search for a match between a regular expression and this String object.
Return the index of the first match between the regular expression and the given string, or -1 if no match was found.

#### String method - split()
The split() method takes a pattern and divides a String into an ordered list of substrings by searching for the pattern, puts these substrings into an array, and returns the array.

### Regular expression pattern

#### Flag

**Sticky flag ‘y’**: 
The "y" flag indicates that it matches only from the exact index indicated by the lastIndex property of this regular expression in the target string (and does not attempt to match from any later or earlier indexes). 
A regular expression defined as both sticky and global ignores the global flag.

**‘d’ flag**
Generate indices for substring matches.
The "d" flag indicates that the result of a regular expression match should contain the start and end indices of the substrings of each capture group.
```js
const str1 = 'foo bar foo';
const regex1 = new RegExp('foo', 'gd');
console.log(regex1.exec(str1).indices[0]); // Output: Array [0, 3]
console.log(regex1.exec(str1).indices[0]); // Output: Array [8, 11]

‘d’ flag equal to RegExp.prototype.hasIndices
```

**‘u’ flag**
`u` flag indicates a string must be considered as a series of Unicode code points.
For `\p` or `\u{hhhh}` to work, a regular expression must use the u flag.

**‘s’ flag**
‘.’ matches any single character except line terminators: \n, \r, \u2028 or \u2029. 
ES2018 added the s "dotAll" flag, which allows the dot to also match line terminators.
Means that, dot will match all single character when using ‘s’ flag. 

#### Charactar classes

Character classes, [], distinguish kinds of characters such as, for example, distinguishing between letters and digits.

`[abcd], [0-9], [-_.]`: A character class.
Find all the lowcase letters and put them in an Array:
```js
const paragraph = 'The quick mps..';
const regex = /[a-z]/g;
const found = paragraph.match(regex); //Array ["h", "e", "q", "u", "i", "c", "k", "m", "p", "s"]
```
`[^abcd]`: it matches anything that is not enclosed in the brackets. 

`.` : Has one of the following meanings:
1. Matches any single character except line terminators: \n, \r, \u2028 or \u2029. For example, /.y/ matches "my" and "ay", but not "yes", in "yes make my day".
2. Inside a character class, [.], the dot loses its special meaning and matches a literal dot.

`\d`
Matches any digit (Arabic numeral). Equivalent to [0-9].
`\D` : Equivalent to [^0-9]. 

`\w`
Matches any alphanumeric character from the basic Latin alphabet, including the underscore. Equivalent to `[A-Za-z0-9_]`
`[\w-]` is the same as `[A-Za-z0-9_-]`
`\W` : Equivalent to `[^A-Za-z0-9_]`, opposite to `/w`

`\s`	
Matches a single white space character, including space, tab, form feed, line feed, and other Unicode spaces. Equivalent to `[ \f\n\r\t\v\u00a0\u1680\u2000-\u200a\u2028\u2029\u202f\u205f\u3000\ufeff]`.
`\S` : opposite to `/s`

`\xhh` : Matches the character with the code hh (two hexadecimal digits).

`\uhhhh` : Matches a UTF-16 code-unit with the value hhhh (four hexadecimal digits).

`\u{hhhh}` or `\u{hhhhh}` : (Only when the u flag is set.) Matches the character with the Unicode value U+hhhh or U+hhhhh (hexadecimal digits).

`\` : Indicates that the following character should be treated specially, or "escaped". 

`x|y` : Disjunction: Matches either "x" or "y". 

#### Unicode property escapes

`\p{UnicodeProperty}`, `\P{UnicodeProperty}`	

Unicode property escapes RE allows for matching characters based on their Unicode properties. 
To match, for example, emoji characters, or Japanese katakana characters, or Chinese/Japanese Han/Kanji characters, punctuations, letters (even letters from specific languages or scripts), etc.

##### What is Unicode property?
A character is described by several properties which are either binary ("boolean-like") or non-binary. 

Some Unicode properties encompasses many more characters than some character classes (such as \w which matches only latin letters, a to z) but the latter is better supported among browsers. 
The Unicode Standard assigns various properties to each Unicode character and code point.

The properties can be used to handle characters (code points) in processes, like in line-breaking, script direction right-to-left or applying controls. Some "character properties" are also defined for code points that have no character assigned and code points that are labeled like `"<not a character>"`.

Properties are displayed in the following order:
`[code];[name];[gc];[cc];[bc];[decomposition];;;[nv];[bm];[alias];;;;`

#### Boundary-type Assertions

`^` :
Matches the beginning of input. If the multiline flag is set to true, also matches immediately after a line break character. For example, /^A/ does not match the "A" in "an A", but does match the first "A" in "An A".

`$` :
Matches the end of input. If the multiline flag is set to true, also matches immediately before a line break character. 
For example, /t$/ does not match the "t" in "eater", but does match it in "eat".

`\b` : 
Matches a word boundary. Indicate the beginnings and endings of lines and words.
This is the position where a word character is not followed or preceded by another word-character, such as between a letter and a space. 

Examples can help to understand this assertion:
- `/\bm/` matches the "m" in "moon".
- `/oo\b/` does not match the "oo" in "moon", because "oo" is followed by "n" which is a word character.
- `/oon\b/` matches the "oon" in "moon", because "oon" is the end of the string, thus not followed by a word character.
- `/\w\b\w/` will never match anything, because a word character can never be followed by both a non-word and a word character. 
- `/\b\d{4}\b/g`, the first `\b` means start matching at the beginning of a word, the second `\b` means end matching at the end of a word. 

`\B` :
Matches a non-word boundary. This is a position where the previous and next character are of the same type: Either both must be words, or both must be non-words, for example between two letters or between two spaces. The beginning and end of a string are considered non-words. Same as the matched word boundary, the matched non-word boundary is also not included in the match. For example, /\Bon/ matches "on" in "at noon", and /ye\B/ matches "ye" in "possibly yesterday".

#### Look-ahead Look-behind assertion

`x(?=y)`: 
Lookahead assertion, Matches "x" only if "x" is followed by "y". 

`x(?!y)`: 
Negative lookahead assertion, Matches "x" only if "x" is not followed by "y".

`(?<=y)x`: 
Lookbehind assertion, Matches "x" only if "x" is preceded by "y". 

`(?<!y)x`: 
Negative lookbehind assertion, Matches "x" only if "x" is not preceded by "y".

#### Groups and backreferences

Groups group multiple patterns as a whole, and capturing groups provide extra submatch information when using a regular expression pattern to match against a string. Backreferences refer to a previously captured group in the same regular expression.

`(x)`: 
Capturing group: Matches x and remembers the match. 
For example, `/(foo)/` matches and remembers "foo" in "foo bar".
```js
const personList = `First_Name: John, Last_Name: Doe
First_Name: Jane, Last_Name: Smith`;

const regexpNames =  /First_Name: (\w+), Last_Name: (\w+)/mg;
for (const match of personList.matchAll(regexpNames)) {
  console.log(`Hello ${match[1]} ${match[2]}`);
}
> "Hello John Doe"
> "Hello Jane Smith"
```
`matchAll ()` method return capturing groups information and store in the returned array. 
`match[0]` store the matched text. match[1] store the submatched text 1 which refer to the first parentheses. 

Why need capturing group? 
provide sub-matched information. 

You can even name a capturing Group
 `(?<Name>x)`: Name-capturing group

You can match and does not remember the match
 `(?:x)`: Non-capturing group

back reference
`\n`
n, positive number. A back reference to the last substring matching the n parenthetical in the regular expression (counting left parentheses). 
For example, `/apple(,)\sorange\1/` matches "apple, orange," in "apple, orange, cherry, peach".

`\k<Name>`
A back reference to the last substring matching the Named capture group specified by `<Name>`.

#### Quantifiers (Repetitions)

- following ‘x’, just refer to one letter
- Repetitions such as *, + are greedy; 

`x+`: 
Matches the preceding item "x" 1 or more times. Equivalent to {1,}. 
For example, /a+/ matches the "a" in "candy" and all the "a"'s in "caaaaaaandy".

`x*`: 
Matches the preceding item "x" 0 or more times. Equivalent to {0,}.
For example, /bo*/ matches "boooo" in "A ghost booooed" and "b" in "A bird warbled", but nothing in "A goat grunted".

`x?`: 
Matches the preceding item "x" 0 or 1 times. Equivalent to {0,1}.
For example, /e?le?/ matches the "el" in "angel" and the "le" in "angle."
If used immediately after any of the quantifiers *, +, ?, or {}, makes the quantifier non-greedy. (quantifiers are always greedy by default)

`?` following quantifiers example:
```js
const regex1 = /<.*>/
const str1 = 'some <foo> <bar> new </bar> </foo> thing';
console.log(str1.match(regex1)) // > Array ["<foo> <bar> new </bar> </foo>"]
const regex2 = /<.*?>/
console.log(str1.match(regex2)) // Array ["<foo>"]
```

`x{n}`: 
matches exactly "n" occurrences of the preceding item "x". 
For example, /a{2}/ doesn't match the "a" in "candy", but it matches all of the "a"'s in "caandy", and the first two "a"'s in "caaandy".

`x{n,}`: 
matches at least "n" occurrences of the preceding item "x". 
For example, /a{2,}/ doesn't match the "a" in "candy", but matches all of the a's in "caandy" and in "caaaaaaandy".

`x{n,m}`: 
m > n, matches at least "n" and at most "m" occurrences of the preceding item "x". 


### Questions

- Why use string methods to implement RegExp function? Why not use RegExp object methods to complete all function? 
- Why it won’t work for \p (Unicode property escape) without ‘u’ flag?

#### `\uhhhh vs. \u{hhhh}`
`\uhhhh` : hhhh is UTF-16 code-unit value. It works even without u flag.
`\u{hhhh}` or `\u{hhhhh}` : (Only when the u flag is set.) U+hhhh or U+hhhhh is Unicode value.
`u` flag indicates a string must be considered as a series of Unicode code points.


## RE in Python

### RE in Python Overview

Regular expressions are made available through the `re` module.

- Match patterns is the core of RE.
- Python RE patterns follows perl syntax. it is very similar with JavaScript. 
- RE patterns are compiled into a series of bytecodes which are then executed by a matching engine written in C. 
- REs aren’t part of the core Python language.
- No special syntax was created for expressing RE in Python.

#### Unicode string and bytes. 
Both patterns and strings to be searched can be Unicode strings (str) as well as 8-bit strings (bytes). However, Unicode strings and 8-bit strings cannot be mixed: that is, you cannot match a Unicode string with a byte pattern or vice-versa; similarly, when asking for a substitution, the replacement string must be of the same type as both the pattern and the search string.

#### In many cases, using String methods will be better choice than re. 
Strings have several methods for performing operations with fixed strings and they’re usually much faster, because the implementation is a single small C loop that’s been optimized for the purpose, instead of the large, more generalized regular expression engine.
- For Replacing a single fixed string with another one, consider the string `replace()` method first.
- Another common task is deleting every occurrence of a single character from a string or replacing it with another single character. Try string `translate()` first.

### How RE works in Python

The `re` module provides an interface to the regular expression engine, allowing you to compile REs into objects and then perform matches with them. Pattern objects have methods for various operations such as searching for pattern matches or performing string substitutions.

### Classic way to use RE

#### Firstly, Compiling RE to Pattern object
Regular expressions are compiled into pattern objects, which have methods for various operations such as searching for pattern matches or performing string substitutions.

```py
import re
p = re.compile('ab*')
```

RE is handled as string in Python and is passed to `re.complie()`. No special syntax was created for expressing RE in Python. ( There are special syntax for RE in JavaScript, like `/[a-z]/g` )
This keep Python simpler, but has one disadvantage: backslash plague! So it is recommend to use raw string r’rawstring’.

#### Secondly, Use Pattern objects to Perform Matches

### Using module-level Functions 

- You don’t have to create a pattern object and call its methods; The re module also provides top-level functions called match(), search(), findall(), sub(), and so forth. 
- Under the hood, these functions simply create a pattern object for you and call the appropriate method on it. They also store the compiled object in a cache, so future calls using the same RE won’t need to parse the pattern again and again.
- Should you use these module-level functions, or should you get the pattern and call its methods yourself? If you’re accessing a regex within a loop, pre-compiling it will save a few function calls. Outside of loops, there’s not much difference thanks to the internal cache.

### Pattern Object 

#### match()
Return match object. 
Determine if the RE matches Just at the beginning of the string. 
```
>>> m = p.match('tempo')
>>> m
<re.Match object; span=(0, 5), match='tempo'>
```

#### search()
Return None or match object.
Scan through a string, looking for any location where this RE matches. 

#### findall()
Return a list of strings or a list of tuples
Find all substrings where the RE matches, and returns them as a list.
Return all non-overlapping matches of pattern in string, as a list of strings or tuples

#### finditer()
Find all substrings where the RE matches, and returns them as an iterator.

#### split()
String split() only supports splitting by whitespace or by a fixed string.
But re.split() provides much more generality in the delimiters.
```
>>> p = re.compile(r'\W+')
>>> p.split('This is a test, short and sweet, of split().')
['This', 'is', 'a', 'test', 'short', 'and', 'sweet', 'of', 'split', '']
```
If capturing parentheses are used in the RE, then the capturing group values are also returned as part of the list. Compare the following calls:
```
>>> p = re.compile(r'\W+')
>>> p2 = re.compile(r'(\W+)')
>>> p.split('This... is a test.')
['This', 'is', 'a', 'test', '']
>>> p2.split('This... is a test.')
['This', '... ', 'is', ' ', 'a', ' ', 'test', '.', '']
```

#### sub(replacement, string[, count=0])
Returns the string obtained by replacing the leftmost non-overlapping occurrences of the RE in string by the replacement replacement. 
```
>>> p = re.compile('(blue|white|red)')
>>> p.sub('colour', 'blue socks and red shoes')
'colour socks and colour shoes'
```

replacement can be a function:
```
>>> def repl(m):
...     inner_word = list(m.group(2))
...     random.shuffle(inner_word)
...     return m.group(1) + "".join(inner_word) + m.group(3)
>>> text = "Professor Abdolmalek, please report your absences promptly."
>>> re.sub(r"(\w)(\w+)(\w)", repl, text)
'Poefsrosr Aealmlobdk, pslaee reorpt your abnseces plmrptoy.'
```

### Match object
match(), search() return match object. 
Match object methods and attributes:

#### group(([group1, ...]))
Returns one or more subgroups of the match. 
```
>>> m = re.match(r"(\w+) (\w+)", "Isaac Newton, physicist")
>>> m.group(0)       # The entire match
'Isaac Newton'
>>> m.group(1)       # The first parenthesized subgroup.
'Isaac'
>>> m.group(2)       # The second parenthesized subgroup.
'Newton'
>>> m.group(1, 2)    # Multiple arguments give us a tuple.
('Isaac', 'Newton')
```

#### groups(default=None)
Return a tuple containing all the subgroups of the match, from 1 up to however many groups are in the pattern. 
```
>>> m = re.match(r"(\d+)\.(\d+)", "24.1632")
>>> m.groups()
('24', '1632')
```

#### start()
Return the starting position of the match
#### end()
Return the ending position of the match
#### span()
Return a tuple containing the (start, end) positions of the match

### RE Pattern Syntax

metacharacters:
`. ^ $ * + ? { } [ ] \ | ( )`

#### Compilation Flag

- ASCII, A: 
Makes several escapes like \w, \b, \s and \d match only on ASCII characters with the respective property. Corresponds to the inline flag (?a)
- DOTALL, S: 
Make . match any character, including newlines.
- IGNORECASE, I: 
Do case-insensitive matches. (?i)
- LOCALE, L: 
Do a locale-aware match. (not recommend in python3, because of using unicode by default)
- MULTILINE, M: 
Multi-line matching, affecting ^ and $.
- VERBOSE, X (for ‘extended’): 
Enable verbose REs, which can be organized more cleanly and understandably. You can even use comments within a RE.
Corresponds to the inline flag (?x)

#### Character class

- `[` and `]`. They’re used for specifying a character class. 
- Metacharacters (except `\`) are not active inside classes. For example `[$]` can match $.
- You can match the characters not listed within the class by complementing the set. This is indicated by including a '`^`' as the first character of the class. 

```
\d  Matches any decimal digit; this is equivalent to the class [0-9].
\D  [^0-9].
\s   Matches any whitespace character; this is equivalent to the class [ \t\n\r\f\v].
\S  [^ \t\n\r\f\v].
\w  Matches any alphanumeric character; this is equivalent to the class [a-zA-Z0-9_].
\W  [^a-zA-Z0-9_].
```

#### backslash `\`

Perhaps the most important metacharacter is the backslash, `\`.
- You can precede metacharacters with a backslash to remove their special meaning.
- Some of the special sequences beginning with '\' represent predefined sets of characters.

`\w` matches any alphanumeric character. 
If the regex pattern is expressed in bytes, this is equivalent to the class `[a-zA-Z0-9_]`. 
If the regex pattern is a string, `\w` will match all the characters marked as letters in the Unicode database provided by the unicodedata module. 

#### Repeating Things

Repetitions such as * are greedy;
(This part is similar with JavaScript)

Do `*?` work in Python?
It works!

```
>>> s = '<html><head><title>Title</title>'
>>> print(re.match('<.*>', s).group())
<html><head><title>Title</title>
>>> print(re.match('<.*?>', s).group())
<html>
```

#### Assertion
```
| Alternation; 
Crow|Servo will match either 'Crow' or 'Servo', not 'Cro', a 'w' or an 'S', and 'ervo'.
^ Matches at the beginning of lines. 
$ Matches at the end of a line,
\A Matches only at the start of the string. When not in MULTILINE mode, \A and ^ are effectively the same.
\Z Matches only at the end of the string.
\b Word boundary.
\B Another zero-width assertion, this is the opposite of \b
```

#### Grouping
Because Frequently you need to obtain more information than just whether the RE matched or not. you need Grouping.
- Groups are marked by the '(', ')'
- Match object methods all have group 0 as their default
- Example: 

```
>>> p = re.compile('(a(b)c)d')
>>> m = p.match('abcd')
>>> m.group(0)
'abcd'
>>> m.group(1)
'abc'
>>> m.group(2)
'b'
>>> m.group(2,1,2)
('b', 'abc', 'b')
```

- The groups() method returns a tuple containing the strings for all the subgroups, from 1 up to however many there are.

```
>>> m.groups()
('abc', 'b')
```
- Repetition will repeat the whole group.:

```
>>> p = re.compile('(ab)*')
>>> print(p.match('ababababab').span())
(0, 10)
```

- Backreferences in a pattern allow you to specify that the contents of an earlier capturing group must also be found at the current location in the string.

```
>>> p = re.compile(r'\b(\w+)\s+\1\b')
>>> p.search('Paris in the the spring').group()
'the the'
```
Backreferences are very useful when performing string substitutions.

#### Non-capturing Groups
Sometimes you’ll want to use a group to denote a part of a regular expression, but aren’t interested in retrieving the group’s contents. You can make this fact explicit by using a non-capturing group: (?:...), where you can replace the ... with any other regular expression.
```
>>> m = re.match("([abc])+", "abc")
>>> m.groups()
('c',)
>>> m = re.match("(?:[abc])+", "abc")
>>> m.groups()
()
```

Except for not retrieving the content, a non-capturing group behaves exactly the same as a capturing group.

#### Named Groups
The syntax for a named group is one of the Python-specific extensions: `(?P<name>...)`.
```
>>> p = re.compile(r'(?P<word>\b\w+\b)')
>>> m = p.search( '(((( Lots of punctuation )))' )
>>> m.group('word')
'Lots'
>>> m.group(1)
'Lots'
```
It’s obviously much easier to retrieve group.

backreferences can like this:
`(?P=name)` indicates that the contents of the group called name should again be matched at the current point. 
```
>>> p = re.compile(r'\b(?P<word>\w+)\s+(?P=word)\b')
>>> p.search('Paris in the the spring').group()
'the the'
```

#### Lookahead Assertions

(?=...) Positive lookahead assertion.
(?!...) Negative lookahead assertion.

For example,
simple pattern to match a filename and split it apart into a base name and an extension, separated by a .
In news.rc, news is the base name, and rc is the filename’s extension.
The pattern to match this is quite simple:
`.*[.].*$`
Now, consider complicating the problem a bit; what if you want to match filenames where the extension is not bat or exe?
`.*[.](?!bat$|exe$)[^.]*$`

### Questions
 
- JavaScript support `/g` flag, but Python don’t. How to search whole string? 
Python use findall() to search globally. search() just match once. 
- JavaScript support `/u` flag, but Python don’t. How do Python implement unicode? 
- Python support A, X flag, but JavaScript don’t.

