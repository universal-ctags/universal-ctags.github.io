---
layout: post
title: optscript
---

For server years, @masatake has made the execution of ctags slower.
He is working hard for implementing yet another feature called
**optscript** that makes the execution much slower.

---

optscript is an implementation of PostScript alike stack-oriented
programming language. optscript has many non-graphical operators
already.

You can use optscript to add extra actions to `--regex-<LANG>=`
option by putting optscript code at the end of the option. You
must wrap the code with `{{` and `}}`.

Let's see an example.

input source code (`input.hello`):
```
def x-y-z:
end
```

optlib file (`example.ctags`):
```
--langdef=hello
--kinddef-hello=d,def,definitions
--map-hello=+.hello

--regex-hello=/^def +([-a-z]+):$/\1/d/{{
	/putlast { 1 index length exch put } def
	/tr {
	    % str [int<from> int<to>] str'
	    ()
	    2 index {
		% str [int<from> int<to>] str' int<chr>
		dup 3 index
		% str [int<from> int<to>] str' int<chr>  int<chr> [int<from> int<to>]
		0 get
		% str [int<from> int<to>] str' int<chr> int<chr> int<from>
		eq {
		    pop
		    % str [int<from> int<to>] str'
		    dup 2 index 1 get putlast
		} {
		    % str [int<from> int<to>] str' int<chr>
		    1 index exch putlast
		} ifelse
	    } forall
	    % str [int<from> int<to>] str'
	    exch pop
	    0 exch putinterval
	} def
	. :name {
	   dup (-_) tr
	   . exch name:
	} if
}}
```

ctags execution:
```
$ u-ctags --options=example.ctags -o - input.hello
x_y_z	input.hello	/^def x-y-z:$/;"	d
```

In the example Universal-ctags with `example.ctags` extracts
`x-y-z` in `input.hello`.

`example.ctags` transforms the tag name `x-y-z` to `x_y_z`.
The code written in optscript does this transformation.

Let's look the code between `--regex-hello=/^def +([-a-z]+):$/\1/d/{{`
and `}}`.

```
	/putlast { 1 index length exch put } def
```

This fragment defines a procedure named `putlast`.
`putlast` put a character at the end of a string buffer.

```
	/tr {
...
	} def
```

This defines a procedure named `tr`.
`tr` replaces translates characters.

```
(a_b_c) (_-) tr
```

In the above example, tr replaces `_` in `a_b_c` with
`-`. As a result, you get `a-b-c`.

Defining `putlast` and `tr` is just for the preparation
for the next step.

```
	. :name {
	   dup (-_) tr
	   . exch name:
	} if
```

`.` represents a tag entry for `x-y-z` extracted by the
regular expression pattern `/^def +([-a-z]+):$/`.  A tag entry
(`tagEntryInfo`) having `x-y-z` as name and `d` as kind is already
built on the memory, but ctags doesn't emit it to a tags file yet because
ctags must execute the action specified with `{{...}}` before
emitting.

`. :name` gets the name of the tag entry as a string and
put it to the operand stack of optscript interpreter.
`(-_) tr` replaces `-` in the string with `_`. `. exch name:` sets
the string to the name field of the tag entry.

After executing the optscript code, ctags emits the tag entry to the tag file.
What you will see is the transformed tag entry:

```
x_y_z	input.hello	/^def x-y-z:$/;"	d
```

I will add more operators to export the ctags internal APIs that could
be used only from parsers (crated parsers) written in C language.
Providing the way to access the scope stack is the primary target. It will
be an important part for Vue parser.
See https://github.com/universal-ctags/ctags/issues/1577 about the background.

Do you want to use optscript in your .ctags?  You can learn the language with
[BlueBook](https://www.amazon.com/PostScript-Language-Tutorial-Cookbook-Systems/dp/0201101793)
till I (@masatake) merge the change for the optscript interpreter to ctags.

[K&R](https://www.amazon.com/Programming-Language-2nd-Brian-Kernighan/dp/0131103628)
is a book for persons who think that studying the stack-oriented
languge is too hard.
