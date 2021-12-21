---
title: GRASP-cmd
summary: Command line version of GRASP.<br><br>[Download bnkit.JAR file](project/graspcmd/archive/bnkit.jar) <br><br> [Download legacy bnkit.JAR file](project/graspcmd/archive/bnkit_legacy.jar)
tags:
- Inference
date: "2020-03-09T00:00:00Z"

# Optional external URL for project (replaces project detail page).

image:
  focal_point: Smart
---

## GRASP command-line interface

Users who would like to automate ASR, there is now a command-line version of GRASP that contains all of the indel inference methods. Indels are encoded using either Position Specific (PS), Simple Indel Coding (SIC), or Bi-directional Edge (BE) methods, and inferred using either Parsimony (P) or Maximum Likelihood (ML).

This gives six methods - PS-P, PS-ML, SIC-P, SIC-ML, BE-P, BE-ML.

It is implemented in [bnkit](https://github.com/bodenlab/bnkit) on the branch 'POG2020' as a class `asr.GRASP`.

there is also alimited-feature, legacy command-line version of GRASP. It is implemented in [bnkit](https://github.com/bodenlab/bnkit) on the `master` branch as a class `reconstruction.GraspCmd`.

### GraspCmd: What can it do?

GraspCmd accepts an alignment (FASTA or Clustal formats) and phylogenetic tree (Newick format) with perfectly concordant labels, to infer ancestor sequences by joint or marginal reconstruction by maximum likelihood. In the process, the program also infers the most parsimonious insertion and deletion events which are internally represented via partial-order graphs; it also identifies the _most supported_ path of sequence inclusions at each ancestor.

This command line version of the web service has currently no function to save the (potentially more complex) partial-order graph, but the most supported path always identifies a sequence. The program saves all ancestor sequences (in the case of joint reconstruction, again as FASTA or Clustal) or one sequence (in the case of marginal reconstrution; optionally with character state distributions as a TSV file). It can also re-save the tree with assigned ancestor labels.

GRASP was designed primarily for protein sequences but command-line version 0309.2020 incorporates a DNA model "Yang" (based on "REV" in Yang Z, *J Mol Evol* 1994). At this stage we have not tested DNA sequence functionality extensively, nor have we developed specific features around DNA sequences (codon-centric analyses, user-provided background stats, etc).

### GraspCmd: How do I make it work on my computer?

First, you will need Java version 8 or newer. Any operating system with Java should work, including Mac OS, MS Windows and Linux.

Then, you have a choice: you can clone/download [bnkit](https://github.com/bodenlab/bnkit) in its entirety. You may need JUnit 5 testing to get everything working; this is only required if you want to run software tests, say if you are a developer.

Alternatively, just download the pre-compiled version with all indel inference methods [bnkit JAR file](archive/bnkit.jar). 

Or, the legacy version [bnkit JAR file](archive/bnkit_legacy.jar).


### GraspCmd: How do I run it? 

Optionally, we suggest that you save the JAR file somewhere where everyone on your system can read it, e.g. `/Users/mikael/GRASP-suite/archive/bnkit.jar`. Then, you create a script [`grasp`](bin/grasp) to run it, which you save where other applications are kept on your system, e.g. `/Users/mikael/GRASP-suite/bin/`.

Once you have the JAR file, type
```console
  java -jar bnkit.jar -help
```
or if you are running the JAR file through the script above
```console
  grasp -help
```

This will print out the arguments that specifies your input data and options.

```console
Usage: GraspCmd 
	[-aln <alignment-file> -nwk <tree-file> -out <output-file>]
	{-model <JTT(default)|Dayhoff|LG|WAG>}
	{-thr <n-threads>}
	{-joint (default) | -marg <branchpoint-id>} 
	{-gap}
	{-savetree <tree-file>}
	{-format <FASTA(default)|CLUSTAL|DISTRIB>}
	{-verbose}{-help}
where 
	alignment-file is a multiple-sequence alignment on FASTA or CLUSTAL format
	tree-file is a phylogenetic tree on Newick format
	output-file will be populated by inferred ancestor or ancestors
	Inference is either joint (default) or marginal (marginal requires a branch-point to be nominated)
	"-gap" means that the gap-character is included in the resulting output (default for CLUSTAL format)
	"-savetree" re-saves the tree on Newick with ancestor names
	The output file is written on the specified format.
	-verbose will print out information about steps undertaken, and the time it took to finish.
Notes: 
	Greater number of threads may improve processing time, but implies greater memory requirement (default is 1).
	Evolutionary models include Jones-Taylor-Thornton (default), Dayhoff-Schwartz-Orcutt, Le-Gasquel and Whelan-Goldman.
	~ This is version 1027.2019 ~
```

A typical command may look like this
```
grasp -aln 500_2112_dhad_18032019.aln -nwk r_500_2112_dhad_18032019.nwk -out recon_0500.aln -verbose -gap -thr 5
```

### What else?

Running the command-line version is typically a quicker affair, at least for smaller reconstructions, but it requires decent hardware. A reconstruction of less than 1,000 sequences should take less than 10 minutes.

You can probably run a reconstruction with 10,000 sequences on a server, but how "gappy" the alignment is will also play a part in deciding this. If the alignment is reasonably clean, a powerful, modern laptop with at least 16GB of memory, can do this in under a day. If the alignment covers a diverse family, you will probably need a lot more memory. We recommend you set the Java heap size to 60GB RAM, which you can using the option -Xmx60000m.

The rough estimates above assume you use multiple threads; we recommend 5 or so on decent hardware (-thr 5).
