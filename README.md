# Fueling a Predator Death‑Trap — README

This README explains how to set up your environment and run the analysis contained in `Analysis.Rmd`.

---

## Project structure

```
.
├─ Analysis.Rmd                # Main R Markdown analysis
└─ data/
   ├─ Density_Data.csv
   ├─ Size_Data.csv
   └─ CRM_Data.xlsx            # Uses sheets: "ch" and "covars"
```

---

## Prerequisites

- **R ≥ 4.2 (earlier may work, but these versions are tested most often with packages below)**
- **R package toolchain** (needed by `glmmTMB` and friends)
  - **Windows:** Install **Rtools** matching your R version
  - **macOS:** Install **Xcode Command Line Tools** (`xcode-select --install`)
  - **Linux:** Install build tools (e.g., `build-essential`, `libcurl4-openssl-dev`, `libxml2-dev`, `libssl-dev`)
- **Java JDK/JRE** (required by the **`xlsx`** R package to read `CRM_Data.xlsx`)
- *(Optional)* **LaTeX** for PDF output. The simplest path is **TinyTeX** (installed from R via `tinytex::install_tinytex()`). HTML output does **not** require LaTeX.

> If you cannot or do not want to install Java, see the **Troubleshooting** section for alternatives to `xlsx`.

---

## R packages used

This project uses `Require` to install/load packages programmatically. Core packages:

- `Require`
- `data.table`, `dplyr`, `ggplot2`
- `glmmTMB`, `performance`, `DHARMa`, `emmeans`, `multcompView`
- `xlsx` *(for reading the Excel file)*
- `RMark` *(listed but not required by the steps below; only needed if you plan to extend analyses using MARK on Windows)*

### One‑line install

In a fresh R session, run:

```r
install.packages("Require")
Require::Require(c(
  "data.table", "dplyr", "ggplot2", "emmeans",
  "glmmTMB", "performance", "DHARMa", "multcompView",
  "xlsx", "RMark"
))
```

> Notes
>
> - `glmmTMB` requires a working C/C++ toolchain; see **Prerequisites** above.
> - `xlsx` requires Java (JRE or JDK). If R cannot find Java, install it and then run `R CMD javareconf` (macOS/Linux) or restart R after installing Java (Windows).

---

## Data

Make sure the following files are in the `data/` folder (relative to the project root):

- `Density_Data.csv`
- `Size_Data.csv`
- `CRM_Data.xlsx` (must include two sheets named **`ch`** and **`covars`**)

Paths in the script are **relative** (e.g., `data/Density_Data.csv`), so run the analysis from the project root where `Analysis.Rmd` resides. Alternatively, in RStudio open the project file `mabuyaAnalysis.Rproj`.

---

## How to run

### Option A — RStudio

1. Open `Analysis.Rmd` in **RStudio**.
2. Click **Knit** → choose **HTML** (recommended) or **PDF** (requires LaTeX/TinyTeX).

### Option B — From R console/terminal

```r
# From the project root
install.packages("rmarkdown")
rmarkdown::render("Analysis.Rmd", output_format = "html_document")
# For PDF (requires LaTeX):
# rmarkdown::render("Analysis.Rmd", output_format = "pdf_document")
```
The rendered file will be written alongside `Analysis.Rmd` (e.g., `Analysis.html` / `Analysis.pdf`).

---

## Troubleshooting

**Java / `xlsx` errors**
- Symptom: `Error in .jinit()`, `Java not found`, or `read.xlsx` fails.
- Fix: Install a Java JDK/JRE and restart R. On macOS/Linux, run `R CMD javareconf` afterward. On Windows, ensure JAVA is on your PATH.
- Alternative: Use `readxl::read_excel()` instead of `xlsx` (no Java needed), or export the two Excel sheets to CSV and read with `read.csv`.

**`glmmTMB` / compilation errors**
- Ensure your compiler toolchain is installed (see **Prerequisites**). Try updating `TMB` and `glmmTMB`: `install.packages(c("TMB","glmmTMB"))`.

**PDF rendering fails**
- Install TinyTeX in R: `install.packages("tinytex"); tinytex::install_tinytex()` and re‑Knit.

**`RMark` on non‑Windows**
- `RMark` interfaces with MARK, which is Windows‑only. If you are not running the MARK‑based part of the analyses, you can omit `RMark` entirely.

---

## Reproducibility tips
- Run from the project root so relative paths resolve correctly. Alternatively, open the file `mabuyaAnalysis.Rproj` to set up the correct environment.
- Keep the folder structure unchanged (`Analysis.Rmd` plus `data/`).

---

## Citation
If you use or adapt this analysis, please cite the manuscript and the R packages used (see `citation("pkg")`).

