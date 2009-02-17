Scripts differentiate computational research from software production. Scripting is writing a file of code with a specific purpose such as running a BLAST search on the *E. coli* genome. A bioinformatician using scripts as the tools for research is comparable to the laboratory biologist using a pipette. In comparison with software development scripting is an auxiliary to make designing software to sell to a customer. The focus is the finished product and scripts can make source code management or unit testing easier to perform than repetitive manual activation. Is there best practice for organising a set of scripts?

TODO: Insert something here about directory organisation

### Managing dependency

Scripts are often required to run in a specific order. One script produces results which is the input to the next script. One script is dependent on the previous script being run first. Greater dependency in software equates to increased complexity and creates more work to maintain a project. If there is a bug in one script, which creates undetected errors in the data, mistakes are propagated through the remaining workflow. In another example, if one script in a workflow is forgotten, and the results of a previous work iteration are still present, then the results of the previous iteration can enter the script workflow.

Removing dependencies between workflow steps is difficult. Build files such as [Rake][rake], [Ant][ant], and [make][make] allow dependencies between scripted steps to be formalised: any required previous steps are run first. This is useful to create maintenance dependencies to delete previous results, [reload new data][biorake], or even repeat the complete analysis from scratch. [Capistrano][cap] is example how build files can be used to automate tasks across multiple computers. All of this can be managed using single command line calls.

### Light and fluffy

Light and simple scripts are easier to maintain. To simplify, a script reads in a set of input data (a protein sequence), analyses the data (formatdb on a protein database followed by BLAST), and then returns to the data (prints the results to the command line). Using this simplification, a script only needs to know what data is coming in, how to analyse the data, and where to return it. Scripts can be made lighter by reducing the contained analytical code. Instead of writing the code to use BLAST, use existing code such as in [BioPerl][Perl]. If the code you need doesn't exist anywhere else, consider writing it as a separate library which can be shared across all your scripts, or with other people.

Keeping light and simple, and formalising dependencies makes scripting easier to manage, maintain, and execute.

[rake]: http://www.example.com
[ant]: http://www.example.com
[make]: http://www.example.com
[biorake]: http://www.example.com
[cap]: http://www.example.com
[Perl]: http://www.example.com 
