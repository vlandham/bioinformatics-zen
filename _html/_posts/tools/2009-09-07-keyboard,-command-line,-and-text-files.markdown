---
  category: tools
  layout: default
---

Here I'm outlining an approach to bioinformatics focusing on using three tools: the keyboard to enter commands, the wide range of functions available at the command line, and storing data in easily searched and manipulated text files. These combination of these three can make bioinformatics analysis faster than using the mouse with graphical interfaces because typing into the command line is faster than clicking buttons and selecting menus. A series text commands is also easier to reproduce and automate than remembering a sequence of mouse commands.

### The Keyboard

The best reason for using the keyboard is that it's faster to type a command in contrast to doing the same thing with a mouse and GUI. As the keyboard often used to write code the more you can use the keyboard for other actions to change to a different graphical window or to save the current document the less your "working flow" is broken by reaching for the mouse. Finally once you know the keyboard commands to do your analysis learning to touch type will make you work faster.

### The Command Line

The command line gives predictable and reproducible results unlike a graphical interface. A workflow in the command line is easier for someone else to test and execute than a list screenshots of which buttons and menu items to click. 

The Linux command line has a wide variety of tools ranging from interacting with the operating system to manipulating entries in a text file. Many bioinformatics applications can also be run at the command line and the benefit of using the command line version of a tool is that sequences of commands can be chained together to parse the results. The output of one command becomes the input of another command with filters the results. In this way sequences of simple commands can be combined together to perform more complex bioinformatics analysis. As these these tasks do not require a user with a mouse either they can be performed simultaneously across multiple machines or scheduled to be performed overnight.

### Text Files

Binary files in specific formats can usualy only be search and editted by the program that created them. Command line tools can manipulate any plain text file. Therefore common data text formats such as CSV, JSON, YAML, and XML can be searched for specific records. Combining the command line with text files makes it relatively simple to perform simultaneous large searches or manipulations across many text based files.

### Managing spelling in text and images

In this video I'm illustrating how the spelling of the sulfur across text and image files can be maintained consistently using the command line program [sed][sed].

<object width="640" height="385"><param name="movie" value="http://www.youtube.com/v/d0TkCdqekS0&hl=en&fs=1&hd=1"></param><param name="allowFullScreen" value="true"></param><param name="allowscriptaccess" value="always"></param><embed src="http://www.youtube.com/v/d0TkCdqekS0&hl=en&fs=1&hd=1" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="640" height="385"></embed></object>
<p/>

### Manipulating data files on the command line

In this video I'm showing how spread sheet of amino acid atomic content can be converted to [comma separated value (CSV) format][csv]. The amino acid data is then searched using [grep][grep] to find for the entry for cysteine and [AWK][awk] to sort by carbon content. Finally I use [R language][R] to create histograms and x/y plots of the data.

<object width="640" height="385"><param name="movie" value="http://www.youtube.com/v/tUuRBIZVOpY&hl=en&fs=1&hd=1"></param><param name="allowFullScreen" value="true"></param><param name="allowscriptaccess" value="always"></param><embed src="http://www.youtube.com/v/tUuRBIZVOpY&hl=en&fs=1&hd=1" type="application/x-shockwave-flash" allowscriptaccess="always" allowfullscreen="true" width="640" height="385"></embed></object>
<p/>

[sed]: http://en.wikipedia.org/wiki/Sed
[csv]: http://en.wikipedia.org/wiki/Comma-separated_values
[grep]: http://en.wikipedia.org/wiki/Grep
[awk]: http://en.wikipedia.org/wiki/AWK
[R]: http://en.wikipedia.org/wiki/R_%28programming_language%29
