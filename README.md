# **Metagenomics Analysis Pipeline**  

This repository contains a Bash script (`script1.sh`) for processing metagenomic paired-end sequencing data. The pipeline performs quality control, host decontamination, sequence assembly, gene prediction, and abundance quantification, generating organized results for each sample.  

## Features

- **Quality Control**: Uses `fastp` to trim and filter low-quality reads.
- **Host Decontamination**: Removes host sequences using `bowtie2` with a mouse genome index.
- **Sequence Assembly**: Assembles contigs using `megahit`.
- **Sequence Statistics and Filtering**: Generates contig statistics and filters contigs (>500 bp) with `seqkit`.
- **Gene Prediction**: Predicts genes on filtered contigs using `prodigal`.
- **Abundance Quantification**: Quantifies contig abundance using `salmon`.

## Prerequisites
Ensure the following tools are installed and accessible in your `PATH`:

- fastp (>= 0.20.0)
- bowtie2 (>= 2.3.5)
- megahit (>= 1.2.9)
- seqkit (>= 0.12.0)
- prodigal (>= 2.6.3)
- salmon (>= 1.4.0)

Additionally, you need:

A Bowtie2 index for the host genome (e.g., mouse genome, specified in the script as `/mnt/d/Datas/Metagenomics/Base/bowtie2/mouse`).
Input data in paired-end FASTQ format (gzip-compressed).

## Input Data Structure
The script expects input data organized in a directory (`total_folder`) with subdirectories for each sample:
```
total_folder/
├── sample1/
│   ├── sample1_1.fastq.gz
│   ├── sample1_2.fastq.gz
├── sample2/
│   ├── sample2_1.fastq.gz
│   ├── sample2_2.fastq.gz
...
```



Each sample directory (`sampleX`) contains paired-end FASTQ files named` sampleX_1.fastq.gz` and `sampleX_2.fastq.gz`.
The script automatically detects sample directories matching the pattern `sample*`.

## Output Structure
For each sample, the script creates a result directory (`sampleX_results`) with the following structure:
```
sampleX_results/
├── sampleX_clean_1.fastq.gz        # Quality-controlled FASTQ (R1)
├── sampleX_clean_2.fastq.gz        # Quality-controlled FASTQ (R2)
├── sampleX_fastp.html              # Fastp quality report (HTML)
├── sampleX_fastp.json              # Fastp quality report (JSON)
├── sampleX_host.sam                # Bowtie2 alignment (SAM)
├── sampleX_host_removed_1.fastq    # Host-decontaminated FASTQ (R1)
├── sampleX_host_removed_2.fastq    # Host-decontaminated FASTQ (R2)
├── sampleX_bowtie2.log             # Bowtie2 log
├── sampleX_megahit/                # Megahit assembly results
│   ├── sampleX.contigs.fa          # Assembled contigs
│   ├── contigs_stats.txt           # Seqkit contig statistics
│   ├── contigs_500.fa              # Filtered contigs (>500 bp)
│   ├── sampleX.prodigal            # Prodigal gene predictions (GFF)
│   ├── contigs_500.fna             # Prodigal nucleotide sequences
│   ├── salmon_index/               # Salmon index
│   ├── salmon/                     # Salmon quantification results
│   │   ├── quant.sf                # Abundance quantification
│   │   ├── logs/                   # Salmon logs
```
## Installation

1. Clone the repository:git clone https://github.com/your-username/your-repo-name.git
cd your-repo-name


2. Ensure all required tools are installed (see Prerequisites).  

Verify the host Bowtie2 index path in the script (HOST_INDEX variable). Update it to your local path if necessary:HOST_INDEX="/path/to/your/bowtie2/mouse"  


Update the input directory path in the script (INPUT_DIR variable) to your data directory:INPUT_DIR="/path/to/total_folder"



## Usage

Make the script executable:chmod +x script1.sh


Run the script:./script1.sh

The script will process each sample in `INPUT_DIR`, creating result directories (`sampleX_results`) in the current working directory.

## Example
Assume your data is in /data/metagenomics/total_folder with two samples:
```
/data/metagenomics/total_folder/
├── sample1/
│   ├── sample1_1.fastq.gz
│   ├── sample1_2.fastq.gz
├── sample2/
│   ├── sample2_1.fastq.gz
│   ├── sample2_2.fastq.gz
```


Update script1.sh:INPUT_DIR="/data/metagenomics/total_folder"
HOST_INDEX="/path/to/bowtie2/mouse"


Run:./script1.sh


Results will be generated in the current directory:./sample1_results/
./sample2_results/



## Notes

**Error Handling**: The script checks for input file existence and tool execution status, skipping failed samples with error messages.
**Parallelization**: The script uses multiple threads (4 for fastp/bowtie2, 8 for megahit/salmon). Adjust thread counts (-t, -p) based on your system's resources.
**Large Datasets**: For large datasets, consider running the script on a high-performance computing cluster with a job scheduler (e.g., SLURM).
**Customization**: Modify tool parameters (e.g., salmon --validateMappings, seqkit -m 500) in the script to suit your analysis needs.

## Contributing
Contributions are welcome! Please open an issue or submit a pull request with improvements or bug fixes.


## License
This project is licensed under the MIT License. See the LICENSE file for details.


## Contact
For questions or support, please open an issue or contact 1636770513@qq.com.
