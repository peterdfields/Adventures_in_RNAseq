##### Generate a reference database (no annotations provided)
`STAR --runThreadN 12 --runMode genomeGenerate --genomeDir reference_star_index --genomeFastaFiles reference.fasta --genomeSAindexNbases 12`

##### Align RNAseq data to reference (no annotations provided)
```
STAR --genomeDir WJBH01.1_star_index/ --runThreadN 12 \ 
  --readFilesIn rnaseq/trimmed/SRR10389290.R1.fq.gz rnaseq/trimmed/SRR10389290.R2.fq.gz \ 
  --readFilesCommand zcat --outFileNamePrefix SRR10389290 \ 
  --outSAMtype BAM SortedByCoordinate --outSAMunmapped Within --outSAMattributes Standard
```
