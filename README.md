# Kraken2
Classify any sequences with kraken2
## Download provided kraken2 database
## Custom kraken2 database
1. Install a taxonomy. Usually, you will just use the NCBI taxonomy, which you can easily download using:
```bash
kraken2-build --download-taxonomy --db $DBNAME
```
This command generate a "taxonomy" folder.

2. Contruct the library

Collect fasta file from ncbi or other source put them on "refseq" folder. Then run add library command:
Unzip file
```bash
Find refseq -name "*.gz" -print0 | parallel -0 gunzip
```
Add reference sequence to custom database
```bash
find refseq -name "*.fna" -exec kraken2-build --add-to-library {} --db custom_db \;
```
This command generate a "library" in custom_db.

3. Build kraken custom database
custom_db must contain "library" and "taxonomy" from step 1 & 2.
Build custom database:
```bash
kraken2-build --db custom_db --build --threads 24
```
4. Classify sequence:
Sequences stored in fasta file.
```bash
kraken2 --use-names --db custom_db/ --threads 16 --confidence 0.1 --report repseqs.report rep-seqs.fasta --threads 20 > repseqs.kraken
```
Sequences stored in raw read file.
```bash
kraken2 --use-names --db custom_db/ --threads 16 --confidence 0.1 --report repseqs.report --gzip-compressed read1 read2 --threads 20 > repseqs.kraken
```
