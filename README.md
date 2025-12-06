# CityBEM-Docs

This repository hosts the public documentation for **CityBEM**, a
flexible and scalable Urban Building Energy Modeling (UBEM) framework
designed for large-area, high-resolution simulations.\
It includes user guides, data schemas, PV workflow documentation,
framework explanations, and examples for replicating the CityBEM
workflow in other UBEMs.

------------------------------------------------------------------------

## 📁 Welcome to CityBEM V2 Repository

This tree shows the layout of the source files (`.md`) used by MkDocs to generate the CityBEM V2 documentation website.

```text
CityBEM-Docs/
│
├── docs/ # Main source directory for the website content.
│   ├── index.md                    # Website homepage.
│   ├── installation.md             # Guide for installation and setup.
│   ├── run_citybem.md              # Guide for running CityBEM.
│   ├── data_inputs.md              # Reference for required input data files.
│   ├── outputs.md                  # Explanation of simulation result.
│   ├── solar_radiation.md          # Documentation for solar modeling.
│   ├── building_energy_modeling.md # Theory for the core energy model (UBEM).
│   ├── pv_model.md                 # Documentation for the Rooftop PV module.
│   ├── microclimate.md             # Explanation of microclimate coupling.
│   ├── examples.md                 # Test cases, and sample results.
│   ├── publications.md             # Related academic papers.
│   ├── about.md                    # Aout the project and developers.
│   ├── contact.md                  # Contact information and user support.
│   │
│   │
│   ├── assets/                     # Images, diagrams, figures
│   ├── javascripts/                # Optional custom JS for MkDocs
│   └── stylesheets/                # Custom CSS for MkDocs
│
├── .github/
│   └── workflows/
│         └── deploy.yml            # GitHub Pages deployment workflow
│
├── .gitignore
├── mkdocs.yml                      # MkDocs configuration file
└── README.md                       # Main repository description
```

------------------------------------------------------------------------

## 🧩 What This Repo Contains

-   **All documentation for the CityBEM UBEM framework**
-   **Input data schema description** for the Citywide Building Data
    Arrays (CBDAs)
-   **Workflows** for:
    -   Rooftop PV modeling
    -   Shading analysis
    -   PV array design
    -   UBEM simulation pipeline
-   **Usage examples** and reproducible workflows
-   **Figures and diagrams** for visualization
-   **MkDocs configuration** for building a documentation website

------------------------------------------------------------------------
## 🔄 Documentation Workflow

                +------------------+
                |  Write / Update  |
                |  .md Documents   |
                +---------+--------+
                          |
                          v
                +------------------+
                |   MkDocs Builds  |
                |   Static Pages   |
                +---------+--------+
                          |
                          v
                +------------------+
                | Git Push to Main |
                +---------+--------+
                          |
                          v
                +---------------------------+
                | GitHub Pages Deployment   |
                | (Automatic via Workflow)  |
                +---------------------------+

            User View: https://umbe-lab.github.io/CityBEM-Docs/

------------------------------------------------------------------------

## 🚀 How to Use This Repo

1.  Browse the **`docs/`** folder to find the documentation you need.

2.  To view the full documentation site locally:

    ``` bash
    pip install mkdocs mkdocs-material
    mkdocs serve
    ```

    Then open:\
    **http://127.0.0.1:8000**

3.  Edit any `.md` file to update documentation.

4.  Commit your changes:

    ``` bash
    git add .
    git commit -m "Update documentation"
    git push
    ```

5.  GitHub Pages will automatically rebuild the website.

------------------------------------------------------------------------

## 🤝 How to Contribute

1.  **Fork** the repository.

2.  **Create a new branch**:

    ``` bash
    git checkout -b feature/my-improvement
    ```

3.  Add or update Markdown files inside the `docs/` directory.

4.  Submit a **pull request** with a clear explanation.

------------------------------------------------------------------------

## 🏙️ About CityBEM

CityBEM is a computationally efficient UBEM engine capable of
large-scale, high-resolution transient building simulation, rooftop PV
analysis, and urban-scale scenario studies.\
Its modular architecture allows integration of shading tools, PV models,
and future components such as view-factor engines and grid-coupled
simulations.