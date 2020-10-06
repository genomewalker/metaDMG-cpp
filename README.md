# metaDamage2.0
Fast and efficient method for error and damage rate estimates. Possible modes for running. Program is utilizing mdz field of the aux part of reads and is therefore reference free.

1. Basic single genome analysis with one overall global estimate. Similar to mapdamage1.0 and mapdamage2.0.

2. Basic eDNA or metagenomic analyses. Output is a damage estimate for each referenceID.

3. Lowest common ancestor is used for determining specificity of alignments for the analyses.

For all analyses output is a binary '.bdamage.gz' file, that can be accessed with the 'metadamage print' functionality.


# Single genome analysis
`./metadamage getdamage -l 10 -p 5 --threads 8 input.bam`

All options found below:

```
./metadamage getdamage

Usage: metadamage getdamage [options] <in.bam>|<in.sam>|<in.cram>

Example: ./metadamage getdamage -l 10 -p 5 --threads 8 ../data/subs.sam
Options:
  -f/--fasta	 is required with CRAM
  -l/--minlength	 reads shorter than minlength will be discarded
  -r/--runmode	runmode 1 means that damage patterns will be calculated for each chr/scaffold contig.
		runmode 0 means one global estimate.
  -@/--threads	 Number of threads used for reading/writing
```

# Multiple genome analysis
` ./metadamage getdamage -l 10 -p 5 --threads 8 input.bam -r 1 `

All options found below:

```
./metadamage getdamage

Usage: metadamage getdamage [options] <in.bam>|<in.sam>|<in.cram>

Example: ./metadamage getdamage -l 10 -p 5 --threads 8 input.bam
Options:
  -f/--fasta	 is required with CRAM
  -l/--minlength	 reads shorter than minlength will be discarded
  -r/--runmode	runmode 1 means that damage patterns will be calculated for each chr/scaffold contig.
		runmode 0 means one global estimate.
  -@/--threads	 Number of threads used for reading/writing
```

# LCA analyses

` ./metadamage lca -names names.dmp.gz -nodes nodes.dmp.gz -acc2tax taxid_accssionNO.gz -simscorelow 0.95 -simscorehigh 1.0 -minmapq 30 -howmany 15 -bam input.bam`

All options found below:

```
./metadamage lca 

Usage: metadamage lca [options] -names -nodes -acc2tax [-editdist[min/max] -simscore[low/high] -minmapq -discard] -bam <in.bam>|<in.sam>|<in.sam.gz>

Example ./metadamage lca -names names.dmp.gz -nodes nodes.dmp.gz -acc2tax taxid_accssionNO.gz -simscorelow 0.95 -simscorehigh 1.0 -minmapq 30 -howmany 15 -bam input.bam

Options:
  -simscorelow	integer between 0-1
  -simscorehigh	integer between 0-1
  -names 	/willerslev/users-shared/science-snm-willerslev-npl206/ngsLCA/ngsLCA/ncbi_tax_dump_files/names.dmp.gz 
  -nodes 	/willerslev/users-shared/science-snm-willerslev-npl206/ngsLCA/ngsLCA/ncbi_tax_dump_files/nodes.dmp.gz 
  -acc2tax 	/willerslev/users-shared/science-snm-willerslev-zsb202/soft/ngslca/accession2taxid/combined_taxid_accssionNO_20200425.gz 
  -bam		input alignment file in bam, sam or sam.gz format
  -outnames	output_prefix
  -lca_rank	family/genus/species 
  -discard 
  -minmapq	integer for minimum mapping quality
  -howmany	integer for many positions
```


# ./metadamage print

`./metadamage print file.bdamage.gz [-names file.gz -bam file.bam -ctga -countout] infile: (null) inbam: (null) names: (null) search: -1 ctga: 0 `

All options found below:

```
 ./metadamage print 
 
 Usage: metadamage print 
 
Example ./metadamage print file.bdamage.gz -names names.dmp.gz 
	./metadamage print file.bdamage.gz -r 9639 -ctga
	./metadamage print file.bdamage.gz -countout 
Options:
  -ctga		ONLY print CT+ and GA- (the damage ones)
  -countout	print mismatch as counts and not as transition probabilites
  -r taxid	Only print for specific taxid
  -names	NCBI names.dmp.gz file - option to print taxonomic names to output
  -bam		print referencenames from bamfile, otherwise it prints integeroffset. 


#### Header in print: taxid,nralign,orientation,position,AA,AC,AG,AT,CA,CC,CG,CT,GA,GC,GG,GT,TA,TC,TG,TT 
 ```
 
# ./metadamage merge 

`./metadamage merge file.lca file.bdamage.gz ` 


All options found below:

```
./metadamage merge 

Usage: ./metadamage merge file.lca file.bdamage.gz [-names names.dmp.gz -bam <in.bam>|<in.sam>|<in.sam.gz> -howmany 5 -nodes nodes.dmp.gz]

Example
Options:
-howmany #integer for many positions ?? Thorfinn is this also working for the merge module? Or only at the LCA module? 
-nodes #needs taxonomic paths to calculate damage higher than species level /willerslev/users-shared/science-snm-willerslev-npl206/ngsLCA/ngsLCA/ncbi_tax_dump_files/nodes.dmp.gz
-names #NCBI names.dmp file - option that prints taxonomic names to output  
```

# ./metadamage mergedamage files.damage.*.gz

# ./metadamage index files.damage.gz

