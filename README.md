# CityBEM-Docs

This repository hosts the public documentation for **CityBEM**, a
flexible and scalable Urban Building Energy Modeling (UBEM) framework
designed for large-area, high-resolution simulations.\
It includes user guides, data schemas, PV workflow documentation,
framework explanations, and examples for replicating the CityBEM
workflow in other UBEMs.

------------------------------------------------------------------------

## 📁 Repository Structure

    CityBEM-Docs/
    │
    ├── docs/                     # Main documentation source files
    │   ├── index.md             # Main landing page
    │   ├── overview.md          # High-level introduction to the framework
    │   ├── installation.md      # How to install & run CityBEM
    │   ├── data_inputs.md       # Detailed explanation of input arrays
    │   ├── pv_workflow.md       # Rooftop PV modeling workflow
    │   ├── shading_tool.md      # Inter-building shading tool documentation
    │   ├── view_factor.md       # (Future) View-factor tool documentation
    │   └── examples/            # Usage examples & tutorials
    │
    ├── assets/                  # Figures, diagrams, icons used in docs
    │
    ├── .github/
    │   └── workflows/           # GitHub Pages / CI workflows
    │
    ├── mkdocs.yml               # MkDocs configuration for building the website
    └── README.md                # This file

------------------------------------------------------------------------

## 🧩 What This Repo Contains

-   **All documentation for the CityBEM UBEM framework**
-   **Input data schema description** for the Citywide Building Data
    Arrays (CBDAs)
-   **Workflows** for:
    -   Rooftop PV modeling\
    -   Shading analysis\
    -   PV array design\
    -   UBEM simulation pipeline\
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

            User View: https://<username>.github.io/CityBEM-Docs/

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

1.  **Fork** the repository.\

2.  **Create a new branch**:

    ``` bash
    git checkout -b feature/my-improvement
    ```

3.  Add or update Markdown files inside the `docs/` directory.\

4.  Submit a **pull request** with a clear explanation.

------------------------------------------------------------------------

## 🏙️ About CityBEM

CityBEM is a computationally efficient UBEM engine capable of
large-scale, high-resolution transient building simulation, rooftop PV
analysis, and urban-scale scenario studies.\
Its modular architecture allows integration of shading tools, PV models,
and future components such as view-factor engines and grid-coupled
simulations.