# CityBEM Documentation

This repository contains the full documentation for **CityBEM**, including user guides, model descriptions, input structures, examples, and asset files used for building and deploying the online documentation website.

Documentation is generated using **MkDocs** with the `mkdocs-material` theme and deployed automatically through GitHub Pages.

---

## 📁 Repository Structure

### `docs/`
The main documentation directory. Each Markdown file corresponds to a section of the published website.

Key files include:
- `index.md` — homepage  
- `installation.md` — installation and setup  
- `data_inputs.md` — full input structure documentation  
- `pv_model.md` — rooftop PV model  
- `building_energy_modeling.md` — CityBEM engine documentation  
- `microclimate.md` — microclimate modeling  
- `solar_radiation.md` — solar radiation computations  
- `outputs.md` — output definitions  
- `run_citybem.md` — how to run the model  

### `docs/assets/`
Contains all figures, diagrams, screenshots, and illustrations used across the documentation.

### `docs/examples/`
Contains example input files or demonstration cases.

### `docs/javascripts/` & `docs/stylesheets/`
Optional custom JavaScript and CSS for extending the MkDocs site.

### `.github/workflows/deploy.yml`
GitHub Actions workflow for automatically building and deploying the documentation.

### `.venv/`
Local virtual environment (used only for local builds).

### `mkdocs.yml`
Main MkDocs configuration file (theme, navigation, plugins, extra CSS/JS, site metadata).

---

## 🔄 Repository Workflow Diagram

The following diagram illustrates the full workflow from editing documentation to deployment:

```mermaid
flowchart TD

A[Edit Markdown Files<br/>docs/*.md] --> B[Local MkDocs Build<br/>mkdocs serve]
B --> C[Preview Website Locally<br/>localhost:8000]

A --> D[Push to GitHub Repo<br/>main branch]

D --> E[GitHub Actions Workflow<br/>.github/workflows/deploy.yml]
E --> F[Build MkDocs Site<br/>mkdocs build]
F --> G[Deploy to GitHub Pages]

G --> H[Live Documentation Website<br/>https://UMBE-LAB.github.io/CityBEM-Docs]