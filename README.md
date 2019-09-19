
# Flow control and Starting Docassemble

## For Python try the ascii.py program

# Introduction to YAML

YAML was originally an acronym for 'Yet Another Markup Language', but is now more commonly referred to as 'YAML Ain't Markup Language'

The hardest part about learning **docassemble** is not writing
***Python*** code, since sophisticated interviews can be built using
nothing more complicated than a few [if/else statements].  The more
difficult aspect may be learning **YAML**.  While the **YAML** format
looks simple, it can be frustrating.

To understand **YAML**, you first need to understand the difference
between a "list" and a "dictionary."

A "list" is an ordered collection of things.  If my to-do list for a
Saturday afternoon was first to take out the garbage, and then to
sweep the porch, this could be represented in **YAML** as:

```
- Sweep the porch
- Take out the garbage
```

***(We will cover "dictionaries" and "lists" in more detail shortly - consider this next session  a preview)***

A "dictionary," by contrast, associates things with other things.  For
example, if I have some legal terms that I want to associate with an
explanation, I could put this in a **YAML** dictionary:

```yaml
lawyer: A person who represents you.
judge: A person who decides who wins or loses a court case.
```

While a list has an order to it (e.g., I need to first sweep the porch and
then take out the garbage), the dictionary is just a jumble of words
and definitions.  More generally, it associates "keys" with "values."

**YAML** interprets lines of text and figures out whether you are
talking about a list or a dictionary depending on what punctuation you
use.  If it sees a hyphen, it thinks you are talking about a list.  If
it sees a colon, it thinks you are talking about a dictionary.

Lists and dictionaries can be combined.  You can have a dictionary of
lists and a list of dictionaries.  If I wanted to express the to-do
lists of multiple people, I could write:

```yaml
Frank:
  - Sweep the porch
  - Take out the garbage
  - Clean the toilets
Sally:
  - Rake the leaves
  - Mow the lawn
```

Here, you have a dictionary with two keys: "Frank" and "Sally."  The
value of the "Frank" key is a list with three items, and the value of
the "Sally" key is a list with two items.

If you are familiar with **Python**'s data notation, this translates
into:

```python
{"Frank": ["Sweep the porch", "Take out the garbage", "Clean the toilets"],
"Sally": ["Rake the leaves", "Mow the lawn"]}
```

The **JSON** representation is the same.

You can also have a list of dictionaries:

```yaml
- title: Tale of Two Cities
  author: Charles Dickens
- title: Moby Dick
  author: Herman Melville
- title: Green Eggs and Ham
  author: Dr. Seuss
```

In **Python**'s data notation, this translates into:

```python
[{'title': 'Tale of Two Cities', 'author': 'Charles Dickens'}, {'title': 'Moby Dick', 'author': 'Herman Melville'}, {'title': 'Green Eggs and Ham', 'author': 'Dr. Seuss'}]
```

**YAML** also allows you to divide up data into separate "documents"
using the `---` separator.  Here is an example of using three
documents to describe three different books:

```yaml
title: Tale of Two Cities
author: Charles Dickens
---
title: Moby Dick
author: Herman Melville
---
title: Green Eggs and Ham
author: Dr. Seuss
```

**YAML**'s simplicity results from its use of simple punctuation marks.
However, be careful about data that might confuse the computer.  For
example, how should the computer read this shopping list?

```yaml
- apples
- bread
- olive oil, the good stuff
- shortening: for cookies
- flour
```

In **Python**, this will be interpreted as:

```python
['apples', 'bread', 'olive oil, the good stuff', {'shortening': 'for cookies'}, 'flour']
```

This is a list of apples, bread, olive oil, a dictionary, and flour.
That's not what you wanted!

You wanted `shortening: for cookies` to be a piece of text.  But the
computer assumed you wanted to indicate a dictionary.  **YAML**'s clean
appearance makes it readable, but this kind of problem is the downside
to **YAML**.

You can get around this problem by putting quote marks around text:

```yaml
- apples
- bread
- olive oil
- "shortening: for cookies"
- flour
```

This will result in all of the list elements being interpreted as
plain text.  In **Python**:

```python
['apples', 'bread', 'olive oil', 'shortening: for cookies', 'flour']
```

**YAML** also allows text to be block quoted:

```yaml
title: |
  Raspberry Jam: a "Fancy" Way to Eat Fruit
author: |
  Jeanne Trevaskis
```

The pipe character `|` followed by a line break indicates the start of
the quote.  The indentation is important because it indicates where
the block quote ends.  As long as you are indenting each line of text,
you can write anything you want in the text (e.g., colons, quotation
marks) without worrying that the computer will misinterpret what you
are writing.

The following values in **YAML** are special:

* `null`, `Null`, `NULL` -- these become `None` in **Python**
* `true`, `True`, `TRUE` -- these become `True` in **Python**
* `false`, `False`, `FALSE` -- these become `False` in **Python**
* numbers such as `54`, `3.14` -- these become numbers in **Python**

These values will not be interpreted as literal pieces of text, but as
values with special meaning in **Python**.  This can cause confusion in
your interviews, so if you ever use "True" and "False" as a label or
value, make sure to enclose it in quotation marks.

This **YAML** text:

```yaml
loopy: 'TRUE'
smart: false
pretty: TRUE
energetic: "false"
```

becomes the following in **Python**:

```python
{'loopy': 'TRUE', 'smart': False, 'pretty': True, 'energetic': 'false'}
```

One feature of **YAML** that is rarely used, but that you may see, is
the use of "explicit mapping."  Instead of writing:

```yaml
apple: red
orange: orange
banana: yellow
```

You can write:

```yaml
? apple
: red
? orange
: orange
? banana
: yellow
```

Both mean the same thing.  You might want to use this technique if
your labels in a ***`fields`*** specifier are long.  For example, instead
of writing:

```yaml
question: |
  Please answer these questions.
fields:
  "Where were you born?": place_of_birth
  "What were the last words of the first President to fly in a Zeppelin?": words
```

you could write:

```yaml
question: |
  Please answer these questions.
fields:
  ? Where were you born?
  : place_of_birth
  ? |
    What were the last words of the 
    first President to fly in a Zeppelin?
  : words
```

Note that many punctuation marks, including `"`, `'`, `%`, `?`, `~`, `|`, `#`, `>`, `:`, `!`, `:`, `{`, `}`,
`[`, and `]`, have special meaning in **YAML**, so if you use them in
your text, make sure to use quotation marks or block quotes.

For more information about **YAML**, see the following resources:

* [YAML - Completely different](http://jessenoller.com/blog/2009/04/13/yaml-aint-markup-language-completely-different)

* [Wikipedia](https://en.wikipedia.org/wiki/YAML)

* [YAML - Cheatsheet](https://kapeli.com/cheat_sheets/YAML.docset/Contents/Resources/Documents/index)

* [VS-Code YAML Extension](https://marketplace.visualstudio.com/items?itemName=redhat.vscode-yaml)
