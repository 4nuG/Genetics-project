# Genetics-project
Project pipeline
- have Fragaria iinumae genome assembly in contig stage and will be validating
- started looking for potential softwares, a few of those are: 
    - flye software 
    - BiscoT software by Bionano genomics

update
4/14/2022
We acquired draft sequence of f. illumne using illumina

we can:
    -validate curent contigs
    -form our own contigs
    
    we should probabaly select the task we could most comfortable finish in a week 
    

update
4/2/2022 
we will validate the draft sequence by using BWA which will create a bam file. From the bam files we can create graphs using tools like IGV.

Burrows-Wheeler Aligner: http://bio-bwa.sourceforge.net/
there are 3 algorithms. We will use BWA-MEM becuse our reads are 100-250 bp and this algo can accomidate that. It is aslo one of the the most acurate.
We need to figure out what the two reference genome formats are. If they are compatable or not or how to do so. we will access this sunday.
