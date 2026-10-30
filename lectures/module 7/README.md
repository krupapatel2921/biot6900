# BIOT 6900 · Module 7 · Parkinson's Disease Knowledge Graph

**Name:** Krupa
**Disease:** Parkinson's disease

## What this is
A Neo4j knowledge graph built from my Module 2 multi-omics targets and Module 3
druggability predictions, extended with HPO phenotypes, Reactome pathways, ChEMBL
drugs, GWAS lead variants, two related diseases (PSP, Alzheimer's) and a synthetic
patient cohort. Final graph: 76 nodes, 81 relationships.

## Files
| File | Description |
|---|---|
| `BIOT6900_Module7_Parkinsons.ipynb` | Notebook (load, Cypher queries, PageRank, communities, export) |
| `Module7_Parkinsons_KG_Report.pdf` | 3–4 page interpretation report |
| `my_knowledge_graph.png` | Graph figure (colour = node kind, size = PageRank) |
| `nodes.csv`, `edges.csv` | Exported node and edge tables |
| `biomarker_ranking.csv` | PageRank fused with gene evidence |
| `data/` | Inputs: Module 2/3 target tables and GWAS Catalog file |

## How to run
1. Create a free Neo4j AuraDB instance and download its credentials file.
2. Place the credentials file next to the notebook and update the filename in the
   connect cell. The credentials file is git-ignored and **not** included here.
3. Clear the database first (`MATCH (n) DETACH DELETE n`), then Kernel → Restart & Run All.

## What didn't work / notes
- My AuraDB instance initially still held the Alzheimer's worked-example graph, which
  added extra nodes to the Cypher results. I cleared the database before the final run;
  the load cell does not wipe the database itself, so clear it before re-running.
- The patient cohort is synthetic, not real individuals.
- Data layers were retrieved from Open Targets with AI assistance; see report Section 5.