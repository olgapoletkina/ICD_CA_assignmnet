# Semantic and Geometric Information Embedding from an IFC Model

Author: Olga Poletkina  
Project context: PhD position — AI-based generation of building designs

This folder contains an end-to-end BIM-to-AI pipeline that transforms IFC data into graph representations for GNN-based architectural co-pilot tasks.

## Main Artifact

- `olga_poletkina_assignment.ipynb` — full assignment notebook with extraction, graph construction, learning setup, and SHACL validation.

## Goal

Build a representation that combines:

- **Semantic information** (IFC classes, properties, typed relations)
- **Geometric information** (OBB features + shape descriptors)

and use it for graph learning tasks such as missing property prediction, anomaly detection, and rule-aware quality checks.

## Representation Design

### Semantic embedding

- IFC class (`IfcWall`, `IfcDoor`, `IfcSpace`, etc.) is encoded as node type / feature.
- Property Sets and QTO attributes are parsed and pivoted into node-level features.
- IFC relationships become typed edges (e.g., space boundaries, openings, fills).
- Optional text semantics from element names can be embedded (e.g., reduced transformer embeddings).

### Geometric embedding

- IFC geometry is tessellated to meshes with `ifcopenshell.geom`.
- Two complementary encodings are used:
  - **OBB features (n-dim):** position, size, orientation, ratios, area/volume-like descriptors.
  - **Shape embedding (m-dim):** descriptors from sampled point clouds, reduced with PCA.
- Final node vector combines semantic + geometric blocks.

## Intended AI Capability

The pipeline is designed for **relational** BIM intelligence:

- prediction of missing attributes (e.g., fire rating, external/internal status, load-bearing flags),
- anomaly/conflict detection in model structure and semantics,
- context-aware decisions using message passing over typed edges.

Suggested model families:

- relation-aware: `R-GCN`, `HGT`, `HAN`,
- homogeneous alternatives: `GCN`, `GAT` (with type features).

## Data Transformation Pipeline

1. Geometry extraction from IFC (OBB + point cloud)
2. Semantic extraction (Psets/QTOs)
3. Graph construction (typed directed edges)
4. Cleaning and normalization
   - self-loop removal
   - duplicate/orphan cleanup
   - keep largest connected component

## Symbolic Constraints (SHACL)

SHACL is used as an explainability and compliance layer:

- **Pre-training:** validate IFC-derived RDF graph to catch topology inconsistencies.
- **Post-prediction:** re-validate predictions and flag exact violated rules.

Why SHACL:

- declarative,
- W3C-standard,
- easier to maintain than hardcoded procedural checks.

## Practical Notes

- Pipeline is schema-agnostic and extendable to new IFC classes and regulation domains.
- Cleaned graph outputs remain aligned with IFC IDs for visualization and downstream BIM tooling.
- Folder-level git setup in this assignment keeps core artifacts visible (`.ipynb`, `data/`, `imgs/`, future `*.pdf` report).

## Source Document

This README is based on:  
`Semantic and Geometric Information Embedding from an IFC Model.docx`

