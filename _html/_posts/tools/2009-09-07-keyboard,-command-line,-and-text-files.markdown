---
  category: tools
  layout: default
---

Here I'm outlining a bioinformatics workflow focusing on using three tools: the keyboard to enter commands, the variety of command line programs, and storing data in easily editable text files. These combination of the tools make bioinformatics analysis faster because typing into the command line is faster than clicking buttons and selecting menus in a graphical interface. Furthermore a series text commands is also easier to reproduce and automate than remembering which order to push buttons and select menus with the mouse.

### The Keyboard

The best reason for using the keyboard is that it's faster than the mouse. As a general rule I think that typing commands is faster that doing the same thing with a mouse. As the keyboard is used to write and text the more you can use the keyboard for other actions the less your "working flow" is broken by reaching for the mouse to change to a different graphical window or to save the current document. Finally taking some time to learn to touch type with make you faster at typing but don't anyone wants to get faster at using the mouse.

### The Command Line

The command gives predictable and reproducible results unlike a graphical interface. A workflow in the command line is easier for someone else to test and execute than a list screenshots of which buttons and menu items to click. 

The Linux operating system has wide variety of command line tools ranging from maintaining a webserver to manipulating entries in text file. Many bioinformatics applications can also be run at the command line, and the benefit of using the command line is that sequences of commands can be easily chained together. The text output of one command becomes the input of another command and sequences of simple commands can be combined together to perform more complex bioinformatics analysis. As these these tasks do not require a user with a mouse they can be performed simultaneously across multiple machines or scheduled to be performed overnight.

### Text Files

Binary files in specific formats can only be read by the program that created them. Command line tools instead focus on manipulating the same type of file: plain text. Simple text formats such as CSV, JSON, and YAML easy to search for specific words. Combining the command line with text files makes it relatively simple to perform simultaneous large searches or manipulations across many files.
