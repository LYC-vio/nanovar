<p align="center">
  <img src="http://benoukraf-lab.com/wp-content/uploads/2019/05/Nanovarlogo.png" width="200" alt="accessibility text" align='left'>
</p>  

<br/><br/>

## NanoVar - Structural variant caller using low-depth long-read sequencing
[![Build Status](https://app.travis-ci.com/cytham/nanovar.svg?branch=master)](https://app.travis-ci.com/github/cytham/nanovar)
[![PyPI pyversions](https://img.shields.io/pypi/pyversions/nanovar)](https://pypi.org/project/nanovar/)
[![PyPI versions](https://img.shields.io/pypi/v/nanovar)](https://pypi.org/project/nanovar/)
[![Conda](https://img.shields.io/conda/v/bioconda/nanovar)](https://anaconda.org/bioconda/nanovar)
[![Github release](https://img.shields.io/github/v/release/cytham/nanovar?include_prereleases)](../../releases)
[![PyPI license](https://img.shields.io/pypi/l/nanovar)](./LICENSE.txt)
  
NanoVar is a genomic structural variant (SV) caller that utilizes low-depth long-read sequencing such as
 Oxford Nanopore Technologies (ONT). It characterizes SVs with high accuracy and speed using only 4x depth
  sequencing for homozygous SVs and 8x depth for heterozygous SVs. NanoVar reduces sequencing cost and computational requirements
   which makes it compatible with large cohort SV-association studies or routine clinical SV investigations.  

### Basic capabilities
* Performs long-read mapping (Minimap2 and HS-BLASTN) and SV discovery in a single rapid pipeline.
* Accurately characterizes SVs using long sequencing reads (High SV recall and precision in simulation datasets, overall F1
 score >0.9)  
* Characterizes six classes of SVs including novel-sequence insertion, deletion, inversion, tandem duplication, sequence
 transposition and translocation.  
* Requires 4x and 8x sequencing depth for detecting homozygous and heterozygous SVs respectively.  
* Rapid computational speed (Takes <3 hours to map and analyze 12 gigabases datasets (4x) using 24 CPU threads)  
* Approximates SV genotype
* Detect large chromosomal copy-number variation using [CytoCAD](https://github.com/cytham/cytocad)
* Identifies full-length LINE and SINE insertions (Marked by "TE=" in the INFO column of VCF file) 

## Getting Started

### Quick run

```
nanovar [Options] -t 24 -f hg38 --cnv hg38 sample.fq/sample.bam ref.fa working_dir 
```

| Parameter | Argument | Comment |
| :--- | :--- | :--- |
| `-t` | num_threads | Indicate number of CPU threads to use |
| `-f` (Optional) | gap_file (Optional) | Choose built-in gap BED file or specify own file to exclude gap regions in the reference genome. Built-in gap files include: hg19, hg38 and mm10|
| `--cnv` | hg38 | Perform large CNV detection using CytoCAD (Only works for hg38 genome)
| - | sample.fq/sample.bam | Input long-read FASTA/FASTQ file or mapped BAM file |
| - | ref.fa | Input reference genome in FASTA format |
| - | working_dir | Specify working directory |

See [wiki](https://github.com/cytham/nanovar/wiki) for entire list of options.

#### Output
| Output file | Comment |
| :--- | :--- |
| ${sample}.nanovar.pass.vcf | Final VCF filtered output file (1-based) |
| ${sample}.nanovar.pass.report.html | HTML report showing run summary and statistics |

For more information, see [wiki](https://github.com/cytham/nanovar/wiki).

### Operating system: 
* Linux (x86_64 architecture, tested in Ubuntu 14.04, 16.04, 18.04)  

### Installation:
There are three ways to install NanoVar:
#### Option 1: Conda (Recommended)
```
# Installing from bioconda automatically installs all dependencies 
conda install -c bioconda nanovar
```
#### Option 2: PyPI (See dependencies below)
```
# Installing from PyPI requires own installation of dependencies, see below
pip install nanovar
```
#### Option 3: GitHub (See dependencies below)
```
# Installing from GitHub requires own installation of dependencies, see below
git clone https://github.com/cytham/nanovar.git 
cd nanovar 
pip install .
```
### Installation of dependencies
* bedtools >=2.26.0
* samtools >=1.3.0
* minimap2 >=2.17
* makeblastdb and windowmasker
* hs-blastn ==0.0.5

Please make sure each executable binary is in PATH.
##### 1. _bedtools_
Please visit [here](https://bedtools.readthedocs.io/en/latest/content/installation.html) for instructions to install.

##### 2. _samtools_
Please visit [here](http://www.htslib.org/download/) for instructions to install.

##### 3. _minimap2_
Please visit [here](https://github.com/lh3/minimap2) for instructions to install.

##### 4. _makeblastdb_ and _windowmasker_
```
# Download NCBI-BLAST v2.3.0+ from NCBI FTP server
wget ftp://ftp.ncbi.nlm.nih.gov/blast/executables/blast+/2.3.0/ncbi-blast-2.3.0+-x64-linux.tar.gz

# Extract tar.gz
tar zxf ncbi-blast-2.3.0+-x64-linux.tar.gz

# Copy makeblastdb and windowmasker binaries to PATH (e.g. ~/bin)
cp ncbi-blast-2.3.0+/bin/makeblastdb ~/bin && cp ncbi-blast-2.3.0+/bin/windowmasker ~/bin
```
##### 5. _hs-blastn_
```
# Download and compile the 0.0.5 version
git clone https://github.com/chenying2016/queries.git
cd queries/hs-blastn-src/v0.0.5
make

# Copy hs-blastn binary to path (e.g. ~/bin)
cp hs-blastn ~/bin
```
If you encounter "isnan" error during compilation, please refer to [this](https://github.com/cytham/nanovar/issues/7#issuecomment-644546378).

## Documentation
See [wiki](https://github.com/cytham/nanovar/wiki) for more information.

## Versioning
See [CHANGELOG](./CHANGELOG.txt)

## Citation
If you use NanoVar, please cite:

Tham, CY., Tirado-Magallanes, R., Goh, Y. et al. NanoVar: accurate characterization of patients’ genomic structural variants using low-depth nanopore sequencing. Genome Biol. 21, 56 (2020). https://doi.org/10.1186/s13059-020-01968-7


## Authors

* **Tham Cheng Yong** - [cytham](https://github.com/cytham)
* **Roberto Tirado Magallanes** - [rtmag](https://github.com/rtmag)
* **Touati Benoukraf** - [benoukraflab](https://github.com/benoukraflab)

## License

This project is licensed under GNU General Public License - see [LICENSE.txt](./LICENSE.txt) for details.

## Simulation datasets and scripts used in the manuscript
SV simulation datasets used in the manuscript can be downloaded [here](https://doi.org/10.5281/zenodo.3569479 ). Scripts used for simulation dataset generation and tool performance comparison are available [here](./scripts).

Although NanoVar is provided with a universal model and threshold score, instructions required for building a custom neural-network model is available [here](https://github.com/cytham/nanovar/wiki/Model-training).

## Limitations
* The inaccurate basecalling of large homopolymer or low complexity DNA regions may result in the false determination of deletion SVs. We advise the use of up-to-date ONT basecallers such as Guppy to minimize this possibility.

* For BND SVs, NanoVar is unable to calculate the actual number of SV-opposing reads (normal reads) at the novel adjacency as
 there are two breakends from distant locations. It is not clear whether the novel adjacency is derived from both or either
  breakends, in cases of balanced and unbalanced variants, and therefore it is not possible to know which breakend location(s) to
   consider for counting normal reads. Currently, NanoVar approximates the normal read count by the minimum count from either 
   breakend location. Although this helps in capturing unbalanced BNDs, it might lead to some false positives.
