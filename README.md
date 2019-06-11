knit: (function(input_file, encoding) {
  out_dir <- 'docs';
  rmarkdown::render(input_file,
 encoding=encoding,
 output_file=file.path(dirname(input_file), out_dir, 'index.html'))})

# Tutorial: Overview of machine learning methods in Big Data epidemiology

"R Notebook for workshop Introduction to Big Data Analytics & Multi-site Retrospective Epidemiological Studies"

# 1. Environment setup

All analyses are done in R using RStudio. For detailed session information including R version, operating system and package versions, see the sessionInfo() output at the end of this document.

## Install packages

```{r}
sessionInfo()
# Only install missing libraries
install.packages('ggplot2')
install.packages('tidyverse')
install.packages('mice')
install.packages('readr')
```

## Load packages

```{r}
library(tidyverse)  # for tidy data analysis
library(readr)      # for fast reading of input files
library(mice)       # mice package for Multivariate Imputation by Chained Equations (MICE)
```

# 2. Data preparation

The dataset used in these analyses is the Breast Cancer Wisconsin (Diagnostic) Dataset. The data was downloaded from the UC Irvine Machine Learning Repository
File: breast-cancer-wisconsin.data (https://tinyurl.com/ml-uct-2019-data).

The first dataset looks at the predictor classes:
* malignant or
* benign breast mass.

The features characterise cell nucleus properties and were generated from image analysis of fine needle aspirates (FNA) of breast masses:
* Sample ID (code number)
* Clump thickness
* Uniformity of cell size
* Uniformity of cell shape
* Marginal adhesion
* Single epithelial cell size
* Number of bare nuclei
* Bland chromatin
* Number of normal nuclei
* Mitosis
* Classes, i.e. diagnosis

```{r}
# Change to the directory where you downloaded the data
setwd("~/Downloads") 

bc_data <- read_delim("breast-cancer-wisconsin.data",
                      delim = ",",
                      col_names = c("sample_code_number", 
                       "clump_thickness", 
                       "uniformity_of_cell_size", 
                       "uniformity_of_cell_shape", 
                       "marginal_adhesion", 
                       "single_epithelial_cell_size", 
                       "bare_nuclei", 
                       "bland_chromatin", 
                       "normal_nucleoli", 
                       "mitosis", 
                       "classes")) %>%
  mutate(bare_nuclei = as.numeric(bare_nuclei),
         classes = ifelse(classes == "2", "benign",
                          ifelse(classes == "4", "malignant", NA)))
```

```{r}
summary(bc_data)
```


