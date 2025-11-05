# Dependencies & Environment Setup

## Required Software

- **R**: 4.0 or later
- **Operating System**: Linux, macOS, or Windows

## R Packages

### Required Packages

| Package | Version | Purpose |
|---------|---------|---------|
| `igraph` | ≥ 1.3.0 | Graph data structure and algorithms |
| `stringr` | ≥ 1.4.0 | String manipulation (k-mer extraction) |
| `stringdist` | ≥ 0.9.0 | Edit distance calculations (metrics) |

### Installation

Install all required packages at once:

```r
install.packages(c("igraph", "stringr", "stringdist"))
```

Or install individually:

```r
install.packages("igraph")
install.packages("stringr")
install.packages("stringdist")
```

### Verify Installation

```r
# Load packages to verify
library(igraph)
library(stringr)
library(stringdist)

# Check versions
packageVersion("igraph")
packageVersion("stringr")
packageVersion("stringdist")
```

## Environment Setup

### Option 1: RStudio (Recommended for beginners)

1. Download and install [RStudio Desktop](https://www.rstudio.com/products/rstudio/download/) (Free version)
2. Open RStudio
3. Create a new project in the cloned repository directory
4. Run in RStudio Console:
   ```r
   install.packages(c("igraph", "stringr", "stringdist"))
   source("RUN_ME_FIRST.R")
   ```

### Option 2: Command Line R

```bash
# Navigate to project directory
cd debruijn-assembly-r

# Open R
R

# Inside R console:
install.packages(c("igraph", "stringr", "stringdist"))
source("RUN_ME_FIRST.R")
quit()
```

### Option 3: Rscript (Non-interactive)

```bash
# Navigate to project directory
cd debruijn-assembly-r

# Run all scripts non-interactively
Rscript RUN_ME_FIRST.R
```

## System Requirements

### Minimum
- **CPU**: 1 core (single-threaded)
- **RAM**: 512 MB
- **Storage**: 50 MB for R + packages

### Recommended
- **CPU**: 2+ cores
- **RAM**: 2-4 GB
- **Storage**: 500 MB for R + packages

## Platform-Specific Notes

### Windows

1. Install R from [CRAN](https://cloud.r-project.org/bin/windows/base/)
2. Install RStudio Desktop
3. Open RStudio and run:
   ```r
   install.packages(c("igraph", "stringr", "stringdist"))
   ```

**Note**: Some users report graphviz issues on Windows; igraph falls back gracefully, so visualization works without Graphviz.

### macOS

1. Install R via Homebrew:
   ```bash
   brew install r
   ```
   Or download from [CRAN](https://cloud.r-project.org/bin/macosx/)

2. Install RStudio Desktop from [rstudio.com](https://www.rstudio.com/products/rstudio/download/)

3. In R Console:
   ```r
   install.packages(c("igraph", "stringr", "stringdist"))
   ```

### Linux (Ubuntu/Debian)

```bash
# Install R
sudo apt-get update
sudo apt-get install r-base r-base-dev

# Install RStudio (optional, recommended)
wget https://download1.rstudio.org/desktop/bionic/amd64/rstudio-1.4.1106-amd64.deb
sudo dpkg -i rstudio-1.4.1106-amd64.deb

# In R Console:
install.packages(c("igraph", "stringr", "stringdist"))
```

## Reproducible Setup with renv (Advanced)

For strict reproducibility across environments, use `renv`:

```r
# Install renv
install.packages("renv")

# Initialize renv in project
renv::init()

# Install project dependencies
renv::install(c("igraph", "stringr", "stringdist"))

# Create lock file (commit to git)
renv::snapshot()

# Others can restore environment
renv::restore()
```

## Troubleshooting Installation

### Error: "Cannot install package 'igraph'"

**Solution 1**: Update R to latest version
```r
# Check R version
R.version$version.string

# Update packages
update.packages()
```

**Solution 2**: Install from source (if binary unavailable)
```r
install.packages("igraph", type = "source")
```

### Error: "igraph library not found"

**Cause**: igraph C library dependencies missing

**Windows**: Usually handled automatically

**macOS**: 
```bash
brew install gsl
```

**Linux (Ubuntu)**:
```bash
sudo apt-get install libgsl0-dev libigraph0-dev
```

### Error: "'stringr' is not available for R version X.Y.Z"

**Solution**: Update R to version 4.0+
```bash
# macOS with Homebrew
brew upgrade r

# Ubuntu/Debian
sudo apt-get install r-base --only-upgrade

# Or download from CRAN: https://cloud.r-project.org
```

### Package Installation Hangs

**Solution**: Use different CRAN mirror
```r
# List available mirrors
getCRANmirrors()

# Set to specific mirror
options(repos = "https://cloud.r-project.org")

# Try installation again
install.packages("igraph")
```

## Verification Checklist

After setup, verify everything works:

```r
# 1. Check R version (should be 4.0+)
R.version

# 2. Load required packages
library(igraph)
library(stringr)
library(stringdist)

# 3. Check package versions
packageVersion("igraph")     # Should be ≥ 1.3.0
packageVersion("stringr")    # Should be ≥ 1.4.0
packageVersion("stringdist") # Should be ≥ 0.9.0

# 4. Quick functionality test
test_string <- "ACGTACGT"
nchar(test_string)  # Should return 8
str_sub(test_string, 1, 3)  # Should return "ACG"

# 5. Run the project
source("RUN_ME_FIRST.R")
```

All tests should pass without errors.

## Docker Setup (Optional)

For complete reproducibility, use Docker:

```dockerfile
FROM rocker/r-base:latest

RUN apt-get update && apt-get install -y \
    libgsl0-dev \
    libigraph0-dev

RUN R -e "install.packages(c('igraph', 'stringr', 'stringdist'))"

WORKDIR /app
COPY . .

CMD ["Rscript", "RUN_ME_FIRST.R"]
```

Build and run:
```bash
docker build -t debruijn-assembly .
docker run -v $(pwd):/app debruijn-assembly
```

## Support & Additional Resources

- **R Project**: https://www.r-project.org
- **CRAN Packages**: https://cran.r-project.org/web/packages/available_packages_by_name.html
- **igraph Documentation**: https://igraph.org/r/
- **RStudio Community**: https://community.rstudio.com/
- **Stack Overflow**: Tag with `[r]` and `[igraph]`

## Version History

Last tested with:
- R 4.3.2 (January 2024)
- igraph 1.6.0
- stringr 1.5.1
- stringdist 0.9.10

Older versions may work but are not officially supported.
