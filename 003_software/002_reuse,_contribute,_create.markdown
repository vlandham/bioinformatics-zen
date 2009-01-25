### Use existing code

A quick way to get something done is using code that someone else has already written. Bioinformatics benefits from existing biology orientated libraries. These libraries are have functions for common bioinformatics tasks, such as reading Fasta files, or parsing BLAST results.

One reason to use an existing library over writing your own code is that it will save time. Second these libraries are mature and tested, which means chance of a bug is much less.  If you're having problems and can't find in the documentation, asking a question on the mailing list will usually result in a suggestion where to look.

### Contribute to existing code

The more specific the requirement the less likely there is an already existing solution. This then requires creating a new solution yourself. After coding something up, a generous thing to do would be to contribute the code to an existing library. This means a little more work, but the warm fuzzy feeling of contributing is worth it.

Contributing code will first require getting the existing code base using whatever version control system the code is managed with. This can be difficult if you're never used a VCS before, but is a good change to learn. Once you've got the library you'll need to add your own code, as well as some documentation, and usually a few tests.  
After this you'll need to send your update back via the VCS or submit a patch to the mailing list.

### Create new code

Creating a new library should be a last resort, but sometimes the function you want doesn't fit with any existing libraries. Why is creating a new library a last resort? Because is takes more work than adding to an existing library, and you don't want to make much effort to share your code. Having said that, creating a new library does have benefits, packaged cod is easier to maintain and use across projects. Taking the time to share your code with other people also makes you a good person.

The smaller and simpler the better when creating a new library. The simpler a library the easier it is to use, and for you to maintain. Use a version control system to keep track of changes to the code, git is a good choice. Document the code, and create some web pages highlighting how the library can used. Develop unit tests so that you can test functionality remains the same whenever changes are made. Make the code open source so that anyone else can contribute. Finally host the library somewhere so that people can get access.

This sounds like a lot of work, which is why the simpler and lighter the library the easier it is to maintain. The process of creating a new library can be rewarding itself, but also has benefits. Other people may like your library and decide to contribute or fix any bugs. Therefore if you use the library regularly yourself, the investment in creating the library returns a divided.
