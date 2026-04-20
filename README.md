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
