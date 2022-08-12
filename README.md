# OmicsPLS Short Course
*2022 Edition* | [Said el Bouhaddani](https://www.linkedin.com/in/selbouhaddani/) and [Jeanine Houwing-Duistermaat](https://www.linkedin.com/in/jeanine-houwing-duistermaat/)

## Instructions to download material

Please click on one of the links below. Download the files: **right click on the 'download' button** and click "save link as". Check that the extension is '.html' or '.RData'. 

- https://github.com/selbouhaddani/OmicsPLS_ShortCourse/blob/main/OmicsPLS_shortCourse.html
- https://github.com/selbouhaddani/OmicsPLS_ShortCourse/blob/main/OmicsPLS_shortCourse_ANSWERS.html
- https://github.com/selbouhaddani/OmicsPLS_ShortCourse/blob/main/DownSyndrome.RData

Alternatively, download the whole repo by clicking above on "code", and then "download ZIP".

## Assignments

The exercises are given inline. They typically refer to the next code block. The answers and code output are given in the file ending with ANSWERS.html. 

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

#### Quick troubleshooting

- _I get "These packages have more recent versions available" with a list of packages._ The best way is to enter 1 or 2 (All or CRAN only) and update old packages. In principle, you should be able to get away with selecting 3 (none), but it's recommended to regularly update R and the packages. 
- _I get "package 'xxx' is not available (for R version x.y.z)" when trying to install a package._ Most likely this is due to an outdated R version. Please update your R to at least version 4.0.5. Run `R.version` to see which version you have. It could also be that a dependent package is missing, check **all** error and warning messages to see which package is the culprit. Note that `disgenet2r` needs a special approach, see next point. 
- _I get "dependency 'SPARQL' not available for package 'disgenet2r'"._ This is a more elaborate issue. You will need [Rtools (Windows)](https://cran.r-project.org/bin/windows/Rtools/) or [Xcode (MacOS)](https://developer.apple.com/xcode/) which have separate installation instructions. Please first check whether they are already installed. These tools allow you to install packages from source. Then run `install.packages("https://cran.r-project.org/src/contrib/Archive/SPARQL/SPARQL_1.16.tar.gz", repos=NULL, type="source")`. Then, retry installing `disgenet2r`. 
- _I get another strange message._ Did you update to at least R 4.0.5? Did you install Rtools or Xcode when installing `disgenet2r`? If yes, please contact me (email: s.elbouhaddani at umcutrecht.nl) or open a new issue (see button Issues above). Please carefully describe what you did, and also copy-paste the output of `sessionInfo()` in your message. 


## Evaluation form

Please fill in the short evaluation form. Your feedback is much appreciated. 

[Click here for the form](https://forms.gle/w6Tj3MSeRYZ7HaaW6) (the form will be activated at the end of the course). 


## Reading material

> Bouhaddani, S., Uh, HW., Jongbloed, G. et al. Integrating omics datasets with the OmicsPLS package. BMC Bioinformatics 19, 371 (2018). https://doi.org/10.1186/s12859-018-2371-3

> Vincenzo Borelli, Valerie Vanhooren, Emanuela Lonardi, Karli R. Reiding, Miriam Capri, Claude Libert, Paolo Garagnani, Stefano Salvioli, Claudio Franceschi, and Manfred Wuhrer. Journal of Proteome Research 2015 14 (10), 4232-4245. https://doi.org/10.1021/acs.jproteome.5b00356

> Cook, R. D. (2022). A slice of multivariate dimension reduction. Journal of Multivariate Analysis, 188, 104812. https://doi.org/10.1016/j.jmva.2021.104812

> Gu, Z., el Bouhaddani, S., Pei, J. et al. Statistical integration of two omics datasets using GO2PLS. BMC Bioinformatics 22, 131 (2021). https://doi.org/10.1186/s12859-021-03958-3




