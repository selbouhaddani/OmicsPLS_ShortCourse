# OmicsPLS Short Course
*2022 Edition* | [Said el Bouhaddani](https://www.linkedin.com/in/selbouhaddani/) and [Jeanine Houwing-Duistermaat](https://www.linkedin.com/in/jeanine-houwing-duistermaat/)

## Instructions to download material

Please click on one of the links below. Download the files: **right click on the 'download' button** and click "save link as". Check that the extension is '.html' or '.RData'. 

- https://github.com/selbouhaddani/OmicsPLS_ShortCourse/blob/main/OmicsPLS_shortCourse.html
- https://github.com/selbouhaddani/OmicsPLS_ShortCourse/blob/main/OmicsPLS_shortCourse_ANSWERS.html
- https://github.com/selbouhaddani/OmicsPLS_ShortCourse/blob/main/DownSyndrome.RData

The course slides are in this folder. Click on one of the slides and right click on the download button + "save link as" as described above. 

- https://github.com/selbouhaddani/OmicsPLS_ShortCourse/tree/main/Slides

Alternatively, download the whole repo by clicking above on "code", and then "download ZIP".


## Installing R packages

First update your R to at least version 4.0.5. Run `R.version` to see which version you have. It's recommended to update to the most recent version. 
Please install the required packages before the course starts.

First run 
```
if(compareVersion(paste0(R.Version()[c("major","minor")], collapse = "."), "4.0.5") < 0) cat("Please update R to the latest version (at least 4.0.5)\n")
req_pack <- c("MASS", "parallel", "tidyverse", "magrittr", 
              "OmicsPLS", "httr", "disgenet2r", "GGally")
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
install.packages("httr")      # http tools
install.packages("GGally")    # extra ggplot2 tools
install.packages("OmicsPLS")  # data integration toolkit

install.packages("remotes")   # install experimental packages
remotes::install_bitbucket("ibi_group/disgenet2r") # experimental R package
```

#### Installing disgenet2r

To install disgenet2r, you need to be able to compile packages from source. Practically, this means that you need to install some additional tools. You will need [Rtools (Windows)](https://cran.r-project.org/bin/windows/Rtools/) or [Xcode (MacOS)](https://developer.apple.com/xcode/) which have separate installation instructions. Please first check whether they are already installed, e.g. by looking at the list of installed programs.

Next, check whether you have the package `SPARQL` (e.g. by running `library(SPARQL)`). If not, you need to install this package from the CRAN archive: `install.packages("https://cran.r-project.org/src/contrib/Archive/SPARQL/SPARQL_1.16.tar.gz", repos=NULL, type="source")`. 

Finally, install disgenet2r with `remotes::install_bitbucket("ibi_group/disgenet2r")`. 

If the installation was not successful, it will not be a problem for the course. 

#### Quick troubleshooting

- _There is a problem with the html or other files._ If the one-by-one download didn't work, try downloading the whole repository as a zip file. Extract the zip to a folder and try again. If the issue remains, please contact me (email: s.elbouhaddani at umcutrecht.nl). 
- _I get "These packages have more recent versions available" with a list of packages._ The best way is to enter 1 or 2 (All or CRAN only) and update old packages. In principle, you should be able to still install all packages while selecting 3 (none), but it's recommended to regularly update R and the packages. 
- _I get "package 'xxx' is not available (for R version x.y.z)" when trying to install a package._ Most likely this is due to an outdated R version. Please update your R to at least version 4.0.5. Run `R.version` to see which version you have. It could also be that a dependent package is missing, check **all** errors, warnings and messages to see which package is the culprit. Note that `disgenet2r` needs a special approach, described above. 
- _I get another strange message._ Did you update to at least R 4.0.5? When trying to install `disgenet2r`, do you have Rtools or Xcode installed? If yes, please contact me (email: s.elbouhaddani at umcutrecht.nl) or open a new issue (see button Issues above). Please carefully describe what you did, and also copy-paste the output of `sessionInfo()` in your message. 


## Assignments

The exercises are given inline. They typically refer to the next code block. The answers and code output are given in the file ending with ANSWERS.html. 


## Evaluation form

Please fill in the short evaluation form. Your feedback is much appreciated. 

[Click here for the form](https://forms.gle/w6Tj3MSeRYZ7HaaW6) (the form will be activated at the end of the course). 


## Reading material

> Bouhaddani, S., Uh, HW., Jongbloed, G. et al. Integrating omics datasets with the OmicsPLS package. BMC Bioinformatics 19, 371 (2018). https://doi.org/10.1186/s12859-018-2371-3

> Vincenzo Borelli, Valerie Vanhooren, Emanuela Lonardi, Karli R. Reiding, Miriam Capri, Claude Libert, Paolo Garagnani, Stefano Salvioli, Claudio Franceschi, and Manfred Wuhrer. Journal of Proteome Research 2015 14 (10), 4232-4245. https://doi.org/10.1021/acs.jproteome.5b00356

> Bacalini, M. G., Gentilini, D., Boattini, A., Giampieri, E., Pirazzini, C., Giuliani, C., Fontanesi, E., Scurti, M., Remondini, D., Capri, M., Cocchi, G., Ghezzo, A., Del Rio, A., Luiselli, D., Vitale, G., Mari, D., Castellani, G., Fraga, M., Di Blasio, A. M., … Garagnani, P. (2015). Identification of a DNA methylation signature in blood cells from persons with Down Syndrome. Aging, 7(2), 82–96. https://doi.org/10.18632/aging.100715

> Cook, R. D. (2022). A slice of multivariate dimension reduction. Journal of Multivariate Analysis, 188, 104812. https://doi.org/10.1016/j.jmva.2021.104812

> Gu, Z., el Bouhaddani, S., Pei, J. et al. Statistical integration of two omics datasets using GO2PLS. BMC Bioinformatics 22, 131 (2021). https://doi.org/10.1186/s12859-021-03958-3

> el Bouhaddani, S., Uh, H.-W., Jongbloed, G., & Houwing‐Duistermaat, J. (2022). Statistical integration of heterogeneous omics data: Probabilistic two‐way partial least squares (PO2PLS). Journal of the Royal Statistical Society: Series C (Applied Statistics). https://doi.org/10.1111/rssc.12583




