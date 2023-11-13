# MRC-CLIMB_Workshop_Gambia
This repository represents a showcase of Jupyter Notebooks, presented during the CLIMB-BIG-DATA Bioinformatics Workshop at the MRC Unit in The Gambia. The workshop is taking place between November 13th and 17th, 2023.

## Publication
Genomes and reads associated with this show-case tutorial is linked to the following publication: "Gene pool transmission of multidrug resistance among _Campylobacter_ from livestock, sewage and human disease" published in Environmental Microbiology (doi:10.1111/1462-2920.14760). All sequence data are linked to NCBI BioProject PRJNA528879. The bacterial genomes are available in GenBank under accession codes SRX5575129 to SRX5587545.

## Files Summary
The files include data for 169 isolates representing a collection sampled from human clinical cases, animals and sewage: Each directory contains the following:
* **Raw_Reads**: directory with raw reads data
* **Assemblies**: directory with assemblies
* **Core_alignment.fas**: Core alignment of 169 assembled genomes  
* **Phylogeny.nwk**: Maximum-likelihood tree reconstructed by FastTree  
* **_Visualization_script.R_**: rscript for compiling phylogeny and metadata
* **Metadata.csv**: Genome metadata .csv file used for visualization  

## Software and packages
* SPAdes v3.15.5 
* checkM v1.1.6
* Snippy 4.4.3
* MLST v2.23.0
* PIRATE
* RAxML v8.2.11 
* Abricate

## R packages
* ggplot2
* ggtree
* ape

# Workflow
## Reads assembly (with SPAdes)
```
spades.py –12 short_read_files –phred-offset 33 -o output_directory
```
## Genome quality control (with checkM)
```
checkm lineage_wf -t 30 -x fas ./contigs/ /home/ubuntu/Output_directory
```
## Assigning MLST using pubMLST schemes
```
mlst genome.fas
```
## Generating a core genome alignment
```
snippy --ref ref.gbk --outdir isolate1 --ctgs ./contigs/isolate1.fas (submission script)
```
## Creating a pangenome using PIRATE (optional)
```
perl ./bin/PIRATE -i input_directory (directory with gff files) -o output_directory -a -r
```
## Reconstructing a neighbor-joining phylogeny (using core alignment generated)
```
rapidnj core_alignment.fas -i sth
```
## Reconstructing a Maximum-likelihood phylogeny (using core alignment generated)
```
raxmlHPC-PTHREADS -s core_alignment.fas -n core_phylogeny.nwk -T 8 -m GTRGAMMA -p 12345 
```
## Screening assemblies for antimicrobial resistance genetic determinants
```
for file in *.fas; do abricate --db amrfinderplus $file > abricate-amrfinderplus/$(basename $file .fas).abricate.tsv; done
```
# Visualization using Rstudio
## Visualizing tree phylogeny and metadata using ggtree, ggplot2 and ape packages
