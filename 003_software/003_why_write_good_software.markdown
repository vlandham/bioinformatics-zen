Bioinformatics is far from software development or computer science. A bioinformatician's goal is developing novel scientific research or tools. A software developer is judged on on delivering reliable software that people will pay to own. A biologist, whether they use Perl, a pipette or both, is evaluated on their publication record.

In bioinformatics, or any computational science, software development is therefore a lesser priority than generating new data. A [Fisher exact test][fisher] out weighs a [unit test][unit]. A series of Python scripts for interpreting Chip-chip data are necessary tools; what is more important is the publishable end result.

Compare this with commercial software development, for example development of an online line booking system for a hotel. The developer talks to the hotel to understand the job. A good developer maintains regular meetings with the hotel, to update the project based on the customer's requirements. The developer maintains the code using common software development practices: [unit testing][unit], [automated building][build], and [source control][source].

The situation in bioinformatics is different; a hypothesis is made, implemented, and tested. The method of research is a directory full of BLAST results with an Excel spreadsheet, to a full application stack with a database backend, revision control, and unit tests. Even reading the manuscript in detail can leave the implementation in doubt.

Is good software important for bioinformatics? Either end of the above scenario is extreme, and the common approach is a set of microarray files with Perl scripts to parse out the gene rows related the gene ALS2, with a few R scripts to plot the results.

Receiving peers review on a manuscript is like getting feedback on delivering product to a customer. Instead of new features requests, changes are required as new analysis or the addition of a new data set. The same principles that apply for commercial software do apply for scientific software. Version control is a safety net for making changes to existing code. Unit testing ensures fewer bugs. Using make files makes execution of linear tasks easier. A database enables easier manipulation of large complex data.

[fisher]: http://en.wikipedia.org/wiki/Fisher%27s_exact_test
[unit]: http://en.wikipedia.org/wiki/Unit_testing
[build]: http://en.wikipedia.org/wiki/Build_Automation
[source]: http://en.wikipedia.org/wiki/Revision_control
