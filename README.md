# The Silver Searcher #

My attempt to make something better than ack, which itself is better than grep. Right now it's nowhere close.

## Why use ag? ##

* The binary name 33% shorter than ack!
* It searches through code about 3x-5x faster.
* It's written in C instead of perl.
* It ignores files matched by patterns in your .gitignore and .hgignore.
* If there are files in your source repo that you don't want to search, just add their patterns to a .agignore file. \*cough\* extern \*cough\*


## Current development status ##
1. Make it compile.
2. Make it run.
3. Make it behave correctly.
4. Make it fast.

It's somewhere between 2 and 3 right now. Although it's much faster than ack in my benchmarks.

    ack -i SOLR ~/cloudkick/reach  2.89s user 0.77s system 97% cpu 3.765 total

    ag -i SOLR ~/cloudkick/reach  0.37s user 0.15s system 88% cpu 0.590 total

You can use this with [my fork](https://github.com/ggreer/AckMate) of the popular ackmate plugin, which lets you use both ack and ag for searching in textmate. You can also just move or delete `"~/Library/Application Support/TextMate/PlugIns/AckMate.tmplugin/Contents/Resources/ackmate_ack"` and run `ln -s /usr/local/bin/ag "~/Library/Application Support/TextMate/PlugIns/AckMate.tmplugin/Contents/Resources/ackmate_ack"`


## TODO ##
* print out matches in ackmate-compatible format (this is still buggy)
* parse more ack options correctly (context and --ackmate aren't obeyed right now)
* colorize output. paths should be green, lines white except for matches which are black with yellow background


initial brainstorming notes for myself:

* ack annoyances:
  * only uses one core
  * can't prioritize certain dirs/files
  * can't say "just ignore stuff that's in .gitignore/.svnignore/.hgignore/whatever"
  * can't easily introspect what files are being ignored/searched
  * poor/no support for searching/ignoring files with no dots in their name or dots at the beginning or multiple dots, etc
  * ack is a giant hack. it's full of crazy stuff that perl programmers do. tons of special-cases and other dumb stuff
  * ack hates anything that isn't utf-8
  * by default, it hardly searches anything. you have to edit your ~/.ackrc to get useful behavior
        * grep is on the opposite side of the spectrum
  * no easy way to show skipped files in a directory hierarchy
  * if there are no matches, ack doesn't automatically try some less accurate searches
        * concrete example: if no matches, enable ignore case and redo the search
  * basic options:
        * follow/don't follow symlinks
        * recurse dirs

* ack awesomeness (aka stuff I want to copy):
  * can easily skip files (unlike grep)
  * faster than grep
  * integrates with textmate (ackmate)

* code search: common actions
  * find vars or functions with same/similar names in the project

* pie-in-the-sky stuff
  * integrate with filetype-specific libraries (libxml2 etc) for extra functionality
    * concrete example: searching some xml files
      * instead of printing the context lines (prev/next 5 lines), print everything in the local dom scope
    * track code dependencies (this is almost certainly a different product/program)
  * switch to RE2 http://code.google.com/p/re2/ http://john.freml.in/re2-benchmark
