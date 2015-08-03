### News ###
27 January 2014: Release of PMDtools v0.50.

### Description ###
PMDtools implements a likelihood framework incorporating postmortem damage (PMD), base quality scores and biological polymorphism to identify degraded DNA sequences that are unlikely to originate from modern contamination. Using the model, each sequence is assigned a _PMD score_, for which positive values indicate support for the sequence being genuinely ancient. For details of the method, please see the main paper in _[PNAS](http://www.pnas.org/content/early/2014/01/23/1318934111.abstract)_.

In addition, PMDtools also offers PMD-aware base quality score adjustment and investigation of damage patterns.

PMDtools takes [SAM](http://samtools.sourceforge.net/SAM1.pdf)-formatted input, and requires an MD tag with alignment information. The MD tag is featured in the output of many aligners but can otherwise be added _e.g._ using the [SAMtools](http://samtools.sourceforge.net/) fillmd/calmd tool (Li, Handsaker et al. 2009).

No external packages except for python 2.6 are required, but for manipulating BAM files, [SAMtools](http://samtools.sourceforge.net/) is recommended. Extra plotting requires R.

Questions can be addressed to `pontus.skoglund@gmail.com`.


### Basic usage ###

To restrict to sequences with a PMD score of at least 3, enter:

```
samtools view -h mybam.bam | python pmdtools.py --threshold 3 --header | samtools view -Sb - > mybam.pmds3filter.bam
```



To compute deamination-derived damage patterns, enter:

```
samtools view mybam.bam | python pmdtools.py --deamination --range 30 > PMD_temp.txt
R CMD BATCH plotPMD.R
cp PMD_plot.pdf mybam.PMD_plot.pdf
```


PMDtools also computes damage patterns from sequence libraries in which damage has been repaired, e.g. using uracil–DNA–glycosylase and endonuclease VIII. This is done by restricting to nucleotides in a CpG context, for which deamination of Cytosine results in Thymine. Enter:

```
samtools view mybam.bam | python pmdtools.py --deamination --range 30 --CpG
```



For a full list of options, enter

```
python pmdtools.py --help
```

### Citation ###
Please cite:
P Skoglund, BH Northoff, MV Shunkov, A Derevianko, S Pääbo, J Krause, M Jakobsson (2014) [Separating ancient DNA from modern contamination in a Siberian Neandertal](https://docs.google.com/file/d/0BzxpmkTnJX5AZWxLaHh1Q1hheXc/),  Proceedings of the National Academy of Sciences USA [doi:10.1073/pnas.1318934111](http://dx.doi.org/10.1073/pnas.1318934111)