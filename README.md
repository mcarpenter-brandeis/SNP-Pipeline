# Sample-Pipeline

#Author: Meredith Carpenter

#Goal: Generate a pipeline to analyze fungal species sequences and identify SNP mutations that result in the different color molds
#Inputs: dgorgon_reference.fa (WT reference sequence), harrington_clinicaldata.txt (clinical sample data), hawkins_pooled_sequences.fastq (pooled fastq data)
#Outputs: fastqs (with {sample name}_trimmed.fastq files), bams (with {name}.sorted.bam files), report.txt
#Additional deliverable: pipeline.py (python code) and readme.txt

#Step 1: Read the pooled fastq and output 50 new fastq files that contain only sequences belonging to that sample
#Method: Using the barcode from the clinical data (harrington_clinicaldata.txt), find hte match (at the start of each read), and write the read to its respective file
#Step 1A: Trim the beginning of the read (remove the barcode and associated quality scores)
#Step 1B: Remove the ends of the reads with degraded quality (IFF end has at least 2 consecuitve D or F, then remove rest of read)
#Output from Step 1: 50 fastq files in a directory fastqs, each in the format of {sample name}_trimmed.fastq (ie tim_trimmed.fastq)

#Step 2: Perform alignment on each FASTQ to the reference sequence (dgorgon_reference.fa)
#Method: Call the bwa command to transform the FASTQs to samfiles
#Output: {name}.sam

#Step 3: Convert the sam files to bam files with the samtools view, sort and index the bam files
#Output: {name}.sorted.bam files in directory bams

#Step 4: Use the python pysam pileup function to discover variants in each sorted bam file
#Specifically looking to find what position along the reference has a SNP mutation
#If a position contains nucleotide other than WT (from reference) it is considered a SNP and reported

#Step 5: Creates a report (report.txt) that outputs what nucleotide poisiton and mutation is responsible for each color of the mold.
#Also prints the number of sequences that were used for each sample.

####EXTRA STEP: TRANSLATING DNA AND DETERMINING MUTATION TYPE#####
##GOAL: Determine the protein sequence (AA) of the wildtype dgorgon and use the SNPs for each mold (generated in the previous steps) to determine what amino acid mutations result.
##Results reported: original amino acid at SNP site, mutated amino acid at SNP site, and type of mutation (silent, missense, nonsense)

###To use the script:
#python pipeline.py --fastq /home/rbif/week6/hawkins_pooled_sequences.fastq
