Scripts differentiate computational research from software production. A script is a file of code with a specific purpose such as running a BLAST search on the *E. coli* genome. A bioinformatician uses scripts as the tools for research is comparable to a laboratory biologist using a pipette. Contrasted with software development, scripting supplements designing software to sell to a customer. The focus is the finished product and scripts help are there to make source code management or unit testing easier. Is there best practice for organising a set of scripts?

### Managing dependency

Scripts are often required to run in a specific order. One script produces a result which is the input to the next script. This means the second script is dependent on the first script being run before. Greater dependency in software equates to increased complexity and requires more work to maintain a project. If there is a bug in one script, which creates undetected errors in the data, mistakes are propagated through the remaining workflow. If one script in a workflow is missed, and the output files of a iteration remained, then datasets between iterations are mixed between workflow repetitions.

Removing the dependencies between workflow steps is difficult. Build files such as [Rake][rake], [Ant][ant], and [make][make] allow dependencies between scripted steps to be formalised: any required previous steps are automatically run first. This is useful to make requirements that all previous results be deleted first, [any new data is reloaded][biorake], or even that the complete analysis from is repeated from scratch. [Capistrano][cap] is a twist where build files can be used to automate repetitive tasks across multiple computers. All of this can be managed using single command line calls.

### Light and fluffy

Light and simple scripts are easier to maintain. To simplify, a script reads in a set of input data (a protein sequence), analyses the data (formatdb on a protein database followed by BLAST), and then returns to the data (prints the results to the command line). Using this simplification, a script only needs to know what data is coming in, how to analyse the data, and where to return it. Scripts can be made lighter by reducing the contained analytical code. Instead of writing the code to use BLAST, use existing code such as in [BioPerl][Perl]. If the code you need doesn't exist anywhere else, consider writing it as a separate library which can be shared across all your scripts, or with other people. A script that reads in a the data, calls a bio-library, then prints the results in less than ten lines of code is simple to understand. Contrast this with a script that reads in data, formats the data, has several lines of a bespoke function, and then writes output. The simple script is easier to understand and return to.

Keeping light and simple, and formalising dependencies makes script-based projects easier to manage, maintain, and repeat.

[make]: http://www.gnu.org/software/make/
[ant]: http://ant.apache.org/
[rake]: http://rake.rubyforge.org/
[biorake]: http://github.com/jandot/biorake/tree/master
[cap]: http://www.capify.org/
[Perl]: http://www.bioperl.org/wiki/Main_Page
