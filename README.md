
# Regex Expressions - Finding "URL" 

Somewhere along the line as a coder, you've probably heard of Regex Expressions. An regex expression is essentially a search function that allows the user to modify the parameters of the search.

## Summary

We are going to search for a specific url in a list of urls. 

Take these following website urls for example. This list is going to be a target sheet for your REGEX expressions.

TARGET URL (What needs to be searched via REGEX)

* https://www.ZenithHighlight.com
* https://www.facebook.com
* https://www.sample.edu
* http://www.vertex123.gov
* https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions

DUMMY TARGET/CODE (Used to test false positives)

* com.facebook.www//:https
* http://www.vertex.jello
* 1213$$$vvv.jt3456.$$%hs

We are first going to generate our own REGEX expression via a step by step walkthrough. 
In addition to this, we are going to take a look at the UWBootcamp REGEX expression, that is more comprehensive.
The UW Bootcamp versioon of the REGEX expression will also be explained along the way.


This is our starter code:

* `^(http|https):/{2}(w{3}|[a-zA-Z0-9]\w*).[a-zA-z0-9]\w*\W(edu|gov|com)$`

This is the UW Bootcamp variation;

* `^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

Use the Ctrl-F (Search) and click the .* option in VS code. The .* function allows the search to use regex expressions to find matching strings.

Now the above Regex expression probably looks absolute gibberish, but the following definitions and tutorials will help you understand how the above code matches the target url.

We will break down the UW REGE. In contrast, we will build up our unique REGEX

## Table of Contents

- [Anchors](#anchors)
- [Quantifiers](#quantifiers)
- [Grouping Constructs](#grouping-constructs)
- [Bracket Expressions](#bracket-expressions)
- [Character Classes](#character-classes)
- [The OR Operator](#the-or-operator)
- [Flags](#flags)
- [Character Escapes](#character-escapes)
- [Ending Regex](#ending-regex)

## Regex Components

Regex expressions are comprised of multiple components, that help define and narrow the search criteria. You can search for all encompassing constraints or select a specific character or character sequence.


The basic REGEX components are comprised of the following;

* Single Characters 
* Wild Cards
* Bracket Expression
* Groups
* Escapes
* Anchors


#### Sample Regexs ####

The sample REGEX we will be discussing is a simple search of any URL.

* `^(http|https):/{2}(w{3}).[a-zA-z0-9]\w*\W$(edu|gov|com)`

We will discuss how to generate this string step by step. Throughout the process, we will dicuss the relevancy of each component being added to the REGEX expression.

### Anchors

Anchors are search parameters that define the start/end string.

Target Expression       H213i{}() 

REGEX                    ^H\w*

* .       - Any Character Except New Line
* ^       - Beginning of a String
* $       - End of a String

As well all know, complete URLs either start with https or http. Don't worry, you will learn how to do "or" statements in this tutorial.

For right now, lets start simple.

Lets say that we want to search a url. Lets begin with this REGEX 

`^https:` - Putting this in the regex search will immediately seach the above urls in the summary section. Notice how the "^" key targets the beginning of the string. In the DUMMY code section, you will see none of them highlighted.

We can also target the end of a string with "$". Lets test this out by typing out `com$`. Once you test this, you can find urls ending in "com". However, you will notice that the url ending in edu.

As you can see, while anchors can be used to find the beginning and ending string, they are quite limited in general scope.

For right now, lets keep `^https:` as a starter REGEX expression.

Our starter code: `^https:`

UW Bootcamp starter code: `^(https?:\/\/)?`

UW Bootcamp starter code explanation:

We first denote the starting search string. Next we include a (if) by usage of a `?`. Basically, if there is a https value, then follow the value with a hardcoded `://`. Secondly, there is another `?` following this denoting a conditional presence.

### Quantifiers

Quantifiers are constraints that denote how many times a particular string character appears in the target search.

For example,

TARGET STRING       REGEX EXPRESSION
* a                   = `a`
* aa                  = `a+ or a{2}`
* aaa                 = `a{3}`
* aa22fiftyone51      = `a+2+[a-zA-z]*\d*`

Quantifiers:
* *       - 0 or More
* +       - 1 or More
* ?       - 0 or One
* {3}     - Exact Number
* {3,4}   - Range of Numbers (Minimum, Maximum)

With the starter code `^https:`, lets add a way to find the slash marks after https:

we can denote "//" as `/{2}` or `/+`

So far the regex code can either be; `^https:/{2}` or `^https:/+`

Lets expand on our existing code `^https:/{2}`

So far we know that after the double "//", there usually comes a string or group of characters to signifiy the "WWW".

Lets add onto the existing code like so `^https:/{2}(w{3}}`.



We can then extend this to be `^https:/{2}w{3}` or `^https:/+w{3}`

As you can see, there are many ways to denote duplicate sequential characters

UW Bootcamp Tutorial Regex;

`^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

So far we explained `^(https?:\/\/)?`.

In terms of qauntifiers, there is asn instance of an quantifier denoted by `{2,6}`. This quanitfier is a range, which specifically calls to search/find 2 to 6 instances of a certian set. Given the immediate preceding REGEX, we are searching for "clusters"  of `[a-z\.]`, . This cluster is essentially a directory, akin to how you have www.example.com/home or www.example.com/profile/details.


### Grouping Constructs

( )     - Group

Groups is a method to capture multiple characters in a single constraint.
You might be asking, why in the world would I need to group characters when you can type raw text to search for character sequences?

Groups allow you to generalize search terms so you can encompass a greater search pattern.

You're thinking, why in the world would we do this? 

The reason is what happens if there are custom lead urls as well WWW?
You would then need to find a way to search for the custom lead url and www, just like you would need to find a way to account for https vs http and .com vs edu vs gov."

We can convert our starter code into something like this; `^https:/{2}(w{3})`. This might look redundant, but the concept of grouping search parameters together will come into play in regards to the https vs http split.

UW Bootcamp Tutorial:

* `^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

Lets begin by identifying the groups in this REGEX.

* `^(Group1)?(Group2)\.(Group3)(Group4)*\/?$/`

Group 1:
* `(https?:\/\/)` - An expression that conditionally adds "://" if there is an http

Group 2:
* `([\da-z\.-]+)` - Finds one or more instances of a string of characters separated by a period

Groupe 3: 
* `([a-z\.]{2,6})` - Finds 2 or 6 instances of character strings separated by a slash. This would account for the .www, edu, gov etc + directory.

Group 4:

* `([\/\w \.-]*)*` - This would account for trailing ends of the URL



### Bracket Expressions

Bracket expressions are constraints that help define one or more character strings that are adjacent to one another. In fact, they can help define multiple types of character strings at once.

take for instance this sample code;

1y1y1y = [1y1y1y]

[^a-zA-z] this regex code excludes all character strings.


[]      - Matches Characters in brackets
[^ ]    - Matches Characters NOT in brackets

Starter code: `^https:/{2}(w{3})`

So far we have (w{3}) to denote "www". Right now, we can define sequential duplicated character strings. Lets add a "." (period) after to account for the period after the "www". 


Here is when we add a bracket []

* `^https:/{2}(w{3}).[   ]`

Remember, brackets can affect multiple types of characters. Unlike groups, which target a specific set, brackets address the categories.

think of {} as looking for rice, while [] can look for rice or Grains

For example, you can type out ZenithHighlight. Or you can type in [a-zA-z] to seach for all character strings. Lets check out character classes in the next section

UW Bootcamp Tutorial:

* `^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`


Touching off from groups, we can see brackets inside some of our group sections. We find brackets in Group 1, Group 2, and Group 3

Group 2:
* `([\da-z\.-]+)` - Is specifically looking for all lowercase characters a-z followed by a character escape of a period. This is specifcally looking for one or more instances of this value. Examples would be www.zenith or www.hero.

Group 3: 
* `([a-z\.]{2,6})` - Finds 2 or 6 instances of character strings separated by a slash. This would account for the .www, edu, gov etc + directory.

Group 4:

* `([\/\w \.-]*)*` - This is looking for the escaped slash mark, a word character, and a escaped period



### Character Classes

Think of character classes as a category of characters that you are searching for. 
for example, the vast majority of text in this md is of the "text" class. Character classes are found via using brackets [ ] 

Character classes is special notation for matching cateogires of characters.

Examples
* [a-z]  Searches for a single character in  lowercase text.
* [A-Z]  Searches for a single character in all uppercase text
* [0-9]  Searches for numerical characters
* 0[xX][A-Fa-f0-9]+ searches for CSS hexadecimal numbers


A character class is specific.

Our Starter code:  `^https:/{2}(w{3}).[a-zA-z0-9]\w*`

 The above sample code section; `[a-zA-z0-9]` allows us to search for any character, any digit after the "www.". The "\w*" then notates that we are taking a succession, or sequence of characters that fullfil the search criteria of any character/any digit.

 We can also add the `[a-zA-z0-9]` bracket to account for the fact that not all urls have "www" in front of the domain name. `^https:/{2}[a-zA-z0-9]\w*.[a-zA-z0-9]\w*`

 UW Bootcamp Tutorial:

 `^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$`

So far we've talked about

- `^(https?:\/\/)?` - A conditional starter. That searchers for the presence of https;
- `([a-z\.]{2,6})` - That searches for the directory listsings following .com/edu/gov.

UW Bootcamp Tutorial 

`^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

As you can see, we can explicitly find examples of character classes in the UW Regex.
As built on from prior sections, we can clearly see why the regex expression is looking for generalized ranges of characters. By doing so, you can call out www. , domain names, and .edu/gov/etc ending urls.

`([\da-z\.-]+)` will try to find all one or more instances of `[\da-z\.-]`


### The OR Operator

|       - Either Or

If you've been following along the tutorial so far, you'd probably remember that near the beginning of this tutorial we had to address how to find capture both https and http search values.

If we had used the above class contraint [a-zA-Z0-9]\w* to search for https or http, this regex expression would've found any string sequence in front of a non-digit/non-character. Instead, we can narrow down the starting search values by using the "|" expression. 

First, we have to use a group () to associate https and http. Add the | operator in between https and http to get the this following starter code;

Starter code: `^(http|https):/{2}[a-zA-z0-9]/w*.[a-zA-z0-9]\w*`

using this above code now captures the http://www.vertex.gov target expression.

UW Bootcamp Tutorial:

There isnt a or variable listed in the UW Bootcamp Regex

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

From experimenting in VS code so far, you might notice that VS code seems to have a number of these Flags on by default. 

### Character Escapes

Backlashes in the regex expression are "escapes". Character escapes allow searches to account for metacharacters.

The purpose of the `/` is to signify the next character is not an operator. There is a difference in say, a + sign to denote a mathematical equation, and a `+` sign to denote one or more 

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

Lets take the starting code `^(http|https:/{2}[a-zA-z0-9]/w*.[a-zA-z0-9]\w*`

We know now that the "." can be expressed as `.` , `\W`. lets go ahead and add this value to account for the "."

`^(http|https:/{2}(w{3}).[a-zA-z0-9]\w*\W`

Now lets account for the .edu, .gov, .com variations in urls.

We could do a hard code `(edu|gov|com)`, or `[a-zA-z0-9]\w*`

Using the latter would actually hit a dummy code. The former narrows the values of the search to a valid URL.

`^(http|https):/{2}(w{3}).[a-zA-z0-9]\w*\W(edu|gov|com)`

UW Bootcamp Tutorial:

`^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

There are many character escapes in this REGEX.

We can find 2 character escapes in the first group. `^(https?:\/\/)?`. Using character escapes tells to the search function that the slashes are not operators.

The next character escape is in hte second group; `([\da-z\.-]+)`. The character escaped "." denotes the period in between www and the domain name.

There is an explicit hardcoded `\.` that denotes the period after the domain name.

You then have `([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

Like i have explained before, the REGEX is looking for successive strings separated by slash marks. 

## ENDING REGEX.

UW Bootcamp Version: `^(https?:\/\/)?([\da-z\.-]+)\.([a-z\.]{2,6})([\/\w \.-]*)*\/?$/`

- Searches for URLs and expanded directories

Starter Code Version: `^(http|https):/{2}(w{3}|[a-zA-Z0-9]\w*).[a-zA-z0-9]\w*\W(edu|gov|com)$`

- Searchers for URLS wihtout expanded directories.

## Author

Github Profile: JMagic-Learner 

BIO: A aspiring coder currently in the UWBootCamp program. Currently learning MongoDb.

https://github.com/JMagic-Learner
