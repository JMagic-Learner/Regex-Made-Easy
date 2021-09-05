# Regex Expressions - Finding "Nemo" 

Somewhere along the line as a coder, you've probably heard of Regex Expressions. An regex expression is essentially a search function built into many coding engines that allows the user to find specific text.

## Summary

We are going to search for a specific url in a list of urls. Think of trying to search through a list of websites and thier corresponding passwords.

Take this following website urls. This is pretty standard plaintext.

TARGET URL (What needs to be searched via REGEX)

https://www.ZenithHighlight.com
https://www.facebook.com
https://www.sample.edu
http://www.vertex123.gov

DUMMY TARGET/CODE (Used to test false positives)

com.facebook.www//:https
http://www.vertex.jello
1213$$$vvv.jt3456.$$%hs


`/^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

however in Regex Expression, it'll look like this,


^(https|http)\W\W\W[a-zA-z]\w*\W[a-zA-Z]\w*\W[a-zA-z]\w*

Use the Ctrl-F (Search) and click the .* option in VS code. The .* function allows the search to use regex expressions to find matching strings.

Now the above Regex expression probably looks absolute trash, but the following definitions and tutorials will help you understand how the above code matches the target url.

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Classes](#character-classes)
- [The OR Operator](#the-or-operator)
- [Flags](#flags)
- [Character Escapes](#character-escapes)

## Regex Components

Regex expressions are comprised of multiple components, that help define and narrow the search criteria. You can search for all encompassing constraints or select a specific character or character sequence.

Examples;
Single Characters 
Wild Cards
Bracket Expression
Groups
Escapes
Anchors


#### Sample Regexs ####

The sample REGEX we will be discussing is a simple search of any URL.
`(http|https)\W\W\W[a-zA-z0-9]\w*\W\w*[a-zA-z]`

We will discuss how to generate this string step by step. Throughout the process, we will dicuss the relevancy of each component being added to the REGEX expression.

### Anchors

Anchors are search parameters that define the start/end string.

Target Expression        REGEX
vidY                     [a-zA-z]*Y
H213i{}()                ^H.*\)$

.       - Any Character Except New Line
^       - Beginning of a String
$       - End of a String

As well all know, complete URLs either start with https or http. Don't worry, you will learn how to do "or" statements in this tutorial.

For right now, lets start simple.

Lets say that we want to search a url. Lets begin with this REGEX 

`^https:` - Putting this in the regex search will immediately seach the above urls in the summary section. Notice how the "^" key targets the beginning of the string. In the DUMMY code section, you will see none of them highlighted.

We can also target the end of a string with "$". Lets test this out by typing out `com$`. Once you test this, you can find urls ending in "com". However, you will notice that the url ending in edu.

As you can see, while anchors can be used to find the beginning and ending string, they are quite limited in general scope.

For right now, lets keep `^https:` as a starter REGEX expression.


### Quantifiers

Quantifiers are constraints that denote how many times a particular string character appears in the target search.

For example,

TARGET STRING       REGEX EXPRESSION
a                   = a
aa                  = a+ or a{2}
aaa                 = a{3}
aa22fiftyone51      = a+2+[a-zA-z]*\d*

Quantifiers:
*       - 0 or More
+       - 1 or More
?       - 0 or One
{3}     - Exact Number
{3,4}   - Range of Numbers (Minimum, Maximum)

With the starter code '^https:', lets add a way to find the slash marks after https:

we can denote "//" as '/{2}' or '/+'

So far the regex code can either be;

`^https:/{2}`
or
`^https:/+`

As you can see, there are many ways to denote duplicate sequential characters


### Grouping Constructs

( )     - Group

Groups is a method to capture multiple characters in a single constraint.
You might be asking, why in the world would I need to group characters when you can type raw text to search for character sequences?

Groups allow you to generalize search terms so you can encompass a greater search pattern.

Lets expand on our existing code `^https:/{2}`

So far we know that after the double "//", there usually comes a string or group of characters to signifiy the "WWW".

Lets add onto the existing code like so `^https:/{2}(w{3}}`.
You're thinking, why in the world would we do this? `^https:/{2}www` or ``^https:/{2}w{3}` also works.

The reason is what happens if there is a custom lead url as well as instead of WWW?
You would then need to find a way to search for the custom lead url and www, just like you would need to find a way to account for https vs http and .com vs edu vs gov."

### Bracket Expressions

Bracket expressions are constraints that help define one or more character strings that are adjacent to one another. In fact, they can help define multiple types of character strings at once.

take for instance this sample code;

1y1y1y = [1y1y1y]

[^a-zA-z] this regex code excludes all character strings.


[]      - Matches Characters in brackets
[^ ]    - Matches Characters NOT in brackets

starter code: `^https:/{2}(w{3})`

So far we have (w{3}) to denote "www". Right now, we can define sequential duplicated character strings. Lets add a "." (period) after to account for the period after the "www". 

starter code `^https:/{2}(w{3}).`

Here is when we add a bracket []

`^https:/{2}(w{3}).[   ]`

Remember, brackets can affect multiple types of characters. Unlike groups, which target a specific set, brackets address the categories.

think of {} as looking for rice, while [] can look for rice or Grains

For example, you can type out ZenithHighlight. Or you can type in [a-zA-z] to seach for all character strings. Lets check out character classes in the next section

### Character Classes

Think of character classes as a category of characters that you are searching for. 
for example, the vast majority of text in this md is of the text "class". Character classes are found via using brackets [ ] 

Character classes is special notation for matching cateogires of characters.

Examples
 [a-z]  Searches for a single character in  lowercase text.
 [A-Z]  Searches for a single character in all uppercase text
 [0-9]  Searches for numerical characters
 0[xX][A-Fa-f0-9]+ searches for CSS hexadecimal numbers


 A character class is specific. Tr[ue]

 sample code `^https:/{2}(w{3}).[a-zA-z0-9]\w*`

 The aboe sample code section; `[a-zA-z0-9]` allows us to search for any character, any digit after the "www.". The "\w*" then notates that we are taking a succession, or sequence of characters that fullfil the search criteria of any character/any digit.

### The OR Operator

|       - Either Or

If you've been following along the tutorial so far, you'd probably remember that near the beginning of this tutorial we had to address how to find capture both https and http search values.

If we had used the above class contraint [a-zA-Z0-9]\w* to search for https or http, this regex expression would've found any string sequence in front of a non-digit/non-character. Instead, we can narrow down the starting search values by using the "|" expression. 

First, we have to use a group () to associate https and http. Add the | operator in between https and http to get the this following starter code;

starter code: `^(http|https:/{2}(w{3}).[a-zA-z0-9]\w*`

using this above code now captures the http://www.vertex.gov target expression.

### Flags

Flags are operators that add utility to regex expressions;

Lets take a look at some flags;

`i` - Denoted after a slash, the `i` operator will search for values , ignoring case sensitivity.

For example \hello\i would match 
HELLO, 
Hello, 
HeLlO, 
heLLO

`g` - Would allow the search to find all search values containing the parameter. By default, only the first search value would appear.

`m` - Allows the search value to cover parameters that are broken in two lines.

`s` - Gives the search function the capabality to newline character

`u` - Enables full unicode support for REGEX EXPRESSIONS

`y` - Allows search functions to find the position of parameters

### Character Escapes

Backlashes in the regex expression are "escapes". They are used to find meta characters.

Metacharacters are: . [] {} () \ / ^ $ | ? ! * + - 

For example, lets say you are trying to find the following string:

[ENCASED]

This would be in regex:
\[[a-zA-z]*\]

The \[ denotes that you are looking for the "opening bracket [".
the [a-zA-z]* denotes that you are looking for all word letters irregdless of capitalization.
The \] denotes that yuo are looking specifically for the "closing brackets ]"

In fact, the above regex would also find any single words encased in brackets.

[Like]
[This]

\d      - Digit (0-9)
\w      - Word Character (a-z, A-Z, 0-9, _)
\s      - Whitespace (space, tab, newline)

\b      - Word Boundary
\B      - Not a Word Boundary

\S      - Not Whitespace (space, tab, newline)
\D      - Not a Digit (0-9)
\W      - Not a Word Character

lets take the starting code `^(http|https:/{2}(w{3}).[a-zA-z0-9]\w*`

We know now that the "." can be expressed as `.` , `\W`. lets go ahead and add this value to account for the "."

`^(http|https:/{2}(w{3}).[a-zA-z0-9]\w*\W`

Now lets account for the .edu, .gov, .com variations in urls.

We could do a hard code `(edu|gov|com)`, or `[a-zA-z0-9]\w*`

Using the latter would actually hit a dummy code. The former narrows the values of the search to a valid URL.

`^(http|https):/{2}(w{3}).[a-zA-z0-9]\w*\W(edu|gov|com)`

## Author

A short section about the author with a link to the author's GitHub profile (replace with your information and a link to your profile)
