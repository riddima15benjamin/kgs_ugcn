## Multi-Knowledge Graph Repository

## Overview
This repository contains five independently constructed knowledge graphs (KGs), each representing domain-specific structured data. The graphs are provided as separate compressed (`.zip`) folders for modular access, reproducibility, and ease of integration into downstream pipelines such as graph databases (e.g., Neo4j) or machine learning workflows.

The primary objective of this repository is to organize and distribute these knowledge graphs in a consistent, portable format that supports further processing, integration, and analysis.

## Contents of Each Knowledge Graph

We construct five domain-specific knowledge graphs that collectively capture the multi-scale biological landscape relevant to drug repurposing:
1.	G_DD (Drug–Disease Graph): Encodes direct therapeutic associations and contraindications between drug compounds and disease phenotypes, sourced from DrugBank and OMIM.
2.	G_DG (Drug–Gene Graph): Captures pharmacological interactions, including drug targets, enzymes, transporters, and carriers, sourced from DrugBank and BioGRID.
3.	G_GD (Gene–Disease Graph): Models genotype-phenotype associations representing how genetic variants contribute to disease susceptibility, sourced from DisGeNET.
4.	G_GP (Gene–Pathway Graph): Encodes the participation of genes in biological signaling cascades and metabolic pathways, sourced from KEGG.
5.	G_DP (Disease–Pathway Graph): Connects disease phenotypes to their associated dysregulated pathways, derived from KEGG and HPO annotations. 

---

## Best Practices

- Maintain consistency in entity identifiers across graphs before integration
- Validate edge relationships to avoid duplication or conflicts
- Document any transformations applied during preprocessing
- Version control updates to individual graphs independently

---


For questions, issues, or contributions, please open an issue in the repository or contact the maintainer.
