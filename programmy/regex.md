# regEx

[https://cheatography.com/davechild/cheat-sheets/regular-expressions/](https://cheatography.com/davechild/cheat-sheets/regular-expressions/)

## Anchors <a href="#title_5_160" id="title_5_160"></a>

| ^   | Start of string, or start of line in multi-line pattern |
| --- | ------------------------------------------------------- |
| \A  | Start of string                                         |
| $   | End of string, or end of line in multi-line pattern     |
| \Z  | End of string                                           |
| \b  | Word boundary                                           |
| \B  | Not word boundary                                       |
| \\< | Start of word                                           |
| \\> | End of word                                             |

## Character Classes <a href="#title_5_161" id="title_5_161"></a>

| \c | Control character  |
| -- | ------------------ |
| \s | White space        |
| \S | Not white space    |
| \d | Digit              |
| \D | Not digit          |
| \w | Word               |
| \W | Not word           |
| \x | Hexade­cimal digit |
| \O | Octal digit        |

## POSIX <a href="#title_5_162" id="title_5_162"></a>

| \[:upper:]  | Upper case letters             |
| ----------- | ------------------------------ |
| \[:lower:]  | Lower case letters             |
| \[:alpha:]  | All letters                    |
| \[:alnum:]  | Digits and letters             |
| \[:digit:]  | Digits                         |
| \[:xdigit:] | Hexade­cimal digits            |
| \[:punct:]  | Punctu­ation                   |
| \[:blank:]  | Space and tab                  |
| \[:space:]  | Blank characters               |
| \[:cntrl:]  | Control characters             |
| \[:graph:]  | Printed characters             |
| \[:print:]  | Printed characters and spaces  |
| \[:word:]   | Digits, letters and underscore |

## Assertions <a href="#title_5_163" id="title_5_163"></a>

| ?=          | Lookahead assertion       |
| ----------- | ------------------------- |
| ?!          | Negative lookahead        |
| ?<=         | Lookbehind assertion      |
| ?!= or ?\<! | Negative lookbehind       |
| ?>          | Once-only Subexp­ression  |
| ?()         | Condition \[if then]      |
| ?()\|       | Condition \[if then else] |
| ?#          | Comment                   |



## Quanti­fiers <a href="#title_5_164" id="title_5_164"></a>

| \* | 0 or more | {3}   | Exactly 3 |
| -- | --------- | ----- | --------- |
| +  | 1 or more | {3,}  | 3 or more |
| ?  | 0 or 1    | {3,5} | 3, 4 or 5 |

Add a ? to a quantifier to make it ungreedy.

## Escape Sequences <a href="#title_5_2316" id="title_5_2316"></a>

| \\ | Escape following character |
| -- | -------------------------- |
| \Q | Begin literal sequence     |
| \E | End literal sequence       |

"­Esc­api­ng" is a way of treating characters which have a special meaning in regular expres­sions literally, rather than as special charac­ters.

## Common Metach­ara­cters <a href="#title_5_169" id="title_5_169"></a>

| ^ | \[ | .  | $  |
| - | -- | -- | -- |
| { | \* | (  | \\ |
| + | )  | \| | ?  |
| < | >  |    |    |

The escape character is usually \\

## Special Characters <a href="#title_5_165" id="title_5_165"></a>

| \n   | New line            |
| ---- | ------------------- |
| \r   | Carriage return     |
| \t   | Tab                 |
| \v   | Vertical tab        |
| \f   | Form feed           |
| \xxx | Octal character xxx |
| \xhh | Hex character hh    |



## Groups and Ranges <a href="#title_5_166" id="title_5_166"></a>

| .       | Any character except new line (\n) |
| ------- | ---------------------------------- |
| (a\|b)  | a or b                             |
| (...)   | Group                              |
| (?:...) | Passive (non-c­apt­uring) group    |
| \[abc]  | Range (a or b or c)                |
| \[^abc] | Not (a or b or c)                  |
| \[a-q]  | Lower case letter from a to q      |
| \[A-Q]  | Upper case letter from A to Q      |
| \[0-7]  | Digit from 0 to 7                  |
| \x      | Group/­sub­pattern number "­x"     |

Ranges are inclusive.

## Pattern Modifiers <a href="#title_5_167" id="title_5_167"></a>

| g    | Global match                             |
| ---- | ---------------------------------------- |
| i \* | Case-i­nse­nsitive                       |
| m \* | Multiple lines                           |
| s \* | Treat string as single line              |
| x \* | Allow comments and whitespace in pattern |
| e \* | Evaluate replac­ement                    |
| U \* | Ungreedy pattern                         |

\* PCRE modifier

## String Replac­ement <a href="#title_5_168" id="title_5_168"></a>

| $n  | nth non-pa­ssive group        |
| --- | ----------------------------- |
| $2  | "­xyz­" in /^(abc­(xy­z))$/   |
| $1  | "­xyz­" in /^(?:a­bc)­(xyz)$/ |
| $\` | Before matched string         |
| $'  | After matched string          |
| $+  | Last matched string           |
| $&  | Entire matched string         |

Some regex implem­ent­ations use \ instead of $.

