# Matching Url Regex: How Does It Work?

To the untrained eye, regular expressions (regex) can look like a bunch of complicated gibberish. But as it turns out, each character has its own unique function in the expression. 

## Summary

The regex /^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/ checks for matching urls. Let's break it down to its core components.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [OR Operator](#or-operator)
- [Character Classes](#character-classes)
- [Flags](#flags)
- [Grouping and Capturing](#grouping-and-capturing)
- [Bracket Expressions](#bracket-expressions)
- [Greedy and Lazy Match](#greedy-and-lazy-match)
- [Boundaries](#boundaries)
- [Back-references](#back-references)
- [Look-ahead and Look-behind](#look-ahead-and-look-behind)

## Regex Components

### Anchors

Anchors can be found by the ^ and \$ characters. The ^ appears at the beginning of the expression and tells the expression that whatever it's looking at must begin with what comes after the character. The \$ at the end of the expression says that whatever it's looking at must end with what comes before that character. In the expression above, we can see ^ at the beginning and \$ at the end, signifying that the the url given must be an exact match.

### Quantifiers

Quantifiers can be complicated. Quantifiers can be found by the characters * + ? and { } <br>
For example: <br><br>
- test* will match a string that has "tes" followed by at least 0 t's
- test+ will match a string that has "tes" followed by at leas 1 t
- test? will match a string that has "tes" followed by either 0 t's or 1 t
- test{x} will match a string that has "tes" followed by exactly x amount of t's
- test{x,} will match a string that has "tes" followed by at least x amount of t's
- test{x,y} will match a string that has "tes" followed by at least x amount of t's but no more than y amount of t's
- te(st)* will match a string that has "te" followed by at least 0 copies of the sequence st
- te(st){x,y} will match a string that has "te" followed by at least x copies of the sequence st but no more than y copies of the sequence st 
<br><br>
In the example above, we can see [a-z\.]{2,6} this includes a bracket expression, which we'll get to later. But the {2,6} after it signifies that the url will match if it has between 2 and 6 of whatever is in the bracket expression.

### OR Operator

Or operatiors can be found with | or []. For example, te(s|t) will match a string that has "te" followed by s OR t. te[st] will do the same without capturing the value in the brackets.

### Character Classes

Character classes tell the engine to look for a specific character, following the classification. There are many different kinds of character classes, but here are the most common ones: <br><br>
- \d matches with a single digit
- \w matches with a single alphanumeric character (also matches with underscores)
- \s matches with whitespace (spaces, tabs, line breaks, etc.)
- . matches with any single character

Along with these classes, each one offers its own inverse class:<br><br>
- \D matches with a single NON-digit character
- \W matches with a single NON-alphanumeric character
etc.<br><br>
In our example, we can see [\/\w \.-] theres a lot happening here but we can see the \w and \. signifies that it will match with an alphanumeric character and any single character after that. NOTE: the characters "^.[$()|*+?{\\" must all be prefixed with a backslash (\\) to be used literally, as they all have special meanings in regex

### Flags

Flags are used to give the regex engine special instructions on how to read strings provided.<br>
- g (global) will not return after the first match, and will keep searching for matches after the previous match was found until the end of the string
- m (multi-line) can be useful if you're checking multiple strings, but giving the engine one long string with all your data. With this flag, \^ and \$ will match the start and end of a line, rather than the entire string
- i (insensitive) will make the expression case-INsensitive, meaning the expression /TEST/i will match with tEsT

### Grouping and Capturing

You can "group" and "capture" parts of strings with (). You can use this when you need to exctract data from said string.<br>
- te(st) will capture a group with the value st
- te(?:st) WON'T be captured in a group. putting ?: before the values of the group will disable capturing
- te(?\<name>st) will capture st from the string and assign it the name of "name". This can enable you to store this data in an object, the name being the keys and captured value being the value.

### Bracket Expressions

Bracket expressions make it easier to match a character logically based off a rule set.<br>
- [test] will match with a string that has t OR e OR s OR t. This is fundamentally the same as using t|e|s|t
- [a-f] will match with a string that has any letter from a to f
- [a-fA-F0-9] is a way to match with a single hexadecimal digit. It is made up of 3 ranges: any lowercase letter from a to f, any uppercase letter from A to F, and any digit from 0 to 9
- [0-9]% will match with a character from 0 to 9 before a %
- You can also use the \^ character to make it an inverse expression. [^a-zA-Z] would match with a string that DOES NOT have any lowercase letter from a to z or any uppercase letter from A to Z
<br><br>
We briefly saw a bracket expression in an earlier example. Let's look at that one again. [a-z\.] is looking for any character from a to z, or any single character

### Greedy and Lazy Match

A greedy match can be seen with \* \+ and {}. So they try to match with as much as possible with the provided text.<br>
- If provided "Test test \<div>test\</div> test", the selector \<.+> will select the div tags and everything inside it (it will match with \<div>test\</div>). In order to just capture the div tags without the text (lazy match), add a \? at the end of the selector within the brackets: \<.+?> will only capture \<div>\</div>

### Boundaries

Boundaries can be seen with \b. When an expression is wrapped in a boundary, it will only match where one side is a word character and the other side is NOT:<br>
- \btest\b will match with test, but not with testtest, or Test, atesta, etc.<br>
- It comes with an inverse as well. \Btest\B will only match if the provided pattern is fully surrounded by word characters: it will match with atesta, but not atest, testa, test, etc.

### Back-references

You can put \1, \2, \3, etc. to match the same text that was matched by the specified capturing group:<br>
- ([test])\1 will match tt from "testt"
- ([test])([in])\2\1 will match "tiit" from "testiit"
- (?\<name>[test])\k\<name> will match "ss" from "tesst", as it matches with the same text as captured by the referenced name. "ss" and "tt" would be matched if provided "tessttest"

### Look-ahead and Look-behind

You can use \?= to match only if the pattern is followed by a specified pattern. t(\?=e) will look for the letter t that is followed by the letter e, but it will not return e in the match. You can also use \?<= to check if the pattern follows the specified pattern. e(\?<=t) will look for the letter e that follows the letter t, but the letter t will not be returned in the match.<br>
This operator also comes with an inverse method. t(\?!e) will find the letter t that IS NOT followed by the letter e, and e(\?<!t) will find the letter e that IS NOT following the letter t.

## Author

A short section about the author with a link to the author's GitHub profile (replace with your information and a link to your profile)
