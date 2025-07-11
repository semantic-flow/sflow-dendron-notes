---
id: xebek3dtv2zgs9ah0vbv57g
title: Semantic Flow General Guidance
desc: ''
updated: 1752061607151
created: 1751259888479
---

**Semantic Flow** is a framework for managing knowledge graphs and other Semantic Web resources in publish-ready [[semantic meshes|sflo.concept.mesh]]

## Workspace Components

The workspace contains multiple interconnected components:

- **flow-api/**: API component for creating, augmenting and maintaining [[sflo.concept.mesh-repo]] (currently empty structure)
- **flow-cli/**: Command-line application that consumes the flow-api
- **flow-service/**: Service component (currently empty structure)
- **flow-ontology/**: Ontology definitions (currently empty)
- **flow-web/**: Web frontend for the flow-service
- **sflo-dendron-notes/**: Complete documentation and specification in Dendron format
- **test-ns/**: Test mesh repo

## Key Concepts

### Semantic Mesh

A dereferenceable, versioned collection of semantic data and other resources, where every HTTP URI returns meaningful content.

#### Core Components

- **Mesh Resources**: 
  - **Nodes**: Semantic Atoms
    - **Dataset Nodes**: Bundles of data with optional quasi-immutable, versioned history
    - **Namespace Nodes**: basically empty folders for URL-based hierarchical organization
    - **Reference Nodes**: Refer to "things that exist" like people, or songs, or ideas
  - **Elements**: things that help define and systematize the nodes
    - **Components**: datasets for node metadata, reference data, and payload data
  - **Handles**: things that let you refer to a node as a node instead of its referent
  - **Asset Trees**: elements that allow you to attach arbitrary collections of files and folders to a mesh; in a sense, these things are "outside" the mesh, and other than the top-level "_meta" folder, they don't contain any other mesh resources

### Semantic Flow Workflow: 

- In General: Mesh resource addition & editing → Weaving → Publishing using GitHub Pages 

### Semantic Site

- The repo IS the site:
  - can be served locally
  - SSG (Static Site Generator) not required
    - but static resource page generation should happen on every weave as necessary
  - after push, you should be able to see the changed mesh at the corresponding github pages URL

## RDF and Semantic Web

- meshes support multiple RDF formats (.trig, .jsonld, etc.)
- be mindful of RDF terminology and concepts
  - Uses DCAT for dataset catalogs
  - Uses PROV for provenance
- When referring to IRIs or URIs that are part of a semantic mesh, prefer the term URLs instead of IRI or URI
  - if you see a reference to IRI or URI, it might need updating, or it might mean a distinction should be drawn
- RDF comments should be extremely brief and clear.

## Documentation

- All specifications and design docs are in `sflo-dendron-notes/`
- Check conversation logs in `sflo.conv.*` for context on design decisions if necessary, but beware of superceded and dangerously-outdated info 

### Documentation First

- unclear or anemic documentation should be called out
- documentation should be wiki-style: focused on the topic at hand, don't repeat yourself, keep things simple and clear
- when assisting with writing documentation, it should be kept concise and specific to the topic at hand
- whenever documentation is updated, any corresponding LLM conversation context should be updated too   
- to encourage documentation-driven software engineering, code comments should refer to corresponding documentation by filename, and the documentation and code should be cross-checked for consistency whenever possible 

### Documentation Architecture

Project documentation, specifications, journaling, and design choices are stored in `sflo-dendron-notes/` using Dendron's hierarchical note system. Key documentation includes:

- **Core concepts**: `sflo.concept.*` files talk about general Semantic Flow concepts
- **Mesh docs**: `sflo.concept.mesh.*` files define the semantic mesh architecture
- **Product specifications**: `sflo.product.*` files detail each component
- **Requirements**: `sflo.requirements.md`
- **Use cases**: `sflo.use-cases.*` files
- **Conversation logs**: `sflo.conv.*` files track design decisions and development history; BEWARE! These conversations contain information and decisions that have been superceded. Only reference conversations when necessary for historical context. Newer conversations are usually less misleading.

### Documentation Standards

- this project use wiki-style documentation. It's fine for articles to be extremely short. In general, articles should focus on their namesake topics.

### Component Development with Docs

- Each component (flow-api, flow-cli, flow-service, flow-web) should follow the architecture defined in the documentation
- Refer to `sflo.products.*` files for component-specifc descriptions, requirements, etc

## Project Architecture

TBD

## Coding Standards

### Language & Runtime

- **TypeScript**: Use strict TypeScript configuration with modern ES2022+ features
- **Deno**: Prefer Deno over Node.js; avoid Node.js-specific dependencies
- **Imports**: Use Deno-style imports with URLs (e.g., `import { assertEquals } from "https://deno.land/std/assert/assert_equals.ts"`)

### RDF Data Handling

- **Primary Format**: .trig files for RDF data storage and processing
- **Secondary Format**: Full JSON-LD support required
- **RDF Libraries**: Use RDF.js ecosystem libraries consistently across components
- **Namespace Management**: Follow URL-based identifier patterns as defined in `sflo.concept.identifier.md`
- **Reserved Names**: Validate against underscore-prefixed reserved identifiers per `sflo.concept.identifier.md`

### Semantic Mesh Architecture

- **Resource Types**: Maintain clear distinction between Nodes, Elements, and Resources as defined in `sflo.concept.mesh.md`
- **Folder Structure**: Validate mesh folder structures (dataset nodes, namespace nodes, etc.)
- **System Elements**: Distinguish between system-generated and user-modifiable elements
- **Weave Integration**: Code must support weave operations as defined in `sflo.concept.weave.md`

### Documentation-Driven Development

- **Code Comments**: Must reference corresponding documentation by filename (e.g., `// See sflo.concept.mesh.resource.node.md`)
- **Interface Definitions**: Link to concept documentation in TSDoc comments
- **Cross-Reference Validation**: Ensure consistency between code and documentation
- **API Documentation**: Generate from TSDoc comments

### Component Architecture

- **Separation**: Maintain clear boundaries between flow-api, flow-cli, flow-service, and flow-web
- **Error Handling**: Use consistent error patterns across all components
- **Async Patterns**: Use async/await for RDF operations and file I/O
- **Type Safety**: Leverage TypeScript's type system for mesh resource validation

### File Organization & Naming

- **TypeScript Modules**: Use `.ts` extension, organize by feature/component
- **Test Files**: Co-locate test files with source using `.test.ts` suffix
- **Mesh Resources**: Follow mesh resource naming conventions from @/flow-ontology/alpha/_node-data/_next/flow-ontology-alpha.trig 
- **Constants**: Use UPPER_SNAKE_CASE for constants, especially for reserved names

### Code Style

- **Formatting**: Use Deno's built-in formatter (`deno fmt`)
- **Linting**: Use Deno's built-in linter (`deno lint`)
- **Line Length**: 100 characters maximum
- **Indentation**: 2 spaces (TypeScript standard)
- **Semicolons**: Required (TypeScript best practice)

### Error Handling

- **Custom Errors**: Create semantic mesh-specific error types
- **Validation**: Validate mesh resource structures before processing
- **Logging**: Use structured logging for debugging weave operations
- **Async Error Propagation**: Properly handle async/await error chains

### Testing

- **Unit Tests**: Use Deno's built-in test runner
- **Integration Tests**: Test mesh operations end-to-end
- **RDF Validation**: Test both .trig and JSON-LD parsing/serialization
- **Mock Data**: Create test mesh structures following documentation patterns

### Performance

- **RDF Processing**: Stream large RDF files where possible
- **Caching**: Cache parsed RDF for repeated operations during weave
- **File I/O**: Use async file operations consistently
- **Memory Management**: Be mindful of large dataset loading
