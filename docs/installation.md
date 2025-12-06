# :material-cog-outline: CityBEM V2 Installation and Build Guide

CityBEM V2 is a high-performance city-scale simulation framework implemented in modern **C++17** and compiled using **Microsoft Visual Studio 2022 (MSVC v143)**. This comprehensive guide details the setup, compilation, and execution of the solver, including optional Python utilities.

---

## 1. Core Requirements & Environment Setup

To successfully build the C++ solver from source, the following software and precise configurations are mandatory:

### :material-laptop: Development Environment

| Requirement | Details |
| :--- | :--- |
| **Compiler** | **Microsoft Visual Studio 2022** |
| **Workload** | Desktop development with C++ |
| **Toolset** | MSVC v143 |
| **Language Standard** | ISO C++17 (`/std:c++17`) |
| **Version Control** | **Git** (for cloning and source management) |
| **Windows SDK Version** | 10.0 (latest installed) |

!!! info "Visual Studio Download"

    Install the latest version of **Microsoft Visual Studio 2022** and ensure the ***Desktop Development with C++*** workload is selected during installation.

    👉 [Download Visual Studio](https://visualstudio.microsoft.com/){:target="_blank"}

---

## :material-code-tags: 2. Source Code Acquisition

Obtain the CityBEM V2 source code by cloning the repository using Git. This downloads the codebase from the remote server to your local machine.

### :material-console-line: Cloning Steps

1.  **Navigate to Path:** Open your preferred terminal (Git Bash, PowerShell, etc.) and use `cd` to navigate to your desired project directory (e.g., `Documents` or `Projects`).
    * *Example:*
        ```bash
        cd C:\Users\YourName\Documents\GitHub
        ```

2.  **Clone Repository:** Execute the `git clone` command.
    ```bash
    git clone [https://example.git/CityBEM.git](https://example.git/CityBEM.git)
    ```

3.  **Enter Directory:** Navigate into the newly created project folder.
    ```bash
    cd CityBEM
    ```

!!! warning "Important Notice: Placeholder URL"

    The official open-source **CityBEM V2 repository is not yet published**. The URL provided is a placeholder. The public release will be announced once the repository is available.

---

## :material-hammer-wrench: 3. Building the Executable

### :material-cog-outline: Required Build Settings

The following C++ settings, configured within Visual Studio, ensure optimal compilation and security:

| Setting | Required Value | Purpose |
| :--- | :--- | :--- |
| **Optimization (Release)** | Maximize Speed (`/O2`) | Applies maximum optimization for high-performance execution. |
| **Debug Information Format** | Program Database (`/ZI`) | Enables detailed debugging for development builds. |
| **SDL Checks** | Enabled (`/sdl`) | Enforces Security Development Lifecycle checks. |
| **Warning Level** | Level 3 (`/W3`) | Sets the compiler to report standard warning levels. |

!!! note "Git LFS Configuration"

    Ensure **Git LFS (Large File Storage)** is properly configured. CityBEM V2 may rely on it for managing large binary files and data assets required during compilation.

### :material-console-line: Build Execution

1.  **Open Solution:** Launch **Visual Studio 2022** and open the solution file: `CityBEM.sln`.
2.  **Select Configuration:** Choose either **`Release | x64`** (recommended for simulation speed) or **`Debug | x64`** (for development and troubleshooting).
3.  **Compile:** Initiate the build process using the standard shortcut:
    ```
    Ctrl + Shift + B
    ```
The compiled solver executable will be found in the designated output folder (e.g., `/out/build/x64-Release/CityBEM.exe`).

---

## :material-run: 4. Running CityBEM V2

The executable requires all necessary input files to be located **in the same directory** as the solver for the simulation to run correctly.

### :material-console-line: Execution Steps

1.  **Preparation:** Copy all required simulation input files (configuration, scenario data, etc.) into the executable's build directory (e.g., `/out/build/x64-Release/`).
2.  **Navigate Terminal:** Use `cd` to change the working directory to the executable's location.
3.  **Execute Solver:** Run the following command:
    ```bash
    ./CityBEM.exe
    ```

---

## :material-language-python: 5. Optional: Python Tools for Automation & Analysis

Integrating Python provides powerful capabilities for advanced data analysis, workflow automation, and visualization of CityBEM V2 results.

### :material-function: Python Tool Capabilities

* **Input Pre-processing** and **Batch Simulation Automation**.
* **Time-Series and Spatial Post-processing** (analysis of outputs like energy fluxes).
* **Optional 3D Visualization** using specialized libraries.

### :material-wrench-outline: Installation and Setup

#### :material-language-python: Install Python 3.10+

1.  **Requirement:** Python version **3.10 or newer** is required.

2.  **Download:** Obtain the latest stable version:

    !!! tip "Download Python"

        [**Download Python 3.10+**](https://www.python.org/downloads/){:target="_blank"}

!!! warning "Important Note for Windows Users"

    During installation, ensure you check the box **"Add python.exe to PATH"**.

#### Install Required Libraries

* **Open Terminal** and run the following command to install the recommended packages:

    ```bash
    pip install numpy pandas matplotlib pyvista
    ```

!!! tip "Virtual Environment"

    We strongly recommend using a virtual environment (**Conda** or **venv**) to isolate these dependencies.

---

## :material-book-open-page-variant: 6. Optional: Build Documentation Locally

If you are **contributing to the CityBEM V2 documentation** or wish to view it locally before deployment, install the necessary tools and run the local server.

### :material-wrench-outline: Installation and Serving

1.  **Install Package:** Install MkDocs with the Material theme using `pip`:
    ```bash
    pip install mkdocs-material
    ```
2.  **Launch Server:** From the project root (where `mkdocs.yml` is), run:
    ```bash
    mkdocs serve
    ```

The local preview will be accessible in your web browser, typically at: `http://127.0.0.1:8000`. Changes to source files automatically refresh the page.

***

CityBEM V2 is designed to remain lightweight, extendable, and developer‑friendly. Compiling from source ensures maximum compatibility and performance for large-scale city simulations.