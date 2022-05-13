# Alignment of two Fragaria iinumae genomes 
### by Anushreeya Gurung and Mamta Kajal

## Background
We are interested in checking how accurate our pseudo chromosome assembly of Fragaria iinumae is and will check how well raw reads of the same species aligns to the reference genome. 
If there is poor alignment, then the pseudo chromosome assembly could be erroneous. 
If the alignment is good but there are still differences, then we can submit changes indicated by the raw reads to the pseudo chromosome assembly to further improve the assembly. 

Thomas Davis and Lise Mahoney et al. 2017 assembled the pesudo chromosome asembly of Fragaria iinumae in 2017 using Illumina Sequencing which is in pseudomolecules form and also Sequenced the raw reads from Fragaria iinumae in 2021 Using Oxford Nanopore which we compared. The raw reads are in its raw form (220 Fatsq.gz files).



## Methods
We accessed the Thomas Davis and Lise Mahoney et al. 2017 pesudochromosomes on a thinkmate in the Davis Lab at UNH whihc is not included in this reposotory for privacy reasons.

The Project pipeline:

![plot](plots/Screenshot_2022-05-11_121105.jpg)
Our pesudochromosome and raw read files were in fastq.gz.
We installed bwa, samtools, and IGV with a conda environment.
We ran our analysis on the thinkmate that stored the data. Below are a few example lines of how to run the tools used in this analysis and the file names should be changed to reflect your own files. The exact commands used in this study are in a shell script file in the directory. 
### BWA
Software for aligning sequence against a genome. There are 3 different algorithms we can specify from that will make the analysis more accurate that is determined from the size of the reads. We used mem algorithm at first because it Align 70bp-1Mbp query sequences and ours are 100-200 bp long. This did not generate viable SAM/BAM files so we were advised to use bwasw as this could be better for long reads. The inputs are reference genome sequence and raw reads in the form of fastq and it outputs a SAM file. A SAM file is a human readable text file that contains the results of the analysis that compared the raw reads to the reference sequence. 
```
BWA can be cloned by: git clone https://github.com/lh3/bwa.git
cd bwa;
make;
./bwa index reference.fastq
./bwa algorithm reference.fastq rawread.fastq > align.sam
```

### SAMtools
A software for parsing and manipulating the alignments in SAM/BAM file. We inputted a SAM file and was returned a BAM file, then the BAM file which is the binary computer legible version of the SAM file was used to generate a stats file. The stats file was then used to generate plots on coverage and depth statistics which will ultimately allow us to figure out if reference pseudochromosome sequence is accurate. 
```
Samtools –b align.sam > align.bam
samtools stats input.bam > input.bam.stats
plot-bamstats -p sample# input.bam.stats
```

### IGV
Integrated Genome Viewer (IGV) is an interactive tool for the visual exploration of genomic data.
```
IGV example command
```

## Results
### 15 concatnated reads
![plot](plots/slide1.jpg)
We first concatated the alignment results of 15 random reeds to make sure the pipeline we provided above would work. 
##### Percentile of mapped sequence ordered by GC content vs mapped depth
Mapped depth increases with percent of mapped sequence and there is a higher medeian depth for higher GC content. This means the sequences that were higher in GC content could be better sequenced. The graph is not showing the lower and upper percentiles for some reason. 
##### Coverage vs Number of mapped bases
This plot is empty even though we expect line of the coverage to peak at 0 and tapper off to 1 to represent that there are less areas with high depth. We are not sure why this plot is not showing depth. 
##### GC Content [%] vs Normalized frequency
The long reads have a model GC content of 38.4% which is less than the 39.70% GC content that the Proceedings of the National Academy of Sciences said in the Frageria iinumae genome. The read could have orignated from a noncoding region explaning why the read was less GC rich than the genome. 
##### Read cycle vs indel count
The numebr of insertions and deletions is high for the first 20000 cycles but then sharply decreases and stays steady. 
##### Read cycle vs Base content [%] 
These reads are high in Thymine(blue) and Adenine(green) as shown by the blue and the faint green line that is underneath the blue. 
##### Indel length vs Indel count [log]
The insertions and deletion patterns of the indels are fairly the same and there are smaller indels than larger. The model of the ins/del ratio is between 50 and 60 which means insertions occur more frequently than deletions in these read samples and the inserted reads are longer than the size of the deleated reads.

### 220 concatnated reads
![plot](plots/slide2.jpg)
![plot](plots/slide3.jpg)
After we were sure the pipleline was not erranous, we proceded the analysis with all the 220 reads we had in our artisinal.
##### Indel length vs Indel count [log]
The insertions and deletion patterns of the indels are fairly the same and there are smaller indels than larger. The model of the ins/del ratio is around 80 which means insertions occur more frequently than deletions and the inserted reads are longer than the size of the deleated reads in these read samples which is consistant with the results of 15 concatnated reads. 
##### Read cycle vs indel counthigh
The numebr of insertions and deletions is high for the first 20000 cycles but then sharply decreases and stays steady which is consistant with the results of 15 concatnated reads. 
##### Read cycle vs Base content [%] 
These reads are high in Thymine(blue) and Adenine(green) as shown by the blue and the faint green line that is underneath the blue which is consistant with the results of 15 concatnated reads. It also makesn sense for the purine and pyrimidine to both be high and they are complements. 
##### Coverage vs Number of mapped bases
This plot is empty even though we expect line of the coverage to peak at 0 and tapper off to 1 to represent that there are less areas with high depth. We are not sure why this plot is not showing depth. 
##### GC Content [%] vs Normalized frequency
The long reads have a model GC content of 38.4% which is less than the 39.70% GC content that the Proceedings of the National Academy of Sciences said in the Frageria iinumae genome. The read could have orignated from a noncoding region explaning why the read was less GC rich than the genome. This is consisitant with the results achieved for the 15 concatnated reads. 

### IGV plot for 5 random reads
Here we wanted to test the pipleline for errors so we randomly selected 15 reads to analyze the depth.
![plot](plots/slide4.jpg)
Here we could see similarities between the reads and could tell that with the addition of more reads that there would be regions of high and low depth.
![plot](plots/slide5.jpg)
This is a concatination of 15 of the reads and already we can tell there is a pattern of regions with higher depth. Sometimes there is a contig in one read where there is a gap in another read. These are split reads and can be used to improve the reference genome assembly. Not pictured here but there are regions of repeated sequences so reads will pile up at the first instance of the repeatable sequence. This means there will be higher depth at a repeatble sequence location at the begining of the refernece genome rather than later instances of the same sequence because alignment is order based. 

## Issues and further work
The analysis resulted in poor coverage in the coverage vs number of mapped bases graph and the reference genome did not appear on the GC contenet [%] vs Normalized frequency graph as expected. Future work includes delving deeper and runing BWA with more reads and figuring out why 2 graphs were abnormal as described. More plots of read data are included in addtional plots folder however they are not noteworthy on their own as we are not sure where they are located in the genome. If we kenw what genee they beloged to, then we could make a more noteworthy analyses













    



## Commands used:
 2060  bwa mem FragariaPseudoChrsV2.fasta FAQ99085_pass_635cf375_2.fastq > sample4.sam
 2061  ls
 2062  less sample4.sam 
 2063  samtools view -S -b sample4.sam > sample4.bam
 2064  ls
 2065  less sample4.bam 
 2066  samtool stats sample4.bam > sample4.bam.stats
 2067  samtools stats sample4.bam > sample4.bam.stats
 2068  less sample4.bam.stats 
 2069  plot-bamstats -p my_output sample4.bam.stats 
 2070  less sample4.bam.stats 
 2071  plot-bamstats -p my_output sample4.bam.stats 
 2072  ls
 2073  samtools index sample4.bam sample4.bam.bai
 2074  samtools index sample4.bam sample4.bai
 2075  ls
 2076  samtools view sample4.bam | head
 2077  samtools sort sample4.bam -o sample4.sorted.bam
 2078  samtools view sample4.sorted.bam | head
 2079  samtools index sample4.sorted.bam
 
 ## Acknowledgements
 #### Dr Jefferey Miller
 #### Dr Tom Davis
 #### Clayton Ludwig (PhD Candidate)
 
 ##### Analysis of the plots made possible by http://avrilomics.blogspot.com/2013/07/ 
 ##### Iinumae data compirison made possible by https://www.pnas.org/doi/10.1073/pnas.2105431118  


 



