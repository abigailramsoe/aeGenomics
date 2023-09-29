# Sherlock Holmes exercise 

The sequencing files you will need to map are single-end NGS data two different pathogens.

## The data is located in sherlock_holmes 

NOTE before you start:
You can either copy the files four sequencing files and the reference genome into your own directory or you can specify the full path of the file when you are performing the data analysis.

In this part you are given two compressed FASTQ files in the directory /TEACHING/BIOINF23/day3/

One of these files is ancient, and one is modern. They are both reads from the pathogen <i>Pseudomonas aeruginosa</i> (reference genome fasta: paeruginosa.fasta.gz)

~~~bash
inspector.fastq.gz
barnaby.fastq.gz 
~~~

The goal is to ascertain which of the files (Inspector or Barnaby) belongs to the modern or ancient sample. For this exercise, you can look at the computer exercises from part1 and part2 for inspirations 

## Questions:

1. Perform, using fastp, adapter trimming on the provided sample sequence files. Make sure to discard reads under 30 bp (-l parameter).

2. How many reads contained adapters in both datasets? 

3. What is the mean length of the reads before and after trimming?

4. Perform bwa alignment using aln and samse. For each sample, sort the sam file, save it as a bam, and index it. Remember that the reference is NOT the same as the previous exercise. 
   
6. For each sample, create new bam files with only the aligned reads. What proportion of reads remain?
   
7. What is the mean depth of coverage for each sample? (use samtools depth test.bam|datamash mean 3)

8. Use mapDamage to identify the nucleotide misincorporation and fragmentation patterns for each sample

9. Based on your results obtained from the previous questions, which of the files looks ancient and which one looks modern? Why`





