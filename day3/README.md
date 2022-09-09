# Computer exercises day 3
In this exercise you will get acquainted with the "bwa" mapper and the "samtools" program to interpret sam files, as well as misalignments

The sequencing files you will need to map are single-end NGS data of a pathogen.

## The data are located on ricco.popgen.dk at /TEACHING/BIOINF22/adv_binf_2022_week1/day3

The reference genome, Mycobacterium_leprae.fa.gz, contains a consensus representation of the target organism.

The two sequencing read files consist of two files with a fixed sequence length of 10 bp and 30 bp.

~~~bash
L10.fq.gz
L30.fq.gz
~~~

~~~bash
Hercule.fq.gz
Poirot.fq.gz
~~~

We recommend that you finish/look at the exercises from day1 and day2 for inspiration for how to solve the following questions.

NOTE before you start:
You can either copy the four sequencing files and the reference genome into your own directory or you can specify the full path of the file when you are performing the data analysis.

Before you try with the different commands, you should try to run them with no arguments - this will show you the help page giving you an explanation of the different parameters and examples on how to run the command. 

## Part 1 - Exact alignment (Rabin-Karp) (Pen and paper):

Consider the text=”ACGTAACGTAACGA” and pattern=”TAACG”.

1. What is the length of the strings and what is the size of the alphabet
2. How many shifts and how many comparisons do we need to do in order to perform an exhaustive naive search
3. Consider the Rabin-Karp algorithm and define a rolling hash function
4. Argue that your choice of rolling hash function is indeed a rolling hash function
5. Calculate the hash of the pattern and the hash of each shift
6. How many operations did the exhaustive search with the Rabin-Karp algorithm require?

## Part 2 - Data structures (Pen and paper):
The goal with part of the assignment is to get familiar with the string operations needed to construct the data structures which several modern day alignment algorithms utilize.

Consider the sequence T=”DINGELINGDING”
And the query sequence S=”ING”

## QUESTIONS
1. Draw the suffix Tree
2. Using tree terminology describe how you can identify the number of times the query sequence S occurs in sequence T.
3. From sequence T, generate a table consisting of two columns, with the first containing the suffix array and the second the corresponding suffix (Hint: see slide 22 from the lecture SuffixArraysBWT_2022.pdf ).
4. Identify the position for which the query sequence S occurs within the sequence T. 
5. Generate the Burrows-Wheeler-Transform of sequence T.

## PART 3 - Mapping Statistics

The first two sequencing files L10 and L30 are created with a simulation software, as such the read ID contains the information of the positions for which the sequence originates. Once aligned to the provided reference genome (Mycobacterium Leprae), there can potentially be differences between the origin of the chromosomal position and the aligned position due to misalignments.

e.g.
@T0_RID0_S0_NZ_CP029543.1:2084294-2084323_length:30_R1
Read originating from chromosome 
NZ_CP029543.1
from position
2084294-2084323

We can use this fact to investigate whether the reads generated are mapping to the correct position. 

## QUESTION

1. How large is the bacterial reference genome?

Use bwa aln (and samse) to align the two fastq files to the bacterial reference (Hint: look at the exercises from https://github.com/ANGSD/adv_binf_2022_week1/tree/main/day2).

Sort and index the resulting files using samtools, and make sure they are saved in bam format. 
Filter out the unaligned reads and create new bam files. Identify which flag to filter out on (https://broadinstitute.github.io/picard/explain-flags.html)

By looking at the reads aligning and the chromosomal positions, we can calculate how many reads are mapped correctly with a 0 nucleotide difference between the origin in the read ID and the mapping coordinate. The script /TEACHING/BIOINF22/adv_binf_2022_week1/day3/get_stats.sh takes a bam file and outputs two numbers - firstly the number of reads that map correctly (i.e. those where the start position in the bam file matches the read ID), and then the number that do not map correctly.

Use this script to find out how many reads map correctly and incorrectly in the two bam files 
~~~bash
/TEACHING/BIOINF22/adv_binf_2022_week1/day3/get_stats.sh L30.bam
~~~

Now create two new bam files where you filter the reads so we only retain reads with a mapping quality of greater than or equal to 1. (Hint: look at the exercises from https://github.com/ANGSD/adv_binf_2022_week1/tree/main/day2).

Repeat the above steps exercise with the two new filtered bam files. How do the numbers differ?

2. Is the proportion of incorrectly mapped reads greater or smaller in the original bams or the quality filtered bams? 
3. Why do you think that is?
4. Given the results here, do you think it is relevant to include mapping filters in downstream analysis?

## PART 4 - Data analysis:
In this part you are given two .fq.gz files in the directory /TEACHING/BIOINF22/assignment1_mapping

~~~bash
Hercule.fq.gz
Poirot.fq.gz 
~~~

One of these files is ancient, and one is modern. The goal is to ascertain which of the files (Hercule or Poirot) belongs to the modern or ancient sample. For this exercise, you can look at the computer exercises from day1 and day2 for inspirations (https://github.com/ANGSD/adv_binf_2022_week1/tree/main/day1 and https://github.com/ANGSD/adv_binf_2022_week1/tree/main/day2) 

## Questions:
1. Briefly describe what characterizes ancient DNA from modern DNA

Perform, using fastp, adapter trimming on the provided sample sequence files.

2. How many reads contained adapters in both datasets? 

3. What is the mean length of the reads before and after trimming?

4. Perform bwa alignment using the “ALN” mode
5. Using the sam flags, extract all of the aligned reads, what is the proportion of aligned reads in each of the bam files?
6. What is the mean depth of coverage and the breadth of coverage?

7. Use mapDamage to identify the nucleotide misincorporation and fragmentation patterns, and describe the output plots generated by MapDamage found in the files Fragmisincorporation_plot.pdf and Length_plot.pdf

8. Based on your results obtained from the previous questions, which of the original fastq files looks ancient and which one looks modern?





