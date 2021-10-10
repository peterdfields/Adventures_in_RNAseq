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
