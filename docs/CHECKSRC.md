# checksrc

This is the tool we use within the curl project to scan C source code and
check that it adheres to our [Source Code Style guide](CODE_STYLE.md).

## Usage

    checksrc.pl [file1] [file2] ...

## What does checksrc warn for?

checksrc does check and verify the entire style guide, but the script is an
effort to detect the most common mistakes and syntax mistakes that
contributers make before they get accustomed to our code style. Heck, many of
us regulars do the mistakes too and this script helps us keep the code in
shape.

    checksrc.pl -h

Lists how to use the script and it lists all existing warnings it has and
problems it detects. At the time of this writing, the existing checksrc
warnings are:

 o `BADCOMMAND`: There's a bad !checksrc! instruction in the code. See the
   **Ignore certain warnings** section below for details.

 o `BANNEDFUNC`: A banned function was used. The funtions sprintf, vsprintf,
   strcat, strncat, gets are **never** allowed in curl source code.

 o `BRACEELSE`: '} else' on the same line. The else is supposed to be on the
  following line.

 o `BRACEPOS`: wrong position for an open brace (`{`).

 o `COMMANOSPACE`: a comma without following space

 o `COPYRIGHT`: the file iis missing a copyright statement!

 o `CPPCOMMENTS`: `//` comment detected, that's not C89 compliant

 o `FOPENMODE`: `fopen()` needs a macro for the mode string, use it

 o `INDENTATION`: detected a wrong start column for code. Note that this warning
   only checks some specific places and will certainly miss many bad
   indentations.

 o `LONGLINE`: A line is longer than 79 columns.

 o `PARENBRACE`: `){` was used without sufficient space in between.

 o `RETURNNOSPACE`: `return` was used without space between the keyword and the
   following value.

 o `SPACEAFTERPAREN`: there was a space after open parenthesis, `( text`.

 o `SPACEBEFORECLOSE`: there was a space before a close parenthesis, `text )`.

 o `SPACEBEFORECOMMA`: there was a space before a comma, `one , two`.

 o `SPACEBEFOREPAREN`: there was a space before an open parenthesis, `if (`,
   where one was not expected

 o `SPACESEMILCOLON`: there was a space before semicolon, ` ;`.

 o `TABS`: TAB characters are not allowed!

 o `TRAILINGSPACE`: Trailing white space on the line

 o `UNUSEDIGNORE`: a checksrc inlined warning ignore was asked for but not used,
   that's an ignore that should be removed or changed to get used.

## Ignore certain warnings

Due to the nature of source code and the flaws of the checksrc tool, there are
sometimes a need to ignore specific warnings. checksrc allows a few different
ways to do this.

### Inline ignore

You can control what to ignore within a specific source file by providing
instructions to checksrc in the source code itself. You need a magic marker
that is `!checksrc!` followed by the instruction. The instruction can ask to
ignore a specific warning N number of times or you ignore all of them until
you mark the end of the ignored section.

Inline ignores are only done for that single specific source code file.

Example

    /* !checksrc! disable LONGLINE all */

This will ignore the warning for overly long lines until later in the code it
is enabled again:

    /* !checksrc! enable LONGLINE */

If the enabling isn't done before the end of the file, it will be enabled
automatically for the next file anyway.

You can also opt to ignore just N violations so that if you have a single long
line you just can't shorten and is agreed to be fine anyway:

    /* !checksrc! disable LONGLINE 1 */

... and the warning for long lines will be enabled again automatically after
it has ignored that single warning. The number `1` can of course be changed to
any other integer number. It can be used to make sure only the exact intended
instances are ignored and nothing extra.

### Directory wide ignore patterns

`checksrc.whitelist` describe!

