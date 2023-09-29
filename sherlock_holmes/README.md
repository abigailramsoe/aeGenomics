# Sherlock Holmes exercise 

## If you are using the course virtual webserver
```
conda activate day1
```


The sequencing files you will need to map are single-end NGS data two different pathogens.

In this part you are given two compressed FASTQ files in the directory course/data/day1

One of these files is ancient, and one is modern. They are both reads from the pathogen <i>Pseudomonas aeruginosa</i> (reference genome fasta: paeruginosa.fasta.gz)

~~~bash
inspector.fastq.gz
barnaby.fastq.gz 
~~~

The goal is to ascertain which of the files (Inspector or Barnaby) belongs to the modern or ancient sample. For this exercise, you can look at the computer exercises from part1 and part2 for inspirations 

## Exercise 3:

1. Find the data folder and copy it to your directory
 ```cp -r course/data/day1/* .```

2. What is in the folder?
```ls course/data/day1/* .```

3. What are the sizes of the files in the folder?
```du -sk course/data/day1/*```


## Exercise 4:

1. Figure out how many lines and how many reads the FASTQ files contain
```zcat file.fastq.gz|wc -l```
```zcat file.fastq.gz|awk '{s++}END{print s/4}'```
   
2. Perform, using fastp, adapter trimming on the provided sample sequence files. Make sure to discard reads under 30 bp (-l parameter).
```fastp -i file.fastq.gz -o file.trimmed.fastq -l 30```

3. What are the output files?
``` ls . ``` 

4. How many reads do the trimmed files contain?
```cat file.trimmed.fastq|awk '{s++}END{print s/4}'```

5. What adapter sequences were detected? (look at the fastp output)

6. How many reads did not pass the filter, and why? (fastp output)

7. What is the average read length of the trimmed files?
```cat file.trimmed.fastq | awk 'NR%4==2{sum+=length($0)}END{print sum/(NR/4)}```   

a. as loop

```
for i in *.gz
do
echo ${i}
zcat ${i}|awk '{s++}END{print s/4}'
done
```

## Exercise 5 

1. Start to map the data
``` bwa aln paeruginosa.fasta.gz file.trimmed.fastq > file.trimmed.sai ``` 
``` bwa samse paeruginosa.fasta.gz file.trimmed.sai file.trimmed.fastq > file.trimmed.sam ``` 
   
2. Convert the sam files to bam
``` samtools view file.trimmed.sam -b > file.trimmed.bam ```

3. Which files are bigger, the sam files or the bam files?
``` ls -l . ``` 

4. Visually inspect the first 10 reads of a bam file
``` samtools view file.trimmed.bam|head ```

5. Sort the bam files
   ``` samtools sort file.trimmed.bam -o file.trimmed.sorted.bam ```

6. How many reads do the bam files contain?
   ``` samtools view -c file.trimmed.sorted.bam ```
   
   
## Exercise 6 

1. Remove unmapped reads and reads with mapping quality less than 30
   ``` samtools view -b -F4 -q 30 file.trimmed.sorted.bam > file.filtered.bam ```

2. How many reads were removed during filtering? (Compare output of exercise 5.6 and below command)
   ``` samtools view -c file.filtered.bam ```

3. Remove duplicate reads
   ``` samtools view -b -F1024 file.filtered.bam > file.filtered.rmdup.bam ```

4. How many reads were duplicates?
   ``` samtools flagstat file.filtered.bam ```

5. Get the average length of the remaining reads
 ```  samtools view file.filtered.rmdup.bam | awk '{print length($10)}' | datamash mean 1 ```

## Exercise 7

1. Index the bam files
   ``` samtools index file.filtered.rmdup.bam ```

2. Run mapDamage
   ``` mapDamage -i file.filtered.rmdup.bam -r  paeruginosa.fasta.gz  --merge-libraries --no-stats```

3. Look at the plots (use evince and then the path to the plots) 





