# bioinfo-command
This is a repository to show case bioinfo projects that utilize command line tools. Most if not all command lines are ran under UNIX environment.
### basic unix code

### Getting Started
1. Navigate to the `project1` folder by using the command `cd project1`.

### Extract Files
2. Extract the compressed data file by running the following commands:
   ```
   gunzip gencommand_proj1_data.tar.gz
   tar -xf gencommand_proj1_data.tar
   ```

### Gene and Transcript Variant Analysis
3. Find out how many chromosomes are there in the genome by using the command:
   ```
   grep -c ">" apple.genome
   ```

4. Determine the number of genes and transcript variants by executing the following commands:
   ```
   cut -f1 apple.genes | uniq | wc -l
   cut -f2 apple.genes | uniq | wc -l
   ```

5. Identify how many genes have a single splice variant:
   ```
   cut -f1 apple.genes | uniq -c | grep " 1 " | wc -l
   ```

6. Calculate the number of genes that have 2 or more splice variants:
   ```
   cut -f1 apple.genes | uniq -c | grep -v " 1 " | wc -l
   ```

7. Determine how many genes are there on the '+' and '-' strands:
   ```
   cut -f1,4 apple.genes | grep "+" | uniq | wc -l
   cut -f1,4 apple.genes | grep "-" | uniq | wc -l
   ```

8. Find out how many genes are there on each chromosome and list them in the order they appear in the genome:
   ```
   cut -f1,3 apple.genes | grep "chr1" | uniq | wc -l
   cut -f1,3 apple.genes | grep "chr2" | uniq | wc -l
   cut -f1,3 apple.genes | grep "chr3" | uniq | wc -l
   ```

9. Determine how many transcripts are there on each chromosome:
   ```
   cut -f2,3 apple.genes | grep "chr1" | uniq | wc -l
   cut -f2,3 apple.genes | grep "chr2" | uniq | wc -l
   cut -f2,3 apple.genes | grep "chr3" | uniq | wc -l
   ```

### Comparison Between Conditions
10. Find out how many genes are in common between condition A and condition B:
    ```
    cut -f1 apple.conditionA | sort -u > conditionA
    cut -f1 apple.conditionB | sort -u > conditionB
    comm -1 -2 conditionA conditionB | wc -l
    ```

11. Determine how many genes are specific to condition A and how many are specific to condition B:
    ```
    comm -2 -3 conditionA conditionB | wc -l
    comm -1 -3 conditionA conditionB | wc -l
    ```

## sequence features

This README file provides instructions and commands for analyzing alignment data in the `project2` folder. The data contains alignment data for an organism's genome and transcript annotations. Follow the steps below to perform the analysis:

### Unzip Files
1. Extract the compressed data file by running the following commands:
   ```
   gunzip gencommand_proj2_data.tar.gz
   tar xvc gencommand_proj2_data.tar
   ```

### Alignment Analysis
2. Determine how many alignments the set contains:
   ```
   samtools view athal_wu_0_A.bam | head
   samtools view athal_wu_0_A.bam | wc -l
   ```

3. Find the number of alignments that show the read's mate unmapped:
   ```
   samtools view athal_wu_0_A.bam | cut -f7 | grep "*" | wc -l
   ```

4. Calculate how many alignments contain a deletion (D):
   ```
   samtools view athal_wu_0_A.bam | cut -f6 | grep "D" | wc -l
   ```

5. Determine how many alignments show the read's mate mapped to the same chromosome:
   ```
   samtools view athal_wu_0_A.bam | cut -f7 | grep "=" | wc -l
   ```

6. Find out how many alignments are spliced:
   ```
   samtools view athal_wu_0_A.bam | cut -f6 | grep "N" | wc -l
   ```

7. Sort and index the BAM file:
   ```
   nohup samtools sort athal_wu_0_A.bam athal_wu_0_A.sorted &
   samtools index athal_wu_0_A.sorted.bam
   ```

### Genome and Annotation Analysis
8. Find the number of sequences in the genome file and the length of the first sequence:
   ```
   samtools view -H athal_wu_0_A.bam
   ```

9. Determine the read identifier (name) for the first alignment and the start position of its mate on the genome:
   ```
   samtools view athal_wu_0_A.bam | head
   ```

10. Count the number of overlaps reported between the annotations and the alignments:
    ```
    bedtools bamtobed -i athal_wu_0_A.bam > data.bed
    bedtools intersect -wo -a athal_wu_0_A_annot.gtf -b data.bed | wc -l
    ```

11. Count the number of overlaps that are 10 bases or longer:
    ```
    bedtools intersect -wo -a athal_wu_0_A_annot.gtf -b data.bed | cut -f16 > count_base.txt
    ```

12. Count how many alignments overlap the annotations and how many exons have reads mapped to them:
    ```
    bedtools intersect -wo -a athal_wu_0_A_annot.gtf -b data.bed | wc -l
    bedtools intersect -wo -a athal_wu_0_A_annot.gtf -b data.bed | cut -f4 | sort -u | wc -l
    bedtools intersect -wo -a athal_wu_0_A_annot.gtf -b data.bed | cut -f5 | sort -u | wc -l
    ```

13. If the transcript annotations in the file `athal_wu_0_A_annot.gtf` were converted into BED format, find the number of generated BED records:
    ```
    bedtools intersect -wo -a athal_wu_0_A_annot.gtf -b data.bed | cut -f9 | cut -d " " -f4 | sort -u | wc -l
    ```

