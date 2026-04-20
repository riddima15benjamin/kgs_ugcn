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

## Unified Knowledge Graph

All five domain-specific KGs are merged into a single unified graph (`unified_nodes.csv` + `unified_edges.csv`) that serves as input to the UltraGCN model. Nodes from each KG are deduplicated by ID, and edges are consolidated by relation type with max-weight retention. The result is a heterogeneous graph with five node types and six directed relation types:

| Relation | Direction | Source KG |
|---|---|---|
| `TREATS` / `INVESTIGATED_FOR` | Drug → Disease | G_DD |
| `ASSOCIATED_WITH` | Gene → Disease | G_GD |
| `INTERACTS_WITH` | Protein ↔ Protein | PPI |
| `TARGETS` | Drug → Protein | Drug–Target |
| `INVOLVED_IN` | Protein → Pathway | Protein–Pathway |

### Cross-KG Connectivity in Neo4j

The Neo4j query below traverses all five relation types in a single connected path, confirming that every KG contributes to the unified graph:

```cypher
MATCH path =
(g:Gene)-[:ASSOCIATED_WITH]->(d:Disease)
<-[:TREATS]-(dr:Drug)
-[:TARGETS]->(p:Protein)
-[:INVOLVED_IN]->(pw:Pathway)
RETURN path
LIMIT 100;
```

**Table view** — each row is a full Gene → Disease ← Drug → Protein → Pathway path:

![Unified KG table view](assets/neo4j_table.png)

**Graph view** — 13 nodes (7 Genes, 1 Disease, 1 Drug, 1 Protein, 3 Pathways) connected by 12 edges spanning all 5 KGs in a single subgraph:

![Unified KG graph view](assets/neo4j_graph.png)

---

## Best Practices

- Maintain consistency in entity identifiers across graphs before integration
- Validate edge relationships to avoid duplication or conflicts
- Document any transformations applied during preprocessing
- Version control updates to individual graphs independently

---


For questions, issues, or contributions, please open an issue in the repository or contact the maintainer.
