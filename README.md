<h1 align="center">Code Repository for Sanger 16s Sequence Analysis</h3>

 <p align="center">
    Instructions for Reproducing 16s Analysis for Sanger Sequencing
    <br />
   <br />
   <br />
  </p>
  

## 0. Input Data:

Sanger sequencing raw absrobance data can be found in "./all_plat_abs".

## 1. Running Phred:
```sh
export PHRED_PARAMETER_FILE=/opt/src/phred/phredpar.dat
phred -id abs/ -sa All_16s.fasta -qa All_16s.qual
```

## 2. Combine Sequences, and quality scores + Transform to Phred64 to Phred33
Follow through notebook phred_fix_fastq.ipynb

## 3. Trim Sequences with FastP (Version: 0.22.0)
```sh
fastp -Q -L -5 -3 --cut_front_mean_quality 25 --cut_tail_mean_quality 25 -i All_16s_fix.fastq -o All_16s_fix_trim_25.fastq 
```

## 4. Convert Fastq to Fasta format
```sh
sed -n '1~4s/^@/>/p;2~4p' All_16s_fix_trim_25.fastq > All_16s_fix_trim_25.fasta 
```

## 5. Identify Sequences with RDP Classifier
### Fasta sequences were uploaded to http://rdp.cme.msu.edu/classifier/ with the following settings: <br>
Classifier: RDP Naive Bayesian rRNA Classifier Version 2.11 <br>
Taxonomical Hierarchy: RDP 16S rRNA training setNo 18 07/2020 <br>
Confidence threshold (for classification to Root ONLY): 80% <br>

