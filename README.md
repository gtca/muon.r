# muon for R

Muon for R (`muon.r`) is an experimental package to provide I/O functionality for `.h5mu` files and Seurat/MultiAssayExperiment objects. 

`muon.r` saves R objects with multimodal data to `.h5mu` files that can be further integrated into workflows in multiple programming languages, including the [`muon` Python library](https://github.com/pmbio/muon) and the [`Muon.jl` Julia library](https://github.com/pmbio/Muon.jl). `muon.r` also reads `.h5mu` files into R objects. 

You can learn more about multimodal data containers in the [`muon` documentation](https://muon.readthedocs.io/en/latest/io/mudata.html).

## Installation

```R
remotes::install_github("pmbio/muon.r")
```

## Quick start

`muon.r` provides a set of I/O operations for multimodal data.

`muon.r` implements a generic `WriteH5MU` function that currently works for Seurat objects (v3 and above) and MultiAssayExperiment objects. `ReadH5MU` function allows to make a Seurat or a MultiAssayExperiment object with the data from the `.h5mu` file.

### Writing files

Seurat or MultiAssayExperiment objects can be serialized into `.h5mu` files.

#### Seurat objects

Start with an existing dataset, e.g. a [Seurat](https://github.com/satijalab/seurat) object with CITE-seq data:

```R
library(SeuratData)
InstallData("bmcite")
bm <- LoadData(ds = "bmcite")
```

`muon.r` allows to save the object into a `.h5mu` file:

```R
library(muon)
WriteH5MU(bm, "bmcite.h5mu")
```

#### MultiAssayExperiment objects

Start with a dataset, e.g. the `myMultiAssay` object from [the MAE vignette](https://www.bioconductor.org/packages/release/bioc/vignettes/MultiAssayExperiment/inst/doc/MultiAssayExperiment.html):

```R
library(muon)
WriteH5MU(myMultiAssay, "myMultiAssay.h5mu")
```

### Reading files

Data from `.h5mu` files can be read into Seurat or MultiAssayExperiment objects.

#### Seurat objects

```R
bm <- ReadH5MU("bmcite.h5mu", "seurat")
```

Please note that only the intersection of cells is currently loaded into the Seurat object due to the object structure limitation. Multimodal embeddings (global `.obsm` slot) are loaded with the `assay.used` field set to the default assay. Embeddings names are changed in order to comply with R & Seurat requirements and conventions.

#### MultiAssayExperiment objects

```R
myMultiAssay <- ReadH5MU("myMultiAssay.h5mu", "mae")
```

Please note that some of the fields such as multimodal embeddings (global `.obsm` slot) are not read into the MultiAssayExperiment object due to the object structure limitations.

## Relevant projects

Other R packages for multimodal I/O include:

- [SeuratDisk](https://github.com/mojaveazure/seurat-disk)
