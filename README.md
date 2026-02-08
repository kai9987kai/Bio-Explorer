# Bio-Explorer

**Bio-Explorer** is a **standalone, biology-first protein explorer** that chains major public bioinformatics resources into one workflow:

**UniProt → STRING → Reactome → AlphaFold**  
…so you can go from *“this protein”* to **functional annotation**, **interaction network**, **pathways**, and **predicted 3D structure** in one place. :contentReference[oaicite:0]{index=0}

> Repo status: this project is currently shipped as a single `index.html` (HTML 100%), with a minimal commit history and no releases. :contentReference[oaicite:1]{index=1}

---

## What it does (concept)

Bio-Explorer is designed to take a **protein identifier** (most naturally a **UniProt accession**) and assemble a unified view:

1. **UniProt**: canonical protein record (name, organism, sequence, features/regions, cross-refs).
2. **STRING**: functional protein association network (interactions + confidence).
3. **Reactome**: curated pathways / events involving the protein.
4. **AlphaFold DB**: predicted structure + confidence metadata (and links to model files).

This is explicitly the project’s intended pipeline (as stated in the repository description). :contentReference[oaicite:2]{index=2}

---

## Data sources and APIs

Bio-Explorer relies on publicly available APIs:

- **UniProt programmatic access / REST APIs** (entry data, search, mappings). :contentReference[oaicite:3]{index=3}  
- **STRING API** (retrieve interaction/network data without the GUI). :contentReference[oaicite:4]{index=4}  
- **Reactome Content Service** (REST API access to Reactome knowledgebase). :contentReference[oaicite:5]{index=5}  
- **AlphaFold DB API** (retrieve metadata + URLs to structure models such as mmCIF/PDB). :contentReference[oaicite:6]{index=6}  

AlphaFold DB itself is an open-access resource designed to accelerate research, and Bio-Explorer’s “structure step” is intended to surface that content per protein. :contentReference[oaicite:7]{index=7}

---

## Quick start

### 1) Clone
```bash
git clone https://github.com/kai9987kai/Bio-Explorer.git
cd Bio-Explorer
