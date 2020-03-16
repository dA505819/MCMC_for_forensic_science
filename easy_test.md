# Easy Test for MCMC for forensic science

Including the required libraries and the dataset.
```
library(seqinr)
library(MCMCpack)

#Reading downloaded data and loading it in data variabe
Data <- read.abif("A02-RD12-0002-35-0.5PP16-001.5sec.fsa")
```
Obtaining the plot.
```
plotabif(Data)
```
**Plot obtained with Data**
![plot_easy](https://github.com/dA505819/MCMC_for_forensic_science/blob/master/images/easytest.png)
