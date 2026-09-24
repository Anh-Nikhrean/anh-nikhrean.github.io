# anh-nikhrean.github.io

# Personal Website

This repository contains my Quarto website with reproducible computational posts written in R and Python.

The website includes a home page, an about page, a blog listing, and computational posts. Quarto renders the website into the `docs/` directory for publication with GitHub Pages.

## Software requirements

The following software is required to reproduce the website:

- Quarto: `1.10.18`
- uv: `0.12.7`
- Python: `3.14`
- R: `4.6.1`
- Git

The Python environment is managed with `uv` using:

- `pyproject.toml`
- `uv.lock`
- `.python-version`

The R environment is managed with `renv` using:

- `renv.lock`
- `.Rprofile`
- `renv/activate.R`

`renv` does not need to be installed manually before cloning the repository because it bootstraps itself from the project files.

To check the software versions installed on your machine, run:

```bash
quarto --version
uv --version
python --version
R --version
```

## Build the website from a clean clone

All commands below should be run from the **top level of the repository** unless otherwise stated.

### 1. Clone the repository

```bash
git clone git@github.com:Anh-Nikhrean/anh-nikhrean.github.io.git
cd anh-nikhrean.github.io
```

### 2. Restore the Python environment

Run:

```bash
uv sync
```

This creates the project `.venv` and installs the exact Python package versions recorded in `uv.lock`.

Do not install packages separately with `pip`. The Python dependencies required by the computational post are managed through `uv`.

### 3. Restore the R environment

Start R from the **top level of the repository**:

```bash
R
```

Then, inside the R console, run:

```r
renv::restore()
```

This installs the R package versions recorded in `renv.lock`.

When the restore has completed, exit R:

```r
q()
```

If R asks whether to save the workspace image, choose:

```text
n
```

### 4. Render the website

From the **top level of the repository**, run:

```bash
uv run quarto render
```

`uv run` ensures that the Python computational post is executed using the Python environment stored in this project's `.venv`.

Running the command from the repository root is also important for the R post because R reads the project's `.Rprofile` and activates the `renv` environment.

Quarto renders the complete website, including the R and Python computational posts.

## Built site

The rendered website is written to:

```text
docs/
```

The main local HTML page is:

```text
docs/index.html
```

To preview the website locally before or after rendering, run:

```bash
uv run quarto preview
```

Quarto will display a local URL in the terminal, usually similar to:

```text
http://localhost:XXXX/
```

Open that URL in a web browser.

The published website is served by GitHub Pages from the `docs/` directory.

## Data

The computational posts use the
[Palmer Penguins dataset](https://allisonhorst.github.io/palmerpenguins/),
which contains measurements of Adelie, Chinstrap, and Gentoo penguins from the Palmer Archipelago in Antarctica.

The data are available under a **CC0 licence ("No Rights Reserved")**. The R and Python posts access the dataset through the `palmerpenguins` package rather than downloading a separate CSV file during rendering. :contentReference[oaicite:1]{index=1}

### Dataset citation

Horst, A. M., Hill, A. P., & Gorman, K. B. (2020). *palmerpenguins: Palmer Archipelago (Antarctica) penguin data*. R package version 0.1.0. https://doi.org/10.5281/zenodo.3960218

The data were originally published in:

Gorman, K. B., Williams, T. D., & Fraser, W. R. (2014). Ecological sexual dimorphism and environmental variability within a community of Antarctic penguins (genus *Pygoscelis*). *PLoS ONE, 9*(3), e90081. https://doi.org/10.1371/journal.pone.0090081

## Network requirements

An internet connection is required when setting up the project from a clean clone because:

1. `uv sync` may need to download the Python packages recorded in `uv.lock`.
2. `renv::restore()` may need to download the R packages recorded in `renv.lock`.

Once all required packages are installed, the Palmer Penguins analysis itself does not need to fetch the dataset from the internet during rendering.

## Repository structure

The main project files are organized approximately as follows:

```text
USERNAME.github.io/
├── _quarto.yml
├── README.md
├── index.qmd
├── about.qmd
├── blog.qmd
├── pyproject.toml
├── uv.lock
├── .python-version
├── renv.lock
├── .Rprofile
├── renv/
│   └── activate.R
├── posts/
│   ├── first-weeks/
│   │   └── index.qmd
│   ├── python-penguins/
│   │   └── index.qmd
│   └── r-penguins/
│       └── index.qmd
└── docs/
```

The source files are the `.qmd` files. The generated website files are written to `docs/`.

## Reproducibility test

To test the project from a completely fresh clone, clone it into a temporary directory:

```bash
git clone git@github.com:USERNAME/USERNAME.github.io.git ~/tmp/m3-test
cd ~/tmp/m3-test
```

Then reproduce the site by following the same sequence:

```bash
uv sync
R
```

Inside R:

```r
renv::restore()
q()
```

Then back in the terminal:

```bash
uv run quarto render
```

After a successful build, confirm that the following file exists:

```text
docs/index.html
```

and open the site locally or preview it with:

```bash
uv run quarto preview
```