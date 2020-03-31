# Workflow ASR

### Install key programs

- BLAST+ executables (NCBI commandline tool; https://blast.ncbi.nlm.nih.gov/Blast.cgi?CMD=Web&PAGE_TYPE=BlastDocs&DOC_TYPE=Download)

- Python distribution v3.x, recommend Anaconda (https://www.anaconda.com/distribution/)

  - Python scripts will help automate many tasks below

- CD-HIT (commandline tool); install is easy if Anaconda has been installed earlier

  `conda install -c bioconda cd-hit`

- Sequence alignment program, recommend Clustal Omega (commandline tool)

- Alignment viewer, recommend AliView

- Phylogenetic tree inference program, recommend FastTree (commandline tool)

- Phylogenetic tree viewer, recommend FigTree (well-designed) or Archaeopteryx (for large trees)

### Decide on a namespace/reference database, e.g. UniProt

- Source for identifiers from, key annotations, etc. Every sequence will (ideally) have an identifier from this source.

### Collect key proteins: two tiers

- Tier 1: "Guide" entries, proteins that are required in the final data set

  Collect as FASTA or name list, e.g. `collect_tier1.fa`; if identifiers or sequence are not known we will find them using a BLAST search from our reference database below.

- Tier-2: POI entries (proteins of interest), prioritise inclusion, but if redundant, choose one

  Collect as FASTA or name list, e.g. `collect_tier2.fa`; like above, identifiers/sequences will be retrieved using a BLAST search

- Devise a strategy with which a root can be placed in the eventual phylogenetic tree and amend Tier-1 and Tier-2 lists; this might involve having examplars in Tier-1 and Tier-2 that can work as outgroups or anchor points (by reference to published phylogenetic tree of the same family)

### Download all members of protein family

- In UniProt, decide on a family identifier, e.g. "DapA", and remove questionable entries, e.g. sequence fragments
- Execute search and download as FASTA, e.g. go to https://www.uniprot.org type in query `family:"dapa family" fragment:no`, download as FASTA, save as `db-100.fa`

### Create BLAST database

- makeblastdb (program part of BLASTx suite) on db-100
  - For example, `makeblastdb -dbtype prot -in db-100.fa -out db-100`

### Create Tier-1 and Tier-2 files

- Use the BLAST database db-100 to re-generate Tier-1 and Tier-2 files, so they are associated with identifiers and sequences from reference database
  - For example, `blastp -db db-100 -outfmt 3 -num_descriptions 1 -num_alignments 0 -num_threads 5 -query collect_tier1.fa -out tier-1.txt -evalue 1e-100`; this will find the sequence with the highest similarity to that given, provided it is statistically supported at E=1e-100
  - Extract sequence identifiers from BLAST report, e.g. `grep -e "^[st][pr]" tier-1.txt | cut -d' ' -f1 > tier-1.tab`; from this create `tier-1.fa` 

### Constrain representation of protein family using key proteins

- Use program such as CD-HIT to filter sequences at different levels of redundancy allowed, e.g. 99%, 95%, 90%, 50% (we call these db-99, db-95, etc)
  - For example, `cd-hit -i Downloaded.fa -c 0.99 -T 5 -o db-99 -d 0` generates a file `db-99.clstr`; We do *not* use the FASTA file created by CD-HIT
  - For each sequence similarity "cluster" identified (in `db-99.clstr`), keep *all* Tier-1 entries, when no Tier-1 entries are in the cluster, keep exactly *one* Tier-2 entry if available; only when no Tier-1 or Tier-2 entries are in the cluster, pick an entry randomly [this step needs to be automated]; from this, create `db-99.fa`, `db-95.fa`, etc

- Make BLAST databases for each redundancy level, db-99, db-95, etc
  - `makeblastdb -dbtype prot -in db-99.fa -out db-99`

### Create datasets

- Search Tier-1 entries in db-100 including all homologs closer than a given E-value (e.g. 1e-100)
  - For example, `blastp -db db-100 -outfmt 3 -num_descriptions 20000 -num_alignments 0 -num_threads 5 -query tier-1.fa -out dataset-99.txt -evalue 1e-100`
  - Extract sequence identifiers from BLAST report, e.g. `grep -e "^[st][pr]" dataset-1.txt | cut -d' ' -f1 > dataset-1.tab`; from this create `dataset-1.fa`
  - Mark Tier-1 and Tier-2 entries in dataset, with distinctive labels etc so that they can be searched for later
- Try different combinations of redundancy levels (db-99, db-95, ...) and E-value thresholds (1e-100, 1e-75, 1e-50, 1e-10, etc) to construct alternative datasets
  - Redundancy level will control how dense the overall sequence composition will be
  - E-value threshold will control how closely we sample around the Tier-1 entries

### Perform sequence alignment

- Use multiple sequence alignment program to establish homologous positions between all entries in a dataset
  - For example, `clustalo -i dataset-1.fa -o dataset-1.aln --output-order=tree-order --threads=5 --force`
- Inspect alignment to evaluate quality
  - For example, use AliView, zoom out so the whole alignment can be viewed

- Check for sole-sequence insertions (and deletions), and remove if incorrectly aligned, and disruptive of the the overall alignment; re-run alignment once cleaned

### Perform phylogenetic tree inference

- Use tree inference program to create an unrooted phylogenetic tree
  - For example, `FastTree -nosupport -out dataset-1_unrooted.nwk dataset-1.fa`
- Inspect tree to place a root and to evaluate quality
  - For example, use FigTree or Archaeopteryx
- Search for markers of Tier-1 and Tier-2 entries to find appropriate place for root, placed at branch point in unrooted tree



