# OmicsPLS Short Course
*Massive Open Online Course* | [Said el Bouhaddani](https://www.linkedin.com/in/selbouhaddani/)

## Course organization

The course is organized in two main parts, consisting of theory and exercises. Part one is about principal component analysis, part two is about partial least squares and O2PLS.

### Part one: Principal component analysis

Please download/view the following materials.

- Theory: Slides in [Powerpoint format](link) or in [Adobe pdf format](link).
- Theory: [video lecture on Youtube](link)
- Exercises: [for part one](link)
- Solutions: [for part one](link)

### Part two: Partial least squares and O2PLS

Please download/view the following materials.

- Theory: Slides for PLS in [Powerpoint format](link) or in [Adobe pdf format](link).
- Theory: Slides for O2PLS in [Powerpoint format](link) or in [Adobe pdf format](link).
- Theory: For PLS a [video lecture on Youtube](link)
- Theory: For O2PLS a [video lecture on Youtube](link)
- Exercises: [for part two (PLS and O2PLS)](link)
- Solutions: [for part two (PLS and O2PLS)](link)

## Download instructions

Download the whole repository by clicking above on "code", and then "download ZIP". Save the zip in a suitable location and unpack.

## Installing R packages

First update your R to at least version 4.0.5. Run `R.version()` to see which version you have. It's recommended to update to the most recent version.
Please install the required packages before the course starts.

First run
```
if(compareVersion(paste0(R.Version()[c("major","minor")], collapse = "."), "4.0.5") < 0) cat("Please update R to the latest version (at least 4.0.5)\n")
req_pack <- c("MASS", "parallel", "tidyverse", "magrittr",
              "OmicsPLS", "gplots","BiocManager", "gridExtra", "illuminaHumanv3.db")
if(sum(!(req_pack %in% installed.packages()[,1])) > 0){
  cat("\nThe following packages are missing:\n")
req_pack[which(!(req_pack %in% installed.packages()[,1]))]
} else cat("\nNo packages missing.\n")
```

Then select from the commands below to install the missing packages (if any).
```
install.packages("MASS")      # statistical tools, such as lda
install.packages("tidyverse") # dataset & viz tools
install.packages("magrittr")  # pipe operators
install.packages("gplots")      # http tools
install.packages("gridExtra")    # extra ggplot2 tools
install.packages("BiocManager") # bioconductor packages
install.packages("OmicsPLS")  # data integration toolkit
# BiocManager::install("illuminaHumanv3.db") ## NOTE this will require additional packages
```


#### Quick troubleshooting

- _There is a problem with the html or other files._ Try downloading the whole repository as a zip file. Extract the zip to a folder and try again. If the issue remains, please contact me (email: s.elbouhaddani at umcutrecht.nl).
- _I get "These packages have more recent versions available" with a list of packages._ The best way is to enter 1 or 2 (All or CRAN only) and update old packages. In principle, you should be able to still install all packages while selecting 3 (none), but it's recommended to regularly update R and the packages.
- _I get "package 'xxx' is not available (for R version x.y.z)" when trying to install a package._ Most likely this is due to an outdated R version. Please update your R to at least version 4.0.5. Run `R.version` to see which version you have. It could also be that a dependent package is missing, check **all** errors, warnings and messages to see which package is the culprit. Note that `illuminaHumanv3.db` depends on many other packages.
- _I get another strange message._ Did you update to at least R 4.0.5? If yes, please contact me (email: s.elbouhaddani at umcutrecht.nl) or open a new issue (see button Issues above). Please carefully describe what you did, and also copy-paste the output of `sessionInfo()` in your message.


## Evaluation form

Please fill in the short evaluation form. Your feedback is much appreciated.

[Click here for the form](https://forms.gle/6FCrtwQk5Qs5hsUs6).


## Reading material

> Bouhaddani, S., Uh, HW., Jongbloed, G. et al. Integrating omics datasets with the OmicsPLS package. BMC Bioinformatics 19, 371 (2018). https://doi.org/10.1186/s12859-018-2371-3

> Inouye, M., Kettunen, J., Soininen, P., Silander, K., Ripatti, S., Kumpula, L. S., Hämäläinen, E., Jousilahti, P., Kangas, A. J., Männistö, S., Savolainen, M. J., Jula, A., Leiviskä, J., Palotie, A., Salomaa, V., Perola, M., Ala-Korpela, M., & Peltonen, L. (2010). Metabonomic, transcriptomic, and genomic variation of a population cohort. Mol. Syst. Biol., 6(441), 441. https://doi.org/10.1038/msb.2010.93

> Cook, R. D. (2022). A slice of multivariate dimension reduction. Journal of Multivariate Analysis, 188, 104812. https://doi.org/10.1016/j.jmva.2021.104812

> Gu, Z., el Bouhaddani, S., Pei, J. et al. Statistical integration of two omics datasets using GO2PLS. BMC Bioinformatics 22, 131 (2021). https://doi.org/10.1186/s12859-021-03958-3

> el Bouhaddani, S., Uh, H.-W., Jongbloed, G., & Houwing‐Duistermaat, J. (2022). Statistical integration of heterogeneous omics data: Probabilistic two‐way partial least squares (PO2PLS). Journal of the Royal Statistical Society: Series C (Applied Statistics). https://doi.org/10.1111/rssc.12583
