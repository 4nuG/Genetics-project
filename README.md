![Static Badge](https://img.shields.io/badge/2022-orange)
![Static Badge](https://img.shields.io/badge/Finished%20-green)
![PyPI - Python Version](https://img.shields.io/pypi/pyversions/bwa)


![Thumbnail](https://raw.githubusercontent.com/4nuG/Genetics-project/main/gene_banner.png)

--------
This README is a detailed analysis of aligning two Fragaria iinumae genomes, combining Illumina and Nanopore sequencing data. 
The files in the GitHub are more of a memonto for us if we continue this project in the future. 

Note: As these genomes are not published, we have not uploaded the files here.

##### Table of Contents  
- [Introduction](#introduction)  
- [Project](#project)
- [Material](#material)
- [Method](#method)
- [Results](#results)
  - [15 Concatenated Reads](#15-concatenated-reads)
  - [220 Concatenated Reads](#220-concatenated-reads)
  - [IGV Analysis](#igv-analysis)
- [Conclusions and Future Work](#conclusions-and-future-work)
- [Commands Used](#commands-used)
- [Acknowledgements](#acknowledgements)
- [References](#references)

# Aligning Two Fragaria iinumae Genomes 
### by Anushreeya Gurung and Mamta Kajal

<a name="introduction"></a>
## Introduction
Fragaria iinumae (2n) is one of the diploid progenitors of cultivated strawberry, Fragaria ×ananassa (8x) (Rousseau-Gueutin et al. 2009; Dimeglio et al., 2014). The Davis Lab at UNH sequenced the genome of F. iinumae in 2017 using the Illumina Sequencing platform. This assembly is in its pseudochromosomes form. However, there are still some gaps or missing reads in the assembly. Recently, the genome from the same plant was sequenced using Oxford Nanopore Sequencing Technology in the Davis Lab. This data is in its raw form (220 Fastq files). The purpose of this assembly is to fill in the gaps between the earlier genome and check its accuracy.

<a name="project"></a>
## Project
For Gen 711/811, we are doing a part of this project, i.e., aligning the raw reads of Nanopore genome data to pseudochromosomes of the Illumina Sequenced Genome. We are interested in checking how accurate the pseudochromosome assembly is and how well the reads from Nanopore data align to it.

<a name="material"></a>
## Material
- Reference Genome: Pseudochromosomes assembly of F. iinumae (~198 Mb)
- Query Genome: Raw reads generated from Oxford Nanopore Sequencing (220 Fastq files)
Both genomes are sequenced from one plant of F. iinumae. As these genomes are not published, we have not uploaded the files here.

<a name="method"></a>
## Method

The Project Workflow:

![image](https://user-images.githubusercontent.com/103778906/168407761-e1b720a8-0af2-40d8-b77b-563b355d809d.png)

The major software used were BWA, SAMtools, and IGV. The exact commands used in this study are in a shell script file in the directory. Following are brief details about these tools:

### BWA
Software for aligning sequence against a genome. There are 3 different algorithms we can specify that will make the analysis more accurate, determined from the size of the reads. We used the mem algorithm at first because it aligns 70bp-1Mbp query sequences, and ours are 100-200 bp long. This did not generate correct SAM/BAM files, so we were advised to use bwasw as it is better for long reads. The inputs are reference genome sequence and raw reads in the form of fastq, and it outputs a SAM file. A SAM file is a human-readable text file that contains the results of the analysis that compared the raw reads to the reference sequence.

```bash
BWA can be cloned by: git clone https://github.com/lh3/bwa.git
cd bwa;
make;
./bwa index reference.fastq
./bwa algorithm reference.fastq rawread.fastq > align.sam
```

### SAMtools
A software for parsing and manipulating the alignments in SAM/BAM file. The input was a SAM file, and a BAM file was generated. Then the BAM file, which is the binary computer-readable version of the SAM file, was used to generate a stats file. The stats file was then used to generate plots on coverage and depth statistics, which are helpful in knowing about the similarities in both genomes.

```bash
Samtools –b align.sam > align.bam
samtools stats input.bam > input.bam.stats
plot-bamstats -p sample# input.bam.stats
```

### IGV
Integrated Genome Viewer (IGV) is an interactive tool designed for the visual exploration of genomic data. We used IGV to align both genome sequences and visually inspect the alignment. Keep in mind that index files for both genomes are required for proper viewing.

<a name="results"></a>
## Results
We initiated the analysis by concatenating 15 fastq files (out of 220 total files) into one file and aligning this combined file to the reference genome.

### 15 Concatenated Reads
![plot](plots/slide1.jpg)

We first concatenated the alignment results of 15 random reads to ensure the functionality of the provided pipeline.

##### Percentile of Mapped Sequence Ordered by GC Content vs Mapped Depth
Mapped depth increases with the percent of mapped sequence, and there is a higher median depth for higher GC content. This suggests that sequences with higher GC content could be better sequenced.

##### Coverage vs Number of Mapped Bases
This plot is empty even though we expect the coverage line to peak at 0 and taper off to 1, representing fewer areas with high depth. We are not sure why this plot is not showing depth, but when this file was aligned with the reference genome in IGV, much read depth was seen.

##### GC Content [%] vs Normalized Frequency
The long reads have a model GC content of 38.4%, approximately the same as mentioned by Qiao et al. (2021), i.e., 39.70% GC content.

##### Read Cycle vs Indel Count
The number of insertions and deletions is high for the first 20,000 cycles but then sharply decreases and stays steady.

##### Read Cycle vs Base Content [%]
These reads are high in Thymine (blue) and Adenine (green) as shown by the blue and faint green lines that are underneath the blue.

##### Indel Length vs Indel Count [log]
The insertion and deletion patterns of the indels are fairly the same, with smaller indels more frequent than larger ones. The model of the ins/del ratio, ranging between 50 and 60, indicates that insertions occur more frequently than deletions in these read samples, and the inserted reads are longer than the size of the deleted reads.

### 220 Concatenated Reads
![plot](plots/slide2.jpg)
![plot](plots/slide3.jpg)

After confirming the pipeline's accuracy with 15 concatenated reads, we proceeded with the analysis by concatenating all 220 fastq files into one file and aligning it with the reference genome.

##### Indel Length vs Indel Count [log]
The insertion and deletion patterns of the indels are similar to the results from 15 concatenated reads, with smaller indels more frequent than larger ones. The model of the ins/del ratio is around 80 which means insertions occur more frequently than deletions and the inserted reads are longer than the size of the deleated reads in these read samples which is consistant with the results of 15 concatnated reads. 

##### Read Cycle vs Indel Count
The number of insertions and deletions is high for the first 20,000 cycles but then sharply decreases and stays steady, which is consistant with the results of the 15 concatnated reads. 

##### Read Cycle vs Base Content [%]
These reads are high in Thymine (blue) and Adenine (green), consistent with the results of 15 concatenated reads. It also makesn sense for the purine and pyrimidine to both be high as they are complements.

##### Coverage vs Number of Mapped Bases
This plot is empty even though we expect the coverage line to peak at 0 and taper off to 1, indicating fewer areas with high depth. We are not sure why this plot is not showing depth.

##### GC Content [%] vs Normalized Frequency
The long reads have a model GC content of 38.4%, consistent with the results achieved for the 15 concatenated reads.

### IGV Analysis
In this section, our aim was to test the pipeline for errors. To achieve this, we randomly selected 15 reads for depth analysis. As we examined these reads, we observed similarities and anticipated that with the inclusion of more reads, there would be identifiable regions of both high and low depth.

![plot](plots/slide5.jpg)
This concatenation of 15 reads already shows a pattern of regions with higher depth. Split reads and regions of repeated sequences can be observed, suggesting potential improvements to the reference genome assembly. Not pictured here, but there are regions of repeated sequences, so reads will pile up at the first instance of the repeated sequence. This means there will be higher depth at a repeated sequence location at the beginning of the reference genome rather than later instances of the same sequence because alignment is order-based.

![image](https://user-images.githubusercontent.com/103778906/168408944-4b301373-73a1-4bee-89ca-96a3b986c4b7.png)

This image was generated by aligning all 220 concatenated files with the reference genome, indicating more depth compared to the 15 concatenated files.

<a name="conclusions-and-future-work"></a>
## Conclusions and Future Work
Initially, we observed that the Nanopore sequencing generated 220 fastq files, likely due to multiple runs on the same sample over around 3 days. The analysis revealed similarities in read length between concatenated files (15 fastq files and 220 fastq files) viewed in IGV. We concluded that eliminating errors in the Nanopore genome assembly and aligning it to the reference genome can fill in the gaps of the reference genome.

<a name="commands-used"></a>
## Commands Used
These are the general commands we used for the analysis:

Reference genome : FragariaPseudoChrsV2.fasta
Query genome files: FAQ99085_pass_635cf375_2.fastq

```bash
 bwa index reference.fastq
 bwa bwasw FragariaPseudoChrsV2.fasta FAQ99085_pass_635cf375_2.fastq > sample.sam
 samtools view -S -b sample.sam > sample.bam
 samtools stats sample4.bam > sample4.bam.stats
 samtools view sample4.bam | head
 samtools sort sample4.bam -o sample4.sorted.bam
 samtools view sample4.sorted.bam | head
 samtools index sample4.sorted.bam
 plot-bamstats -p my_output sample4.bam.stats 
```

 <a name="acknowledgements"></a>
 ## Acknowledgements
- Dr Jefferey Miller
- Dr Tom Davis
- Clayton Ludwig (PhD Candidate)

<a name="references"></a>
## References
#### DiMeglio, L. M., Staudt, G., Yu, H., & Davis, T. M. (2014). A Phylogenetic Analysis of the Genus Fragaria (Strawberry) Using Intron-Containing Sequence from the ADH-1 Gene. PLoS ONE, 9(7), e102237. https://doi.org/10.1371/journal.pone.0102237
#### Rousseau-Gueutin, M., A. Gaston, A. Aïnouche, M.L. Aïnouche, K. Olbricht, G. Staudt, L. Richard, and B. Denoyes-Rothan. 2009. Tracking the evolutionary history of polyploidy in Fragaria L. (strawberry): New insights from phylogenetic analyses of low-copy nuclear genes. Mol. Phylogenet. Evol. 51:515–530. doi:10.1016/j.ympev.2008.12.024
#### Qiao Qin, Edger Patrick P., Xue Li, Qiong La, Lu Jie, Zhang Yichen, Cao Qiang, Yocca Alan E., Platts Adrian E., Knapp Steven J., Van Montagu Marc, Van de Peer Yves, Lei Jiajun, & Zhang Ticao. (2021). Evolutionary history and pan-genome dynamics of strawberry (Fragaria spp.). Proceedings of the National Academy of Sciences, 118(45), e2105431118. https://doi.org/10.1073/pnas.2105431118
