# metaKrak
This repository helps you master metagenomics with Kraken2.


## official training site [link](https://ugene.net/metagenomic-classification-with-kraken/)
## Official Training Site
- [Trimmomatic Usage](http://www.usadellab.org/cms/?page=trimmomatic)
- [Kraken2 Database](https://benlangmead.github.io/aws-indexes/k2)

## Official Training Site
- [Trimmomatic Usage](http://www.usadellab.org/cms/?page=trimmomatic)
- [Kraken2 Database](https://benlangmead.github.io/aws-indexes/k2)

# Metagenomic with Kraken
## III. install your working environment
![tree metakrak](https://github.com/user-attachments/assets/2d70eba7-f09a-4972-95eb-75b3be24698b)
- Start with conda [conda installation](https://github.com/Bboy010/eDNA-training-24/tree/main)
1. download the working directory
```bash
wget https://github.com/Bboy010/metaKrak/archive/refs/heads/main.zip
```
2. unzip the folder
```bash
sudo apt install unzip
```
```bash
unzip main.zip
rm –r  main.zip
```
3. create Conda environment
```bash
cd metaKrak
```
```bash
cd config-file/
```
```bash
conda env create -f metakrak.yml
```
```bash
conda activate metakrak
conda init
```
---
4. Download Kraken2
*Download directly and store in the metaKrak folder: metagenomic*
Note
: The Kraken database zip is about 5 gb and the unzip 8 gb

``` bash
wget https://genome-idx.s3.amazonaws.com/kraken/minikraken2_v2_8GB_201904.tgz
```
``` bash
tar xfvz minikraken2_v2_8GB_201904.tgz
```
5. delet the zipped file

```bash
rm minikraken2_v2_8GB_201904.tgz
```
6. Check the reposotory and create the folder *data*

```bash
ls
mkdir data
ls data
```
--- 
7. Download Trimmomatic Data (Adapters)
```bash
wget https://raw.githubusercontent.com/usadellab/Trimmomatic/refs/heads/main/adapters/TruSeq3-PE.fa
```
8. Download fastq files *(lib3-1 and 2)* directly from [Google Drive](https://drive.google.com/drive/folders/1c2xbJzB3GmmAEax6u-bMwd4eVAWqCrVJ?usp=drive_link)

- file-1 
> wget https://drive.google.com/file/d/1QHi6kVP2uJSUcPflr_zVa0u0lce5xJ5D/view?usp=drive_link
- file-2
> wget  https://drive.google.com/file/d/1n-_eNympSv9PIIxqNVcP1tkndDcRjUyX/view?usp=drive_link

9. Store downloaded files in Data Folder
```bash
mv *.fastq.gz data/
ls/data

ls # TruSeq3-PE.fa  data  minikraken2_v2_8GB_201904_UPDATE
```
10. Create Shortcuts

``` bash
read1=data/lib3.R1_001.fastq.gz
read2=data/lib3.R2_001.fastq.g
````

11. Run Trimmomatic
``` bash
trimmomatic PE $read1 $read2 lib3.trimmed.paired.R1.fastq.gz lib3.trimmed.unpaired.R1.fastq.gz lib3.trimmed.paired.R2.fastq.gz lib3.trimmed.unpaired.R2.fastq.gz ILLUMINACLIP:TruSeq3-PE.fa:2:30:10 LEADING:3 TRAILING:3 MINLEN:36
```

- Check the output
```bash
ls
mkdir trimmed
mv *.gz trimmed/
ls trimmed/
```
12. Create Shortcuts for Trimmed Data
``` bash
read1=trimmed/lib3.trimmed.paired.R1.fastq.gz
read2=trimmed/lib3.trimmed.paired.R2.fastq.gz
krakendb=minikraken2_v2_8GB_201904_UPDATE
```
13. Run Kraken2
``` bash
mkdir kraken_report
kraken2 --use-names \
--db $krakendb \
--threads 2 \
--report kraken_report/lib3.report \
--paired --gzip-compressed $read1 $read2 > kraken_report/lib3.kraken
```
- checked output classified /unclassified
``` bash
ls
head kraken_report/lib3.report
head kraken_report/lib3.kraken
```
14. Visualization with Krona
- installation de krona
``` bash
cd ~
git clone https://github.com/marbl/Krona.git
cd Krona/KronaTools
sudo ./install.pl
```
- Add Krona to PATH
``` bash
export PATH=$PATH:~/Krona/KronaTools/bin
source ~/.bashrc
```
- Activate the environment again
``` bash
conda activate metakrak
```
- Navigate to the report folder and extract necessary columns
- Extract data

``` bash
tail -n +2 lib3.report | cut -f2,3,6 > lib3_krona_input.txt
```
15. Generate the HTML file with Krona
``` bash
ktImportText lib3_krona_input.txt -o lib3_krona.html
explorer.exe lib3_krona.html
```

🤦[HONGO](hkoffianderson@gmail.com) 🔗[linkedIn](https://www.linkedin.com/in/koffi-anderson-hongo-b165a4170/)