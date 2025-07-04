---
id: faq002
title: Why are there elements at the top of a repo?
desc: ''
updated: 1751565743168
created: 1751351383000
---

## Question

Why are there [[mesh elements|sflo.concept.mesh.resource.element]] like `_meta/`, `_handle/`, and `_assets/` at the top level of a repository? Shouldn't elements only be inside nodes?

## Answer

Elements at the repository root exist because **the repository root itself is a [[mesh node|sflo.concept.mesh.resource.node]]** - specifically, it's the [[root node|sflo.concept.mesh.node-facet.root]] of the mesh.

### Repository Root = Mesh Root Node

Every semantic mesh has a root node, and in a repository-based mesh, the repository root **is** that root node. It's a "nameless" node locally (represented as "/") that can be any type of mesh node:

- **Namespace node**: If the repo organizes other nodes
- **data node**: If the repo represents a single dataset  
- **Reference node**: If the repo represents an external entity

### Elements Belong to Nodes

Since the repository root is a mesh node, it follows the same rules as any other node:

- **`_meta/`**: Contains the [[catalog dataset|sflo.concept.mesh.resource.element.meta-dataset]] with administrative metadata for the root node
- **`_handle/`**: Contains the [[node handle|sflo.concept.mesh.resource.element.node-handle]] for referential indirection
- **`_assets/`**: May contain an [[asset tree|sflo.concept.mesh.resource.element.asset-tree]] for file attachments
- **Other elements**: Depending on the root node type (e.g., `_ref/` for reference nodes, `_next/` for versioned datasets)

### Consistency Principle

This maintains architectural consistency: **every mesh node has the same structure and capabilities**, whether it's nested deep in the hierarchy or at the repository root. The root node isn't special - it's just the top-level node in the mesh hierarchy.

### Mesh Self-Containment

This design also supports the principle that **any subtree is a complete mesh**. The repository root, being a proper mesh node with all its elements, ensures the entire repository is a self-contained, functional semantic mesh.