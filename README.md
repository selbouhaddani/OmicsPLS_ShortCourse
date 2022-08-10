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

Please install the following packages before the course starts.

```
install.packages("tidyverse") # dataset & viz tools
install.packages("magrittr")  # pipe operator
install.packages("plotly")    # interactive plots

install.packages("OmicsPLS")  # data integration toolkit

install.packages("devtools")  # install experimental packages
devtools::install_bitbucket("ibi_group/disgenet2r") ## Experimental R package
```

#### Quick troubleshooting

- _I get "package 'xxx' is not available (for R version x.y.z)" when trying to install a package._ Most likely this is due to an outdated R version. Strictly speaking, the packages in this course require an R version of at least 3.5.0, but it's highly recommended to update R to version 4.x.x. It could also be that a dependent package is missing, check **all** error and warning messages to see which package is the culprit. Finally, note that the package `disgenet2r` should be installed with a specific command: `devtools::install_bitbucket("ibi_group/disgenet2r")`. For this, you need `devtools`. 
- _I get another strange message._ Please contact me (s.elbouhaddani at umcutrecht.nl) or open a new issue (see button Issues above). Please carefully describe what you did, and also copy-paste the output of `sessionInfo()` in your message. 


## Evaluation form

Please fill in the short evaluation form. Your feedback is much appreciated. 

[Click here for the form](https://forms.gle/w6Tj3MSeRYZ7HaaW6) (the form will be activated at the end of the course). 


## Reading material

> Bouhaddani, S., Uh, HW., Jongbloed, G. et al. Integrating omics datasets with the OmicsPLS package. BMC Bioinformatics 19, 371 (2018). https://doi.org/10.1186/s12859-018-2371-3

> Gu, Z., el Bouhaddani, S., Pei, J. et al. Statistical integration of two omics datasets using GO2PLS. BMC Bioinformatics 22, 131 (2021). https://doi.org/10.1186/s12859-021-03958-3

> Cook, R. D. (2022). A slice of multivariate dimension reduction. Journal of Multivariate Analysis, 188, 104812. https://doi.org/10.1016/j.jmva.2021.104812



