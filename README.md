## Frameshift Promoting Motif Identification In High-Throughput Selection Libraries ##

A computational workflow for nominating -1 PRF motifs from in vitro selection libraries, developed by Andrew V. Anzalone and Sakellarios Zairis.

### Requirements: ###
---

- UNIX like operating system
- ghostscript with X11 support
- python 2.7.x
    - numpy
    - pandas
    - matplotlib
    - seaborn
    - biopython
    - scipy
    - Distances
- an internet connection

### Setup: ###
---

Clone this repository and export the environment variable FRAMESHIFT_DIR=/path/to/this/repository (add to .bashrc).
The frameshift executable contains the entire analysis pipeline, and relies on the directory structure of this repository (do not alter).
A sample configuration file is provided in this repository (config_sample.json).
The configuration file defines certain global parameters of the pipeline, whose values will vary with the user data sets.
The following fields are required in the configuration:

1. the pseudoknot scaffold, with fixed nucleotides denoted by {"A", "C", "G", "T"} and variable sites denoted by "."
2. the fraction of the total library reads desired within the top X most abundant unique sequences (to be plotted). running step 1 with different values of "top_fraction" will not necessitate recalculating the abundances.txt from the raw data, so all but the first run will be fast.
3. the allowed lengths to search over for the 7 components of an H-type pseudoknot (upstream, stem1, loop1, stem2, loop2, loop3, downstream).
4. the minimum number of supporting sequences needed to nominate a motif in the final report.

### Usage: ###
---

step 1. input data must be in fastq.gz format; sequences will be counted according to the scaffold defined in the config file.

step 2. specify the top "n" unique sequences by abundance to use in constructing a feature space.

step 3. specify the top "n" unique sequeunces by abundance to scan for framshift motifs; "n" cannot exceed the value used in step 2.

step 4. provide a motif sequence "m" to be used for nucleotide variant analysis.

```bash
$ export FRAMESHIFT_DIR=/path/to/repo
$ frameshift -c config.json -o output_directory -s 1 -i ngs_data.fastq.gz
$ frameshift -c config.json -o output_directory -s 2 -n 10000
$ frameshift -c config.json -o output_directory -s 3 -n 5000
$ frameshift -c config.json -o output_directory -s 4 -m CGCGGTTCTATCTAGTTACGCGTTAAACCAACTAGAAGGCGGTT
```

### Output: ###
---

```bash
$ tree output_directory
    step1/
        abundances.txt
        library_mass_by_rank.pdf
    step2/
        PK_compatibilities.tsv
    step3/
        logo_1.fasta
        logo_2.fasta
        ...
        motif_1.pdf
        motif_2.pdf
        ...
        table_motifs.tsv
    step4/
        single_variants.pdf
        single_variants.txt 
        pairwise_variants.pdf
        pairwise_variants.txt
    tmp/
        ...
```