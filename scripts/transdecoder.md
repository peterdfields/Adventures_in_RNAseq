##### Transdecoder get longest ORFs
`TransDecoder.LongOrfs -t transcripts.fasta`

##### Let's run Diamond to find matches to swissprot
`diamond blastp --query longest_orfs.pep --max-target-seqs 1 \ 
  --ultra-sensitive --threads 55 --db uniprot_sprot.dmnd --evalue 1e-5 -out --outfmt 6 \
  --out diamond.sample_name.outfmt6`

##### Now let's use hmmer to generate matches against pfam. This is super slow so I'm going to use parallels, see script hmmscan.sh, though the basic form is:
`hmmscan --cpu 1 --domtblout ${sample}.pfam.domtblout ~/bioinformatics/pfam/Pfam-A.hmm ${sample}`

##### let's split up the longest_orfs.pep into 100 smaller fasta files; pyfasta can be pulled using bioconda
`pyfasta split -n 100 longest_orfs.pep`

##### and we call it as follows:
`ls longest_orfs.pep.* | parallel -j55 -k bash hmscan.sh {}`

##### because the resultant files have a header we need to cut this and then cat these files; see trim_header.sh and use the same as hmmscan.sh
```
ls *.pfam.domtblout | parallel -j12 -k bash trim_headers.sh {}
cat *.out > compile.pfam.domtblout
head -n 3 longest_orfs.pep.000.pfam.domtblout | cat - compile.pfam.domtblout > pfam.transcripts.domtblout
```

##### now we can finish up the Transdecoder pipeline ande get our target proteins
```
TransDecoder.Predict -t transcripts.fasta --retain_pfam_hits pfam.transcripts.domtblout \
  --retain_blastp_hits diamond.transcripts.outfmt6
```
