Writing good code makes life easier. If there's a common theme in bioinformatics, it is this: you will write a script, move on to something else, then return to the script in a few months or years time and try to remember how it works. The clearer the code is written, the better to understand how it works. Writing code is personal, and discussing what makes good code is controversial. Here I'm illustrating what I think are some basic principles that apply to most languages.

### Be verbose

I think code should err on the side of being to verbose, rather than being to concise. I mean that code should be loud and descriptive about what its purpose is. An example is variable names.

    # Concise
    seq = File.read('gene.fasta')
    
    # Verbose
    fasta_gene_sequence = File.read('gene.fasta')

The second example is longer, but leaves no doubt as to what the variable contains. The same can be applied to method names.

    # Concise
    def get_seq(file)
      # ...
    end

    # Verbose
    def read_fasta_from(file)
      # ...
    end

Next are magic numbers, numbers that appear in code, but have no explanation to their meaning.

    # Magic number here
    dna_sequence.scan(3)

    # Verbose
    nucleotides_per_codon = 3
    dna_sequence.scan(nucleotides_per_codon)

Comments never hurt either, as long as they are correct. Comments are more useful when the meaning of the code is not obvious.

    # Why the chop?
    protein = dna_sequence.translate.chop

    # Remove the stop codon after translating
    protein = dna_sequence.translate.chop

Try to follow the indentation guidelines for the language you're writing in. Indentation makes code easier to read for you and anyone you share the code with.

  # Unindented
  def translate_dna(dna_string)
  
  end

  # Easier to read perhaps...

### DRY

DRY means don't repeat yourself. Code for a single function should exist in a single place. When code needs fixing or maintaining, it only needs to changed once in the one place that it resides. In the short term its tempting to to copy and paste code to save time, but this will be time consuming in the long term when debugging is required.

A common function, such as system specific BLAST setting, used across a variety of scripts can be kept in a single file. The setting can be called by any script when required. By moving all the common code to a single file, if the BLAST settings need to be updated, this is done in one place.

### Books and frameworks

When I used Java, Joshua Bloch's  Effective Java book helped me learn a great deal about how to programme using Java. When learning Ruby I found the Ruby Way book had many useful examples of how to write in Ruby. I might guess for any popular programming language there is a very respected book that illustrates the best practices in the language. The type of book I'm thinking of is not necessarily useful for beginners, but more for people who are confident writing in the language, and want to learn well. I think a book like this is a good investment for writing better, more maintainable code.

In addition to good books, examples of the best practices in a language can be found in popular open source libraries. Rails is a ruby framework for creating dynamic website. Knowing Rails will come in handy if I ever need to create an interactive website, but practising with Rails also gives an opinionated view of the best way to organise a ruby project, from people who are experienced in creating them.
