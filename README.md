# BioExplorer v3

**BioExplorer v3** is a single-file, browser-based protein research workstation that brings together sequence, interaction, pathway, structural, domain, variation, and provenance data in one interactive interface.

It integrates data from:

- **UniProt**
- **STRING**
- **Reactome**
- **AlphaFold Database**
- **InterPro**
- **PDBe / SIFTS**
- optional human variation sources

BioExplorer is designed for exploratory research, hypothesis generation, cross-database comparison, and reproducible biological analysis.

It runs entirely in the browser and requires no backend server.

---

## Overview

BioExplorer started as a compact protein explorer linking:

```text
UniProt
   ↓
STRING
   ↓
Reactome
   ↓
AlphaFold
```

Version 3 expands that concept into a broader evidence-integration environment:

```text
                           ┌──────────────┐
                           │   UniProt    │
                           └──────┬───────┘
                                  │
               ┌──────────────────┼─────────────────┐
               │                  │                 │
               ▼                  ▼                 ▼
          ┌─────────┐       ┌──────────┐      ┌──────────┐
          │ STRING  │       │ Reactome │      │ InterPro │
          └────┬────┘       └────┬─────┘      └────┬─────┘
               │                 │                  │
               └──────────┬──────┴───────────┬─────┘
                          │                  │
                          ▼                  ▼
                    ┌───────────┐      ┌───────────┐
                    │ AlphaFold │      │ PDBe/SIFTS│
                    └─────┬─────┘      └─────┬─────┘
                          │                  │
                          └────────┬─────────┘
                                   ▼
                        ┌────────────────────┐
                        │ Evidence / Research│
                        │       Layer        │
                        └────────────────────┘
```

The goal is not merely to display information from several APIs.

BioExplorer attempts to show how different biological evidence layers relate to one another.

---

# Features

## Protein Search

Search using:

- gene symbols,
- protein names,
- UniProt accessions,
- text queries.

Examples:

```text
TP53
BRCA1
PTEN
P04637
EGFR
AKT1
```

Autocomplete is backed by UniProt.

Reviewed entries are preferred when appropriate.

---

# UniProt Integration

BioExplorer retrieves the core protein record from UniProt.

Displayed information can include:

- UniProt accession
- protein name
- gene names
- gene synonyms
- species
- sequence length
- amino-acid sequence
- functional description
- annotated protein features

The UniProt entry acts as the central identity anchor for the rest of the application.

---

# Sequence Viewer

The complete protein sequence can be viewed directly inside BioExplorer.

Features can be selected to jump to the corresponding sequence region.

This makes it easier to inspect:

- domains,
- motifs,
- modified residues,
- active sites,
- binding sites,
- transmembrane regions,
- repeats,
- regions,
- signal peptides,
- other UniProt annotations.

---

# Protein Feature Track

Protein annotations are displayed on an interactive linear feature track.

Feature filters include:

```text
All
Domains / Regions
Sites / PTMs
```

Selecting a feature exposes additional information and its sequence coordinates.

BioExplorer v3 also connects these features with structural-confidence evidence.

---

# STRING Protein Network

BioExplorer builds an interactive protein-association network using STRING.

Controls include:

- STRING confidence threshold
- number of neighbours
- functional network
- physical network

The network can be:

- zoomed,
- moved,
- searched,
- re-laid out,
- exported,
- filtered,
- cross-highlighted with pathway data.

---

# Identifier Normalization

Version 3 improves STRING handling by explicitly normalizing identifiers before downstream network analysis where possible.

This reduces errors caused by mixing:

```text
gene symbols
UniProt accessions
STRING identifiers
database-specific identifiers
```

and improves reproducibility across network operations.

---

# Network Evidence

STRING edges may contain evidence from several channels.

Examples include:

```text
Neighbourhood
Gene fusion
Phylogenetic co-occurrence
Co-expression
Experiments
Databases
Text mining
```

BioExplorer preserves these signals where available rather than treating every edge as equivalent.

---

# Advanced Network Analysis

BioExplorer v3 goes beyond simple node degree.

Depending on the loaded network, analysis may include:

- degree
- network density
- hub ranking
- betweenness centrality
- closeness-style measures
- local connectivity
- community structure
- connected components
- edge confidence
- physical vs functional context

This provides a more nuanced view than assuming:

> the highest-degree protein must automatically be the most important target.

Network statistics should still be interpreted cautiously because biological interaction networks depend strongly on:

- database coverage,
- confidence thresholds,
- evidence type,
- experimental bias,
- network sampling.

---

# Network Communities

V3 can identify groups of densely interconnected proteins.

Communities may help reveal:

- pathway modules,
- complexes,
- functional clusters,
- signalling sub-networks,
- potential cross-talk regions.

Community membership is exploratory evidence rather than proof of a biological complex or mechanism.

---

# STRING Functional Enrichment

The current STRING neighbourhood can be analysed for enriched biological terms.

Possible categories include:

- Gene Ontology
- pathways
- biological processes
- molecular functions
- cellular components
- protein domains
- other STRING enrichment categories

Where available, BioExplorer displays statistical measures such as FDR.

---

# Reactome Integration

BioExplorer maps the loaded protein into Reactome.

It can explore:

- pathways
- reactions
- pathway participants
- event membership

Selecting a pathway can highlight corresponding proteins in the STRING network.

This enables direct comparison between curated pathway membership and interaction-network context.

---

# Reactome Set Analysis

BioExplorer v3 adds support for multi-protein analysis.

A list of proteins or genes can be supplied and analysed together for Reactome pathway enrichment / over-representation.

This is useful for:

- gene lists,
- experimental hits,
- proteomics candidates,
- differential-expression outputs,
- manually selected network modules.

---

# Pathway ↔ Network Gap Analysis

BioExplorer compares selected Reactome participants with the currently visible STRING network.

Proteins present in a pathway but absent from the network may represent:

- confidence-threshold effects,
- identifier-mapping differences,
- pathway breadth,
- incomplete interaction coverage,
- network-boundary choices.

These are labelled as **data-gap candidates**, not automatically as missing biological interactions.

---

# AlphaFold Structure Viewer

Where available, AlphaFold structures can be viewed interactively in 3D.

The viewer supports:

- rotation,
- zoom,
- cartoon representation,
- confidence colouring,
- structural inspection.

AlphaFold confidence values are extracted where possible.

---

# pLDDT Analysis

BioExplorer analyses AlphaFold per-residue confidence.

Typical interpretation:

```text
pLDDT > 90
very high local confidence

70–90
generally confident

50–70
low confidence

<50
very low confidence
```

BioExplorer may calculate:

- mean pLDDT,
- minimum / maximum confidence,
- fraction below 70,
- fraction below 50,
- low-confidence segments.

---

## Important pLDDT Guardrail

Low pLDDT does **not automatically mean disorder**.

It may reflect:

- intrinsic disorder,
- flexible regions,
- uncertain structure,
- missing biological context,
- conformational heterogeneity,
- limitations of the prediction.

BioExplorer therefore treats low-confidence regions as **candidates for further investigation**, not definitive disorder annotations.

---

# Predicted Aligned Error — PAE

BioExplorer v3 substantially improves PAE handling.

Earlier versions primarily exposed AlphaFold PAE visually.

V3 can analyse the underlying matrix when the source data are available.

PAE represents uncertainty in the relative positioning of residues when one part of the structure is aligned to another.

It is particularly useful for understanding:

- domain-to-domain confidence,
- uncertain relative domain orientations,
- flexible domain arrangements,
- possible modular structure.

---

# PAE Domain-Packing Analysis

BioExplorer separates:

```text
local structural confidence
```

from:

```text
relative domain-placement confidence
```

because these answer different questions.

A domain may contain residues with excellent local pLDDT while its orientation relative to another domain remains uncertain.

V3 can use the PAE matrix to identify these situations.

---

# InterPro Integration

BioExplorer v3 can retrieve InterPro annotations for the selected protein.

These may include:

- domains,
- protein families,
- conserved sites,
- repeats,
- signatures,
- functional classifications.

InterPro annotations can be compared with:

- UniProt features,
- AlphaFold confidence,
- PAE patterns,
- sequence regions.

---

# PDBe / SIFTS Integration

V3 introduces experimental structure context using PDBe and SIFTS mappings.

Where data are available, BioExplorer can identify experimentally determined structures associated with the protein.

This allows the user to compare:

```text
Predicted structure
        ↕
Experimental structure evidence
```

rather than relying exclusively on AlphaFold.

Potential information includes:

- PDB entries
- mapped UniProt residues
- experimental coverage
- structure metadata
- available chains

---

# Predicted vs Experimental Structure Context

When both sources are available, BioExplorer can highlight situations such as:

- experimental structures covering only one domain,
- AlphaFold predicting regions without experimental coverage,
- experimental fragments supporting high-confidence predicted regions,
- regions where structure evidence remains limited.

This is intended as research context rather than automatic structural validation.

---

# Human Variation Context

For human proteins, BioExplorer v3 can optionally display available variant information.

The variation layer is designed to connect:

```text
variant position
      ↓
UniProt feature
      ↓
InterPro domain
      ↓
AlphaFold confidence
      ↓
structural region
```

This allows residue-level evidence to be inspected in context.

Variation information must not be interpreted as clinical diagnosis.

---

# Residue-Level Evidence Integration

One of the major goals of v3 is to connect evidence that traditionally appears in separate databases.

For a residue or region, the application may bring together:

- amino-acid position
- UniProt features
- InterPro annotation
- AlphaFold pLDDT
- PAE context
- experimental structure coverage
- variation evidence

This provides a much richer biological view than inspecting each database independently.

---

# Evidence Tab

V3 introduces an **Evidence** workspace.

It is designed to answer:

> Where did this conclusion come from?

The evidence layer records information such as:

- source database
- endpoint
- query identifier
- parameters
- retrieval time
- source status
- derived calculation

This allows a result to be traced back to the evidence used to generate it.

---

# Provenance

BioExplorer v3 treats provenance as part of the analysis rather than an afterthought.

Where practical, the program records:

```text
Database
Endpoint
Identifier
Species
Request parameters
Timestamp
BioExplorer version
Derived calculation
```

This makes exported analyses easier to reproduce later.

---

# Research Lens

The Research Lens combines information from multiple evidence layers.

It can surface observations involving:

- network connectivity,
- enrichment,
- pathways,
- structure confidence,
- PAE,
- protein features,
- domain annotations,
- experimental structures,
- pathway/network differences.

---

## Hypothesis Builder

BioExplorer generates transparent research hypotheses from available evidence.

Examples:

- a domain overlapping an uncertain structural region,
- a high-connectivity network protein worth validating,
- an enriched pathway associated with the network,
- a pathway participant absent from the current STRING neighbourhood,
- a residue region supported by several independent annotation sources.

These are intended to suggest **questions to investigate**, not establish biological mechanisms.

---

# Evidence-Aware Interpretation

V3 deliberately avoids collapsing every signal into one unquestioned biological score.

Where possible it distinguishes between:

```text
Observed database evidence
Derived statistics
Heuristic interpretation
Hypothesis
```

This separation makes it clearer which statements come directly from a data source and which are generated by BioExplorer.

---

# Research Guardrails

BioExplorer follows several interpretation rules.

### Association does not imply mechanism

STRING interactions do not prove direct biological interaction.

### Enrichment does not prove causation

An enriched term describes statistical over-representation, not necessarily direct mechanism.

### Network hubs are not automatically ideal targets

Connectivity depends on data coverage and network construction.

### Low pLDDT does not prove disorder

It indicates low prediction confidence.

### PAE and pLDDT measure different things

Local residue confidence should not be confused with relative domain-placement confidence.

### Predicted structures do not replace experiments

AlphaFold predictions should be interpreted alongside available experimental evidence.

### Variant presence does not establish pathogenicity

Variation requires appropriate genetic and clinical interpretation.

---

# Robust API Client

BioExplorer v3 improves the networking layer substantially.

Requests can include:

- request timeout
- retries
- exponential-style backoff
- cancellation
- stale-request protection
- graceful source failure

This reduces failures caused by temporary network or API problems.

---

# Stale Request Protection

If the user quickly switches from one protein to another, older requests are prevented from silently overwriting the new protein state.

This is important when querying several biological services simultaneously.

---

# Graceful Degradation

Not every data source will always be available.

If one source fails, BioExplorer attempts to keep the rest of the analysis usable.

For example:

```text
UniProt        ✓
STRING         ✓
Reactome       ✓
AlphaFold      ✓
InterPro       unavailable
PDBe           ✓
```

should still allow most of the application to function.

---

# Caching

BioExplorer uses local caching to reduce unnecessary API requests.

V3 improves this system with application-specific cache namespaces.

The cache can therefore be cleared without indiscriminately deleting unrelated browser `localStorage`.

Different biological resources may also use different cache durations.

---

# Reproducibility

Research results can change as biological databases evolve.

BioExplorer therefore records available context including:

- retrieval date
- selected parameters
- species
- network score threshold
- neighbour count
- network type
- source information
- BioExplorer version

This makes later comparison more meaningful.

---

# Exports

BioExplorer supports several client-side exports.

## Network JSON

Exports the current interaction network.

---

## Network PNG

Exports the network visualisation.

---

## Report

Creates a Markdown summary of the current analysis.

---

## Research Pack

Creates a research-oriented Markdown document containing major evidence and hypotheses.

---

## Research Bundle

V3 adds a richer machine-readable export containing relevant:

- query information,
- parameters,
- UniProt summary,
- network metrics,
- pathway results,
- enrichment,
- structural confidence,
- PAE-derived information,
- InterPro context,
- PDBe mappings,
- variation context,
- evidence provenance,
- timestamps.

This is the recommended export for reproducible downstream analysis.

---

# Notes

BioExplorer includes a persistent workspace for recording:

- hypotheses,
- experimental ideas,
- questions,
- observations,
- follow-up work.

Notes can be copied or exported as Markdown.

---

# Shareable Queries

The current query and major parameters can be encoded into a shareable URL where supported.

This makes it easier to reproduce a view with another researcher or on another machine.

---

# Species

Built-in options include:

```text
Human
Mouse
Rat
Zebrafish
Drosophila
C. elegans
S. cerevisiae
Arabidopsis
```

Additional species support depends on the underlying APIs.

Some v3 evidence layers, especially variation data, may only be available for human proteins.

---

# User Interface

BioExplorer is organised into research workspaces:

```text
Network
Pathways
Structure
Features
Research Lens
Evidence
Notes / Report
```

The original BioExplorer workflow is retained while the evidence and analysis capabilities are expanded.

---

# Accessibility

V3 includes improvements intended to make the interface easier to use with:

- keyboard navigation,
- semantic controls,
- clearer focus states,
- responsive layouts,
- readable contrast,
- reduced ambiguity in status messages,
- assistive-technology-friendly labels.

---

# Local-First Design

BioExplorer has no application backend.

Database requests are made directly from the browser.

Benefits include:

- simple deployment,
- no server maintenance,
- transparent data flow,
- portable single-file distribution.

---

# Privacy

BioExplorer itself does not require:

- an account,
- tracking,
- analytics,
- a BioExplorer server,
- cloud storage.

However, biological queries are sent to the external scientific APIs required to retrieve the requested data.

---

# Installation

BioExplorer v3 is distributed as one HTML file.

```text
BioExplorer_v3.html
```

No build step is required.

---

## Option 1 — Open Directly

Open the HTML file in a modern browser.

Some APIs may reject requests originating from:

```text
file://
```

because of browser CORS restrictions.

---

## Option 2 — Local HTTP Server

Recommended.

With Python installed:

```bash
python -m http.server 8000
```

Then open:

```text
http://localhost:8000/BioExplorer_v3.html
```

This usually provides more predictable behaviour than `file://`.

---

# Browser Requirements

A current Chromium, Firefox, or Safari-based browser is recommended.

The application relies on browser features including:

- Fetch API
- AbortController
- localStorage
- Canvas / SVG
- modern JavaScript
- downloadable Blob URLs

---

# External Libraries

BioExplorer uses:

- **Cytoscape.js** for biological network visualisation
- **3Dmol.js** for interactive molecular structure rendering

These are loaded from external CDNs in the standalone build.

Internet access is therefore required for the libraries and biological APIs unless they are hosted locally.

---

# Data Sources

## UniProt

Protein sequence, annotation, function, identifiers, and features.

https://www.uniprot.org/

---

## STRING

Protein association networks and enrichment.

https://string-db.org/

---

## Reactome

Curated biological pathways and reactions.

https://reactome.org/

---

## AlphaFold Database

Predicted protein structures and confidence information.

https://alphafold.ebi.ac.uk/

---

## InterPro

Protein families, domains, repeats, and functional signatures.

https://www.ebi.ac.uk/interpro/

---

## PDBe

Experimentally determined macromolecular structure information.

https://www.ebi.ac.uk/pdbe/

---

## SIFTS

Mappings between PDB structures, UniProt sequences, and other biological resources.

https://www.ebi.ac.uk/pdbe/docs/sifts/

---

# Architecture

BioExplorer v3 remains a standalone application but internally separates responsibilities conceptually.

```text
┌───────────────────────────────┐
│          User Interface       │
└───────────────┬───────────────┘
                │
                ▼
┌───────────────────────────────┐
│       Application State       │
└───────────────┬───────────────┘
                │
        ┌───────┴─────────┐
        │                 │
        ▼                 ▼
┌───────────────┐  ┌──────────────┐
│ API / Cache   │  │ Analysis     │
│ Layer         │  │ Layer        │
└───────┬───────┘  └──────┬───────┘
        │                  │
        ▼                  ▼
┌───────────────┐   ┌──────────────┐
│ Data Sources  │   │ Evidence /   │
│               │   │ Provenance   │
└───────────────┘   └──────────────┘
```

---

# Analysis Philosophy

BioExplorer tries to avoid a common problem in biological software:

> presenting derived computational signals as if they were direct experimental observations.

For this reason, v3 attempts to distinguish:

### Source evidence

Information directly obtained from a database.

### Derived metric

Something calculated by BioExplorer.

Example:

```text
Network density
Mean pLDDT
Betweenness centrality
```

### Heuristic

A software-defined interpretation.

### Hypothesis

A proposed research question generated from combined evidence.

---

# Example Workflow

Search:

```text
TP53
```

BioExplorer may then:

```text
1. Resolve TP53 through UniProt
2. Load canonical protein information
3. Fetch sequence and annotated features
4. Normalize identifiers
5. Build a STRING network
6. Calculate network statistics
7. Retrieve network enrichment
8. Map TP53 into Reactome
9. Load pathway participants
10. Retrieve AlphaFold structure
11. Analyse pLDDT
12. Analyse PAE
13. Retrieve InterPro domains
14. Find experimental PDBe/SIFTS mappings
15. Load supported human variation context
16. Cross-reference residue-level evidence
17. Generate Research Lens hypotheses
18. Record provenance
19. Export a reproducible Research Bundle
```

---

# Example Research Questions

BioExplorer can help investigate questions such as:

> Which proteins are most central in this interaction neighbourhood?

> Are the strongest network modules associated with the same pathways?

> Which Reactome pathway proteins are absent from my STRING network?

> Does a UniProt regulatory feature overlap a low-confidence predicted region?

> Is a high-pLDDT domain positioned confidently relative to another domain?

> Is an AlphaFold-predicted region covered by an experimental PDB structure?

> Do InterPro domains agree with visible structural boundaries?

> Which variants fall inside annotated or structurally interesting regions?

> Which observations are supported by multiple independent databases?

---

# What BioExplorer Is Not

BioExplorer is **not**:

- a clinical diagnostic system,
- a medical device,
- a replacement for experimental validation,
- a molecular-dynamics package,
- a protein-design system,
- proof that a predicted interaction occurs in vivo,
- proof that a variant is pathogenic,
- proof that a low-confidence AlphaFold region is disordered.

It is an exploratory research and evidence-integration tool.

---

# Validation

The v3 standalone build has been checked using:

- JavaScript syntax validation
- static feature assertions
- DOM-reference validation
- duplicate-ID checks
- preservation checks against the original UI
- unit-style graph-analysis tests
- PAE/parser tests
- browser smoke testing

The compatibility check retained all **105 original DOM controls / IDs** while extending the application with the v3 interface.

The browser smoke test completed without page errors or console warnings in the test environment.

Because browser networking was restricted during automated testing, external biological APIs were not fully end-to-end tested from that environment.

---

# Current Limitations

## External API availability

BioExplorer depends on third-party scientific services.

An upstream outage or API change may temporarily affect a feature.

---

## CORS

Some external services may behave differently when the application is opened using `file://`.

Using a local HTTP server is recommended.

---

## Database disagreement

Different biological resources may contain:

- different identifiers,
- different isoforms,
- different evidence,
- different pathway boundaries,
- different update schedules.

BioExplorer does not assume that one source automatically overrides another.

---

## Structural interpretation

AlphaFold confidence values are useful but must be interpreted carefully.

BioExplorer cannot determine biological flexibility, disorder, binding, or conformational dynamics solely from pLDDT or PAE.

---

## Network bias

Centrality and hub measurements depend on the network that was constructed.

A highly connected node may partly reflect:

- research popularity,
- database coverage,
- STRING evidence thresholds,
- neighbour limits.

---

# Future Development

Potential future directions include:

- multi-protein workspaces
- saved research projects
- direct PDB superposition
- predicted-vs-experimental RMSD analysis
- AlphaFold complex exploration
- residue contact maps
- domain-level PAE clustering
- UniProt isoform comparison
- protein-complex networks
- Gene Ontology evidence-code filtering
- disease / phenotype resources
- tissue-expression context
- protein abundance data
- transcript-expression integration
- publication evidence
- downloadable CSV tables
- graph exchange formats
- richer statistical enrichment tools
- cross-species orthologue analysis
- protein sequence alignment
- local database snapshots
- offline scientific-data packs
- WebGPU structure analysis
- optional on-device AI research summarisation
- automated API schema compatibility testing

---

# Version History

## v3

Major research-workstation upgrade.

Added or substantially improved:

- evidence/provenance system
- robust API layer
- request timeouts
- retries/backoff
- stale-request cancellation
- safer application-specific cache
- STRING identifier normalization
- richer network centrality
- network community analysis
- InterPro integration
- PDBe/SIFTS integration
- experimental-structure context
- human variation context
- raw PAE analysis
- domain-packing confidence analysis
- residue-level evidence integration
- Reactome set analysis
- improved Research Lens
- reproducible Research Bundle
- accessibility improvements
- stronger scientific interpretation guardrails

All major original BioExplorer workflows are retained.

---

## v2

Introduced the original integrated research interface:

- UniProt search
- STRING network
- STRING enrichment
- Reactome pathways
- pathway participant highlighting
- AlphaFold structure viewer
- pLDDT analysis
- UniProt feature track
- FASTA sequence viewer
- Research Lens
- hypothesis generation
- reports
- notes
- network exports

---

# Design Principles

## Preserve evidence provenance

Every conclusion should be traceable where possible.

## Separate data from interpretation

Database evidence should not be confused with software heuristics.

## Combine complementary sources

No single biological database contains the complete picture.

## Show uncertainty

Missing or uncertain evidence should remain visible.

## Preserve the original evidence

Derived metrics should not overwrite source data.

## Prefer reproducibility

Queries and parameters should be exportable.

## Remain transparent

Research heuristics should be understandable rather than opaque.

---

# Contributing

Contributions are welcome.

Useful contribution areas include:

- biological API integrations
- identifier mapping
- graph algorithms
- structural bioinformatics
- statistical analysis
- accessibility
- UI improvements
- browser compatibility
- automated tests
- scientific documentation

When adding new scientific interpretations, clearly distinguish:

```text
source evidence
derived calculation
heuristic
hypothesis
```

---

# Development

BioExplorer currently uses a deliberately simple distribution model:

```text
one HTML file
```

This makes the application highly portable.

For larger future releases, the internal code may benefit from being split into modules during development and compiled back into a standalone release artifact.

---

# License

Choose the licence appropriate for your project.

Common open-source options include:

```text
MIT
Apache-2.0
GPL-3.0
```

---

# Disclaimer

BioExplorer is intended for **research, education, software experimentation, and hypothesis generation**.

It does not provide medical advice or clinical interpretation.

Scientific conclusions should be verified against primary literature, source databases, experimental evidence, and appropriate domain expertise.

---

# BioExplorer v3

**From protein sequence to systems context in one browser workspace.**

```text
Sequence
+
Features
+
Domains
+
Interactions
+
Pathways
+
Structure
+
PAE
+
Experimental evidence
+
Variation
+
Provenance
=
Integrated protein exploration
```