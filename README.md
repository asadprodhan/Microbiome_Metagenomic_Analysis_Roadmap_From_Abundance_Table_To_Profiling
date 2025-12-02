<h1 align="center">Microbiome Metagenomic Analysis Roadmap: From Abundance Table To Taxonomic and Functional Profilling</h1>

<h3 align="center">M. Asaduzzaman Prodhan<sup>*</sup> </h3>

<div align="center"><b> School of Biological Sciences, The University of Western Australia </b></div>

<div align="center"><b> 35 Stirling Highway, Perth, WA 6009, Australia. <sup>*</sup>Correspondence: prodhan82@gmail.com </b></div>

<br />

<p align="center">
  <a href="https://github.com/asadprodhan/Microbiome_Metagenomic_Analysis_Roadmap_From_Abundance_Table_To_Profiling#GPL-3.0-1-ov-file"><img src="https://img.shields.io/badge/License-GPL%203.0-yellow.svg" alt="License GPL 3.0" style="display: inline-block;"></a>
  <a href="https://orcid.org/0000-0002-1320-3486"><img src="https://img.shields.io/badge/ORCID-green?style=flat-square&logo=ORCID&logoColor=white" alt="ORCID" style="display: inline-block;"></a>
</p>

<br />


---

# **CONTENT**

## **Section 1: Community Ecology**
1.1 Alpha diversity extraction (Script 1)  
1.2 Alpha diversity statistics (Script 2)  
1.3 Beta diversity (Script 3)  
1.4 Phylum-level abundance (Script 4)  
1.5 Genus-level abundance (Script 5)  

## **Section 2: Co-occurrence Networks and Keystone Taxa**
2.1 Co-occurrence network and topology (Script 6)  
2.2 Keystone taxa abundance tables (Script 7)  
2.3 Core vs treatment-specific keystone taxa (Script 8)  

## **Section 3: Differential Abundance**
3.1 DESeq2 with restored taxonomy (Script 9)  
3.2 DESeq2 volcano plots for all taxa (Script 10)  
3.3 DESeq2 at phylum level (Script 11)  
3.4 ALDEx2 differential abundance (Script 12)  

## **Section 4: Functional Metagenomics (HUMAnN)**
4.1 HUMAnN pathway visualisation (Script 13)  
4.2 HUMAnN functional contributors (Script 14)  
4.3 HUMAnN gene family heatmaps (Script 15)  

<br />

---

# **COMMON INPUTS**

## Metadata
**File**: `MetaData_StudySamples.csv`  
Contains:
- Sample IDs  
- Treatment groups: `T1.Control`, `T2.TreatmentA`, `T3.TreatmentB`

## Community Table
**File**: `Metagenome_CommunityProfile.biom`

Taxonomy format used:
`Kingdom, Phylum, Class, Order, Family, Genus, Species`

<br />

---

# **SECTION 1: COMMUNITY ECOLOGY**

---

## 1.1 **Alpha Diversity: Data Extraction (Script 1)**  
**File**: `Script1_AlphaDiversity_Extraction.R`

### Why  
To compute alpha diversity metrics after rarefaction.

### Input  
- `MetaData_StudySamples.csv`  
- `Metagenome_CommunityProfile.biom`

### What was done  
- Loaded phyloseq and tidyverse  
- Built phyloseq object  
- Cleaned taxonomy  
- Rarefied samples  
- Computed Chao1, Shannon, Simpson  
- Merged with metadata  

### Output  
- `AlphaDiversity_WithMetadata.csv`

---

## 1.2 **Alpha Diversity: Statistics & Boxplots (Script 2)**  
**File**: `Script2_AlphaDiversity_StatsPlots.R`

### Why  
To test differences in alpha diversity across treatments.

### Input  
- `AlphaDiversity_WithMetadata.csv`  
- `MetaData_StudySamples.csv`

### What was done  
- Kruskal–Wallis tests  
- Pairwise post-hoc tests  
- Compact letter assignment  
- Publication-quality boxplots  

### Output  
- `AlphaDiversity_Boxplots.pdf`  
- `AlphaDiversity_Stats.csv`

---

## 1.3 **Beta Diversity: Ordination & PERMANOVA (Script 3)**  
**File**: `Script3_BetaDiversity.R`

### Why  
To assess community composition differences.

### Input  
- `MetaData_StudySamples.csv`  
- `Metagenome_CommunityProfile.biom`

### What was done  
- Bray–Curtis distance  
- NMDS / PCoA ordination  
- PERMANOVA  

### Output  
- `BetaDiversity_Ordination.pdf`  
- `BetaDiversity_PERMANOVA.csv`

---

## 1.4 **Phylum-Level Abundance Tables (Script 4)**  
**File**: `Script4_PhylumAbundance.R`

### Why  
To summarise phylum-level community composition.

### Input  
- Metadata  
- BIOM file  

### What was done  
- Aggregated to Phylum  
- Computed absolute and relative abundance  

### Output  
- `Phylum_Abundance_Absolute.csv`  
- `Phylum_Abundance_Relative.csv`

---

## 1.5 **Genus-Level Abundance Tables (Script 5)**  
**File**: `Script5_GenusAbundance.R`

### Why  
To generate genus-level abundance matrices for visualisation and downstream use.

### Input  
Same as Script 4.

### What was done  
- Aggregation to Genus  
- Exported wide abundance tables  

### Output  
- `Genus_Abundance_Absolute.csv`  
- `Genus_Abundance_Relative.csv`

---

# **SECTION 2: CO-OCCURRENCE NETWORKS AND KEYSTONE TAXA**

---

## 2.1 **Co-occurrence Network & Node Topology (Script 6)**  
**File**: `Script6_CooccurrenceNetwork.R`

### Why  
To infer co-occurrence networks and identify central taxa.

### Input  
- Metadata  
- BIOM table  

### What was done  
- Split phyloseq by treatment  
- Network inference  
- Computed centrality metrics  
- Exported node tables  

### Output  
- `NodeTopology_T1.csv`  
- `NodeTopology_T2.csv`  
- `NodeTopology_T3.csv`

---

## 2.2 **Keystone Taxa Abundance Tables (Script 7)**  
**File**: `Script7_KeystoneTaxaAbundanceTable.R`

### Why  
To extract treatment-wise abundance tables for keystone genera.

### Input  
- Metadata  
- BIOM table  
- Keystone lists from network topology files  

### What was done  
- Loaded phyloseq and tidyverse  
- Subset by treatment  
- Subset by keystone genus list  
- Exported abundance matrices  

### Output  
- `KeystoneAbundance_T1.tsv`  
- `KeystoneAbundance_T2.tsv`  
- `KeystoneAbundance_T3.tsv`

---

## 2.3 **Core vs Treatment-Specific Keystone Taxa (Script 8)**  
**File**: `Script8_Keystone_VennAnalysis.R`

### Why  
To identify shared (core) and unique keystone taxa.

### Input  
- `NodeTopology_T1.csv`  
- `NodeTopology_T2.csv`  
- `NodeTopology_T3.csv`

### What was done  
- Extracted keystone genera  
- Calculated intersections  
- Generated Venn diagram  
- Exported summary tables  

### Output  
- `Core_KeystoneTaxa.csv`  
- `KeystoneUnique_T1.csv`  
- `KeystoneUnique_T2.csv`  
- `KeystoneUnique_T3.csv`  
- `Keystone_VennDiagram.pdf`

---

# **SECTION 3: DIFFERENTIAL ABUNDANCE**

---

## 3.1 **DESeq2 Differential Abundance with Taxonomy (Script 9)**  
**File**: `Script9_DESeq2_WithTaxonomy.R`

### Why  
To identify differentially abundant taxa with taxonomy attached.

### Input  
- Metadata  
- BIOM table  

### What was done  
- Converted to DESeq2 object  
- Performed Wald tests  
- Extracted contrasts  
- Merged results with taxonomy  
- Created volcano plots  

### Output  
- `DESeq2_TaxaLevel_Results.csv`  
- `DESeq2_TaxaLevel_Volcano.pdf`

---

## 3.2 **DESeq2 Volcano Plots for All Taxa (Script 10)**  
**File**: `Script10_DESeq2_AllTaxa.R`

### Why  
To visualise all taxa in differential abundance contrasts.

### Input  
- Metadata  
- BIOM table  

### What was done  
- DESeq2 modelling  
- Extracted contrasts  
- Annotated volcano plots  

### Output  
- `DESeq2_AllTaxa_Results.csv`  
- `DESeq2_AllTaxa_VolcanoPlot.pdf`  
- `DESeq2_AllTaxa_VolcanoPlot.png`

---

## 3.3 **DESeq2 Phylum-Level (Script 11)**  
**File**: `Script11_DESeq2_Phylum.R`

### Why  
To analyse differential abundance at the Phylum level.

### Input  
- Metadata  
- BIOM  
- `Phylum_Abundance_Absolute.csv`

### What was done  
- Aggregated to Phylum  
- Built DESeq2 dataset  
- Generated volcano plots  

### Output  
- `DESeq2_PhylumLevel_All.csv`  
- `DESeq2_PhylumLevel_VolcanoPlot.pdf`  
- `DESeq2_PhylumLevel_VolcanoPlot.png`

---

## 3.4 **ALDEx2 Differential Abundance (Script 12)**  
**File**: `Script12_ALDEx2.R`

### Why  
To perform CLR-based differential abundance testing.

### Input  
- `Genus_Abundance_Absolute.csv`  
- Metadata  

### What was done  
- CLR transformation  
- ALDEx2 testing  
- Effect size visualisation  

### Output  
- `ALDEx2_GenusLevel_Results.csv`  
- `ALDEx2_EffectPlot.pdf`  
- `ALDEx2_EffectPlot.png`

---

# **SECTION 4: FUNCTIONAL METAGENOMICS (HUMAnN)**

---

## 4.1 **HUMAnN Merged Pathway Visualisation (Script 13)**  
**File**: `Script13_HUMAnN_PathwayViz.R`

### Why  
To visualise differences in pathway-level functional profiles.

### Input  
- `HUMAnN_Pathways_Merged.tsv`  
- Metadata  

### What was done  
- Cleaned and normalised pathway abundance  
- Selected top pathways  
- Generated heatmaps and summaries  

### Output  
- `HUMAnN_Pathways_Heatmap.pdf`  
- `HUMAnN_Pathways_Top20.csv`  
- `HUMAnN_Pathways_TreatmentSummary.csv`

---

## 4.2 **HUMAnN Functional Contributors (Script 14)**  
**File**: `Script14_HUMAnN_FunctionalContributors.R`

### Why  
To determine which taxa contribute to functional pathways.

### Input  
- `HUMAnN_Pathways_Stratified.tsv`  
- Metadata  

### What was done  
- Parsed contributors  
- Computed contributions  
- Generated contributor heatmaps  

### Output  
- `HUMAnN_TopContributors.csv`  
- `HUMAnN_ContributorHeatmap.pdf`  
- `HUMAnN_ContributorBarplots.png`

---

## 4.3 **HUMAnN Gene Family Heatmaps (Script 15)**  
**File**: `Script15_HUMAnN_GeneFamilies.R`

### Why  
To visualise gene family distributions across treatments.

### Input  
- `HUMAnN_GeneFamilies_Merged.tsv`  
- Metadata  

### What was done  
- Cleaned gene family names  
- Selected top 50 gene families  
- Generated heatmaps  

### Output  
- `HUMAnN_GeneFamilies_Top50.csv`  
- `HUMAnN_GeneFamilies_Heatmap.pdf`  
- `HUMAnN_GeneFamilies_CleanedTable.tsv`

---
