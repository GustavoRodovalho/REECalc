# REECalc

**REECalc** is a Python/Jupyter notebook for calculating and visualizing
the compositions of rare-earth-element (REE) minerals from oxide
analyses. The current workflow focuses on phosphate compositions and a
**Monazite--Cheralite--Huttonite** ternary diagram.

The notebook is designed for **Google Colab** and uses **pandas**,
**NumPy**, and **Matplotlib**.

> **Project status:** Research-oriented notebook. The structural
> calculations and ternary diagram follow the conventions implemented in
> the current notebook; they should be validated before use for
> definitive mineral identification.

## Features

-   Read oxide and elemental analyses from an Excel workbook.
-   Convert oxide weight percentages to molar proportions.
-   Calculate elemental and oxygen molar proportions using oxide
    stoichiometry.
-   Normalize elemental proportions to atoms per formula unit
    (**apfu**).
-   Summarize the nominal **A**, **X**, and **W** site occupancies.
-   Calculate compositional proportions for Monazite, Cheralite, and
    Huttonite.
-   Plot samples on a labeled ternary diagram with dashed field
    boundaries.

## Notebook workflow

### 1. Import analyses

The notebook mounts Google Drive and reads an Excel workbook using
`pandas.read_excel`. The current input path is:

``` python
data = pd.read_excel("/content/drive/MyDrive/Ycles/Monazita.xlsx")
```

**Update this path to match your own workbook.** The import is currently
tailored to the structure of that spreadsheet: oxide names, element
names, elemental weights, and molecular weights are read from the first
rows, and sample compositions are transposed into a sample-by-oxide
table. It is **not yet a generic Excel importer**.

### 2. Convert to molar proportions

For each oxide, the notebook calculates:

\[ n_i = `\frac{w_i}{M_i}`{=tex} \]

where (w_i) is the reported weight percentage and (M_i) is the molecular
weight. These values are multiplied by the number of elements and oxygen
atoms in each oxide formula to obtain elemental and oxygen molar
proportions. Fluorine and chlorine are handled as elements, not oxides.

### 3. Normalize to apfu

The notebook uses the following spreadsheet-derived convention:

\[ A = `\sum `{=tex}n\_{`\mathrm `{=tex}O} +
`\frac{n_{\mathrm F} + n_{\mathrm {Cl}}}{2}`{=tex} \]

\[ f = `\frac{4}{A}`{=tex}, `\qquad `{=tex}`\mathrm{apfu}`{=tex}\_i =
n_i f \]

This normalization follows the **specific Excel calculation convention**
used to develop the notebook. It should not be assumed to be universally
applicable to all REE minerals or all halogen-bearing phosphate
compositions.

### 4. Summarize structural formula components

The current notebook calculates:

-   **X site:** P + Si
-   **A site:** REE, Y, Ca, Th, U, Pb, Fe, and other cations present
-   **W anion positions:** O, F, and Cl, with O estimated as
    `4 - F - Cl`

The nominal structural representation is **AXW₄**. The notebook reports
site totals rather than automatically generating fully formatted
chemical formulas for every sample.

### 5. Plot a ternary diagram

The current diagram uses the following **operational compositional
proxies**:

  Diagram vertex   Component used
  ---------------- ----------------------------
  **Monazite**     Sum of La--Lu and Y (apfu)
  **Cheralite**    Ca (apfu)
  **Huttonite**    Si (apfu)

The three components are normalized to sum to 1, then converted to
Cartesian coordinates for Matplotlib. The diagram includes a triangular
outline, dashed subdivision lines, field labels, and sample annotations.

**Scientific limitation:** These three quantities are not a complete
charge-balanced end-member decomposition. In particular, the diagram
does not explicitly solve for Th-bearing end-member proportions. Its
field labels should therefore be treated as a compositional
visualization rather than a definitive mineralogical classification.

## Requirements

-   Python 3
-   pandas
-   NumPy
-   Matplotlib
-   An Excel workbook in the expected layout
-   Google Colab for the notebook's current Google Drive mounting step

For local use, install the Python dependencies:

``` bash
pip install pandas numpy matplotlib openpyxl jupyter
```

`openpyxl` is used by pandas to read `.xlsx` files.

## Getting started in Google Colab

1.  Open [`reecalc.ipynb`](reecalc.ipynb) in Google Colab.
2.  Upload your Excel workbook to Google Drive.
3.  Run the Google Drive mounting cell and authorize access.
4.  Change the workbook path in the data-import cell.
5.  Run the notebook cells in order, from data import to ternary
    plotting.
6.  Inspect the apfu and structural-site tables before interpreting the
    diagram.

## Repository structure

``` text
REECalc/
├── README.md
├── LICENSE
└── reecalc.ipynb
```

The Excel workbook is an **external input** and is not included in this
structure.

## Current limitations

-   The workbook parser depends on a fixed row/column layout and a
    hard-coded file path.
-   Missing analytical values are replaced with zero; unmeasured
    elements and true zeros are therefore not distinguished.
-   The normalization convention requires independent mineralogical
    validation.
-   The notebook summarizes site occupancies but does not yet produce
    formatted structural formulas automatically.
-   The ternary diagram is based on Si, Ca, and Σ(REE + Y), not a full
    stoichiometric end-member calculation.
-   No automated analytical-quality or charge-balance validation is
    currently implemented.

## License

REECalc is made available for **non-commercial academic, educational,
and scientific research purposes only**, subject to the terms in
[LICENSE](LICENSE).

Commercial use, including commercial research and paid analytical
services, requires prior written permission from the copyright holder.

**Note:** Because commercial use is restricted, this project is
**source-available**, not open-source under the formal Open Source
Definition.
