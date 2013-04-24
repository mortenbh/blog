# How to make for-loops in Bash and Z shell

Last modified: 24-04-2013 11:57:37

For-loops come in handy at the command-line quite often, especially if you
want to apply the same command to a bunch of similar named files. The syntax
is quite similar between Bash and Z shell, although I can quite remember the
one for Bash. I put both here for completeness.

The most common task I encounter is that I want to apply the same command to a
sequence of files with numbered, zero-padded filenames. Using Bash, this looks
like:

	$ for i in {0000..123}; do mycommand image_$i.png; done

The syntax for Z shell is slightly easier to remember:

	$ for i in {0000..123}; mycommand image_$i.png
