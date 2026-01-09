# Bi-vAriate Genomic Footprinting (BaGFoot)

BaGFoot is an R package for Bi-vAriate Genomic Footprinting analysis.

## Overview

The BaGFoot package facilitates the analysis of genomic footprinting data. It provides tools for processing BAM files, generating cut count files, correcting for hexamer bias, and performing bivariate genomic footprinting analysis.

## Dependencies

The package requires the following system and R dependencies.

### System Dependencies

*   **R** (>= 3.2.0)
*   **Tcl/Tk** (version 8.6 or higher)
*   **X11** (required for plotting functions)
*   **Samtools**
*   **Build Essentials** (C compiler, Make, etc.)
*   **Libraries**:
    *   `libncurses5-dev`
    *   `zlib1g-dev`
    *   `libbz2-dev`
    *   `liblzma-dev`
    *   `libcurl3-dev`

### R Dependencies

**CRAN Packages:**
*   `hash`
*   `parallel`
*   `digest`
*   `data.table`
*   `Cairo`
*   `aplpack`
*   `devtools`

**Bioconductor Packages:**
*   `BiocManager`
*   `BSgenome.Hsapiens.UCSC.hg19`
*   `BSgenome.Mmusculus.UCSC.mm9`
*   `BSgenome.Mmusculus.UCSC.mm10` (optional)
*   `BSgenome.Hsapiens.UCSC.hg38` (optional)
*   `BSgenome.Rnorvegicus.UCSC.rn5` (optional)
*   `BSgenome.Rnorvegicus.UCSC.rn6` (optional)
*   `Rsamtools`
*   `GenomeInfoDb`
*   `GenomicRanges`
*   `IRanges`

## Installation

### Option 1: Docker (Recommended)

A `Dockerfile` is provided to build a container with all necessary dependencies and the BaGFoot package installed.

1.  **Build the Docker image:**

    ```bash
    sudo docker build -t bagfoot .
    ```

2.  **Run the Docker container:**

    *   **Linux**:
        To enable X11 forwarding (for plotting), you need to share your X authority file.

        ```bash
        XAUTH=$HOME/.Xauthority
        touch $XAUTH
        sudo docker run --tty --interactive --network=host --env DISPLAY=$DISPLAY --volume $XAUTH:/root/.Xauthority bagfoot bash
        ```

    *   **Mac**:
        To run on Mac, you need XQuartz.
        1.  Install and run **XQuartz**.
        2.  In XQuartz preferences -> Security, check "Allow connection from network clients".
        3.  Restart XQuartz.
        4.  Ensure it's listening on port 6000: `lsof -i :6000`
        5.  Allow your local machine to talk to XQuartz:
            ```bash
            IP=$(ifconfig en0 | grep inet | awk '$1=="inet" {print $2}')
            xhost + ${IP}
            ```
        6.  Run the container:
            ```bash
            sudo docker run -ti --rm -e DISPLAY=${IP}:0 -e XAUTHORITY=/.Xauthority -v /tmp/.X11-unix:/tmp/.X11-unix -v ~/.Xauthority:/.Xauthority bagfoot bash
            ```

### Option 2: Manual Installation

If you prefer to install locally:

1.  **Install System Dependencies:**
    (Commands for Ubuntu/Debian)
    ```bash
    sudo apt-get update
    sudo apt-get install -y build-essential wget tcl8.6-dev tk8.6-dev \
        libncurses5-dev zlib1g-dev libbz2-dev liblzma-dev libcurl3-dev \
        x11-apps
    ```
    Ensure `samtools` is installed and in your PATH.

2.  **Install R Packages:**

    Open R and run:
    ```R
    install.packages(c('hash', 'parallel', 'digest', 'data.table', 'Cairo', 'aplpack', 'devtools'))

    if (!requireNamespace("BiocManager", quietly = TRUE))
        install.packages("BiocManager")

    BiocManager::install(c(
        'BSgenome.Mmusculus.UCSC.mm9',
        'BSgenome.Hsapiens.UCSC.hg19',
        'Rsamtools',
        'GenomeInfoDb',
        'GenomicRanges',
        'IRanges'
    ))
    ```

3.  **Install BaGFoot:**

    You can install the package from GitHub:
    ```R
    devtools::install_github('sojbaek/bagfootr')
    ```
    (Note: Replace `'sojbaek/bagfootr'` with the correct repository path if different).

    Alternatively, if you have the source code locally:
    ```R
    devtools::install('.')
    ```

## Usage

Example scripts are provided in the `example/` directory.

*   `bagfoot_prep_example.R`: Prepares required data files (e.g., cut counts, bias correction tables).
*   `bagfoot_run_example.R`: Runs the BaGFoot analysis on the prepared data.

**Note:**
*   BaGFoot requires at least **16GB of memory**.
*   The `example_data` used in the scripts might need to be downloaded separately if not present. The Dockerfile contains instructions on where to fetch them (SourceForge).

To run the example in R:
```R
# Ensure you are in the directory containing the scripts and have the necessary data
source("example/bagfoot_run_example.R")
```

## License

GNU General Public License, v 3.0
