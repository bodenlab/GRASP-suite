---
title: GRASP-cmd
summary: Command line version of GRASP. |[Download bnkit.JAR](project/graspcmd/archive/bnkit_20211215.jar) | [Download legacy bnkit.JAR](project/graspcmd/archive/bnkit_legacy.jar)
tags:
- Inference
date: "2020-03-09T00:00:00Z"

# Optional external URL for project (replaces project detail page).

image:
  focal_point: Smart
---

## GRASP command-line interface

For users who would like to automate ASR, there is now a command-line version of GRASP that contains all of the indel inference methods. Indels are encoded using either Position Specific (PS), Simple Indel Coding (SIC), or Bi-directional Edge (BE) methods, and inferred using either Parsimony (P) or Maximum Likelihood (ML).

This gives six methods - PS-P, PS-ML, SIC-P, SIC-ML, BE-P, BE-ML.

It is implemented in [bnkit](https://github.com/bodenlab/bnkit) on the branch 'POG2020' as a class `asr.GRASP`.

there is also alimited-feature, legacy command-line version of GRASP. It is implemented in [bnkit](https://github.com/bodenlab/bnkit) on the `master` branch as a class `reconstruction.GraspCmd`.

### GraspCmd: What can it do?

GraspCmd accepts an alignment (FASTA or Clustal formats) and phylogenetic tree (Newick format) with perfectly concordant labels, to infer ancestor sequences by joint or marginal reconstruction by maximum likelihood. In the process, the program also infers the most parsimonious insertion and deletion events which are internally represented via partial-order graphs; it also identifies the _most supported_ path of sequence inclusions at each ancestor.

This command line version of the web service has currently no function to save the (potentially more complex) partial-order graph, but the most supported path always identifies a sequence. The program saves all ancestor sequences (in the case of joint reconstruction, again as FASTA or Clustal) or one sequence (in the case of marginal reconstrution; optionally with character state distributions as a TSV file). It can also re-save the tree with assigned ancestor labels.

GRASP was designed primarily for protein sequences but command-line version 0309.2020 incorporates a DNA model "Yang" (based on "REV" in Yang Z, *J Mol Evol* 1994). At this stage we have not tested DNA sequence functionality extensively, nor have we developed specific features around DNA sequences (codon-centric analyses, user-provided background stats, etc).


### Access through Docker

GRASP is available through Docker Hub at `gabefoley/grasp`

Once you have docker installed you can


```console
docker run -it -v {full path to where your data is located}:/data gabefoley/grasp grasp -aln /data/{name of your alignment file}.aln -nwk /data/{name of your newick file}.nwk -out /data
```

for example, for me the command looks like (I have test_6.aln and test_6.nwk sitting in a /data folder):

```console
docker run -it -v /Users/coolusername/Documents/code/grasp/data:/data grasp-docker grasp -aln /data/test_6.aln -nwk /data/test_6.nwk -out /data
```

This should give you a file, GRASP_ancestors.fasta appearing in folder: /Users/coolusername/Documents/code/grasp/data.


### GraspCmd: How do I make it work on my computer?

First, you will need Java version 8 or newer. Any operating system with Java should work, including Mac OS, MS Windows and Linux.

Then, you have a choice: you can clone/download [bnkit](https://github.com/bodenlab/bnkit) in its entirety. You may need JUnit 5 testing to get everything working; this is only required if you want to run software tests, say if you are a developer.

Alternatively, just download the pre-compiled version with all indel inference methods [bnkit JAR file](archive/bnkit_20211215.jar). 

Or, the legacy version [bnkit JAR file](archive/bnkit_legacy.jar).


### GraspCmd: How do I run it? 

1. Download the jar file

2. Create a bash script grasp that contains the following two lines, replacing the path with the path to your downloaded jar

`
#!/bin/sh
`

`
java -jar </path/to/filename.jar> $@
`

3. Change permissions on the bash script
```console
chmod 755 grasp
```

4. Place grasp where you store your executable files, for example - /usr/local/bin
```console
mv grasp /usr/local/bin`
```

5. Check it works
```console
grasp -h
```


This will print out the arguments that specifies your input data and options.

A typical command may look like this
```
grasp -aln 500_2112_dhad_18032019.aln -nwk r_500_2112_dhad_18032019.nwk -out recon_0500.aln -verbose -gap -thr 5
```

### What else?

Running the command-line version is typically a quicker affair, at least for smaller reconstructions, but it requires decent hardware. A reconstruction of less than 1,000 sequences should take less than 10 minutes.

You can probably run a reconstruction with 10,000 sequences on a server, but how "gappy" the alignment is will also play a part in deciding this. If the alignment is reasonably clean, a powerful, modern laptop with at least 16GB of memory, can do this in under a day. If the alignment covers a diverse family, you will probably need a lot more memory. We recommend you set the Java heap size to 60GB RAM, which you can using the option -Xmx60000m.

The rough estimates above assume you use multiple threads; we recommend 5 or so on decent hardware (-thr 5).
