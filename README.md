<h1 align="center">Code Repository for Sanger 16s Sequence Analysis</h3>

 <p align="center">
    Instructions for Reproducing 16s Analysis for Sanger Sequencing
    <br />
   <br />
   <br />
  </p>
  

## 0. Input Data:

Sanger sequencing raw absrobance data can be found in "./all_plate_ab1".

## 1. Running Phred (CodonCode Aligner):
Obtain CodonCode Aligner (https://www.codoncode.com/aligner/) and basecall ab1 files from ./all_plate_ab1 with phred-phrap using script found at ./CodonCode_Aligner_Scripts/Basecall_Phred.alscpt. All phred basecalled sequences should be in "all_out.fasta" and quality scores should be in "all_out.q".

## 2. Combine Sequences, and quality scores + Transform to Phred64 to Phred33
Follow through notebook phred_fix_fastq.ipynb. Output files from this step will be "All_16s_fix_2.fasta" and "All_16s_fix_2.fastq". 

## 3. Trim Sequences with FastP (Version: 0.22.0)
```sh
fastp -Q -L -5 -3 --cut_front_mean_quality 25 --cut_tail_mean_quality 25 -i All_16s_fix.fastq -o All_16s_fix_trim_25.fastq 
```
Statistics can be viewed from "fastp.html" with data stored in "fastp.json".
## 4. Convert Fastq to Fasta format
```sh
sed -n '1~4s/^@/>/p;2~4p' All_16s_fix_trim_25.fastq > All_16s_fix_trim_25.fasta 
```

## 5. Identify Sequences with RDP Classifier
### Fasta sequences were uploaded to http://rdp.cme.msu.edu/classifier/ with the following settings: <br>
The online RDP Classifier has been depreciated but results can be generated using the below settings.
Classifier: RDP Naive Bayesian rRNA Classifier Version 2.11 <br>
Taxonomical Hierarchy: RDP 16S rRNA training setNo 18 07/2020 <br>
Confidence threshold (for classification to Root ONLY): 80% <br>
### Results can be found in RDP_Output

## 6. (Optional) Get sequence length statistics
Follow through notebook "fasta_length_table_gen.ipynb". Sequence length statistics can be found in "trimmed16s_seq_length.csv".
