# Medium Test

Including the required libraries and the dataset.
```
library(seqinr)
library(MCMCpack)
library(ggplot2)

#Reading downloaded data and loading it in Data variable
Data <- read.abif("A02-RD12-0002-35-0.5PP16-001.5sec.fsa")
data_Data<-Data$Data
```
Creating dataframe df_1 storing DATA.4 values from data_DATA.
```
Time <- data_Data$DATA.4
integer_values <- c(-2:length(data_Data$DATA.4[c(-12458,-12459,-12460)]))
df_1 <- data.frame(Time, integer_values)
df_1$number_of_data <- "DATA.4"
```
Creating dataframe df_2 storing DATA.3 values from data_DATA.
```
Time <- data_Data$DATA.3
integer_values <- c(-2:length(data_Data$DATA.3[c(-12458,-12459,-12460)]))
df_2 <- data.frame(Time, integer_values)
df_2$number_of_data <- "DATA.3"
```
Creating dataframe df_3 storing DATA.2 values from data_DATA.
```
Time <- data_Data$DATA.2
integer_values <- c(-2:length(data_Data$DATA.2[c(-12458,-12459,-12460)]))
df_3 <- data.frame(Time, integer_values)
df_3$number_of_data <- "DATA.2"
```
Combining df_1, df_2 and df_3.
```
combi = rbind(df_1, df_2, df_3)
```
Plotting the dataframe combi.
```
ggplot(combi) + geom_line(aes(Time, integer_values)) + facet_grid(factor(number_of_data) ~ ., scales = "free", 
   space = "free", shrink = TRUE, as.table = TRUE, switch = "x", margins = TRUE) + 
   xlab("Time/1000") + 
   ylab("RFU/1000") +
   ggtitle("Data ; 1 ; 5-FAM")
```
![plot](https://github.com/dA505819/MCMC_for_forensic_science/blob/master/images/medium_test.png)
