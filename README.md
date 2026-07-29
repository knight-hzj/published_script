# Loop chaining analysis

This repository contains the scripts used for loop-chaining analysis
in SMCHD1 loss re-wires MYOD1 enhancer nexuses and chromatin accessibility landscapes in muscle cells.


## Requirements

- R version 4.3.2
- dplyr   
- data.table
- GenomicRanges
- AnnotationDbi
- TxDb.Hsapiens.UCSC.hg19.knownGene
- org.Hs.eg.db
- ggplot2
- ggrepel
- patchwork
- tibble
- igraph


## Input data

Sequencing data associated with this study are available from GEO under
accession GSE319422, GSE319423, GSE251749, GSE251747



## Usage
The workflow consists of three R scripts.

Step1: Identify MYOD1-associated enhancer modules.
Run:
Rscript Define_MYOD1_related_loop_chained_enhancers.R

This script:
Loads chromatin loops in BEDPE format.
Extends both anchors of each loop by 5 kb.
Retains loops for which both anchors are located within the same TAD.
Identifies enhancers that directly overlap MYOD1 ChIP-seq peaks.
Constructs an undirected loop-anchor graph within each TAD.
Starting from each MYOD1-bound enhancer, identifies additional enhancers connected through one or more loop hops.
Records the minimum number of loop hops between each directly MYOD1-bound enhancer and its related enhancers.
Annotates the direct and related enhancers with H3K27ac and H3K4me1 signal and classifies them as stitched enhancers or super-enhancers.

The script must be run separately for WT and SMCHD1-KO data, with the
condition-specific input and output filenames configured in the script.

Step 2: Link MYOD1 enhancer modules to gene promoters and calculate ABC scores
Run:
Rscript MYOD1_module_link_ABC_Modeling.R

This script:
Loads the WT and SMCHD1-KO chromatin loops.
Loads the MYOD1-related enhancer modules identified in Step 1.
Constructs hg19 gene promoters extending 2 kb upstream and downstream of each transcription start site.
Extends chromatin-loop anchors by 5 kb.
Identifies strict cross-anchor enhancer–promoter connections, meaning that one loop anchor overlaps a promoter and the opposite anchor overlaps an enhancer belonging to a MYOD1-related module.
Calculates the observed/expected contact weight for each enhancer–promoter pair.
Applies distance weighting using an exponent of 0.7 and a minimum enhancer–promoter distance of 5 kb.
Calculates a raw ABC-like score from enhancer activity and distance-weighted contact frequency.
Normalizes ABC scores per gene using candidate enhancer–promoter connections within a 2-Mb window.
Compares WT and SMCHD1-KO enhancer–gene links and calculates changes in ABC scores.
Integrates RNA-seq log2 fold changes and FDR values when the corresponding RNA-seq result file is supplied.

Step 3: Characterize enhancer–enhancer connectivity within each module

Run the nexus-building analysis after the functions and loop objects
required by Step 2 have been loaded:
source("MYOD1_module_link_ABC_Modeling.R")
source("Nexus_building.R")

This script:
Assigns hop 0 to directly MYOD1-bound enhancers.
Assigns the minimum loop-hop distance to each related enhancer.
Identifies enhancer–enhancer edges supported by chromatin loops.
Retains enhancer pairs belonging to the same MYOD1-related module.
Summarizes the internal organization of each module in WT and SMCHD1-KO cells.
Calculates the number of enhancers, maximum hop distance, connectivity between adjacent hop levels, non-adjacent edges and overall edge density for each module.



## Citation

If you use this code, please cite:

 “citation will be added after publication”

## Contact

Zhi-jun Huang  
Van Andel Institute  
zhijun.huang@vai.org

Gerd Pfeifer  
Van Andel Institute  
gerd.pfeifer@vai.org

