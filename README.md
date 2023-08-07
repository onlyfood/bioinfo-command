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

12. Find out how many genes are in common to all three conditions:
    ```
    comm -1 -2 conditionA conditionB > conditionAB
    cut -f1 apple.conditionC | sort -u > conditionC
    comm -1 -2 conditionAB conditionC | wc -l
    ```

**Note**: Make sure to replace `conditionA`, `conditionB`, and `conditionC` with the appropriate filenames representing the gene lists for each condition.

Remember that these commands assume the data is properly formatted and placed in the specified files (`apple.genome` and `apple.genes`). If you encounter any issues, check the file paths and contents to ensure accurate results. Happy analyzing!
