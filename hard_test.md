# Hard Test

We need to demonstrate our capability in one of the "Bayesian packages for general model fitting". I choose MCMCpack package which is commonly used in social and behavioral sciences.

Including all the libraries and the data.
```
library(seqinr)
library(MCMCpack)
library(ggplot2)
library(dplyr)
library(data.table)

#Reading downloaded data and loading it in data variabe
Data <- read.abif("A02-RD12-0002-35-0.5PP16-001.5sec.fsa")
data_Data<-Data$Data
```
Creating dataframes df_1, df_2 and df_3 storing data from data_Data variable.
```
Time <- data_Data$DATA.4
integer_values <- c(-2:length(data_Data$DATA.4[c(-12458,-12459,-12460)]))
df_1 <- data.frame(Time, integer_values)
df_1$number_of_data <- "DATA.4"

#Creating dataframe df_2 storing DATA.3 values from data_DATA
Time <- data_Data$DATA.3
integer_values <- c(-2:length(data_Data$DATA.3[c(-12458,-12459,-12460)]))
df_2 <- data.frame(Time, integer_values)
df_2$number_of_data <- "DATA.3"

#Creating dataframe df_3 storing DATA.2 values from data_DATA
Time <- data_Data$DATA.2
integer_values <- c(-2:length(data_Data$DATA.2[c(-12458,-12459,-12460)]))
df_3 <- data.frame(Time, integer_values)
df_3$number_of_data <- "DATA.2"
```
Combining df_1, df_2 and df_3.
```
combi = rbind(df_1, df_2, df_3)
```
Getting the summary of the dataframe.
```
summary(combi)

 Time         integer_values  number_of_data    
 Min.   :-369.00   Min.   :   -2   Length:37380      
 1st Qu.:  -8.00   1st Qu.: 3113   Class :character  
 Median :  -4.00   Median : 6228   Mode  :character  
 Mean   :  99.18   Mean   : 6228                     
 3rd Qu.:   5.00   3rd Qu.: 9342                     
 Max.   :8614.00   Max.   :12457     
 ```
 Missing value treatment before building a linear regression model of Bayesian inference to check the number of NA values in data.
 ```
sapply(combi, function(df)
{
  sum(is.na(df)==T)/length(df)
})

 Time integer_values number_of_data 
             0              0              0 
```
Label encoding is done to convert character data in number_of_data column into numerical data.
```
combi = data.table(combi)
combi[, number_of_data := ifelse(number_of_data == "DATA.4", 0,
                          ifelse(number_of_data == "DATA.3", 1,2))]
```
Building linear regression model.
```
linear_reg_bayesian = MCMCregress(integer_values ~., data = combi, b0 = 10, B0 = 10)
```
Summary of linear_reg_bayesian
```
summary(linear_reg_bayesian)

Iterations = 1001:11000
Thinning interval = 1 
Number of chains = 1 
Sample size per chain = 10000 

1. Empirical mean and standard deviation for each variable,
   plus standard error of the mean:

                    Mean        SD  Naive SE Time-series SE
(Intercept)    1.045e+01 3.155e-01 3.155e-03      3.142e-03
Time           9.455e-01 5.900e-02 5.900e-04      5.900e-04
number_of_data 1.044e+01 3.235e-01 3.235e-03      3.285e-03
sigma2         5.136e+07 3.779e+05 3.779e+03      3.713e+03

2. Quantiles for each variable:

                    2.5%       25%       50%       75%     97.5%
(Intercept)    9.838e+00 1.023e+01 1.044e+01 1.066e+01 1.107e+01
Time           8.305e-01 9.046e-01 9.461e-01 9.851e-01 1.062e+00
number_of_data 9.811e+00 1.023e+01 1.044e+01 1.067e+01 1.107e+01
sigma2         5.062e+07 5.111e+07 5.136e+07 5.162e+07 5.211e+07
```
Getting Posterior draws for first 20 draws.
```
linear_reg_bayesian[1:20,]

(Intercept)      Time number_of_data   sigma2
 [1,]   10.928050 1.0284310      10.404956 51743078
 [2,]   10.349704 0.8934430      10.566269 51795884
 [3,]   10.609800 1.0187130      10.242217 51266525
 [4,]   10.623518 1.0545976      10.429270 51340750
 [5,]   10.334339 0.9998537      10.463740 51674695
 [6,]   10.714258 1.0094719      10.467513 51572084
 [7,]   10.789743 1.1144858      10.628360 50877085
 [8,]   10.473759 0.9198476      10.610158 51068304
 [9,]   10.960867 1.0076564      10.408610 51812300
[10,]    9.922165 0.8966139      10.191644 50533747
[11,]   10.367852 0.9385160      10.678422 51858923
[12,]   10.119237 1.0109879      10.665742 51963233
[13,]    9.860231 0.9079747      10.420874 51658262
[14,]   10.115110 0.9903675      10.625105 51165245
[15,]   10.081448 1.0107386      10.730440 52420296
[16,]   10.725551 1.0238141       9.644623 50840942
[17,]   10.389474 0.9434154      10.479885 51325291
[18,]   10.459365 0.8170686      10.570132 52051453
[19,]   10.677391 1.1432152      10.703647 51287689
[20,]   10.655549 0.9085740       9.877015 51043883
```
Analyzing posterior draws using apply function with margin = 2(applying for column) for quantile function and probabilities 0.75 and 0.85.
```
apply(linear_reg_bayesian, MARGIN = 2, quantile, probs = c(0.75, 0.85))

(Intercept)      Time number_of_data   sigma2
75%    10.65914 0.9851327       10.66523 51616171
85%    10.77125 1.0065212       10.77664 51754817
```
Visualizing using traceplot which comes in coda package loaded in MCMCpack.
```
traceplot(linear_reg_bayesian[,"Time"], main = "Time Traceplot")
```
![traceplot](https://github.com/dA505819/MCMC_for_forensic_science/blob/master/images/traceplot.png)

Visualizing using denseplot which comes in coda package loaded in MCMCpack
```
densplot(linear_reg_bayesian[,"Time"], main = "Time Densplot")
```
![denseplot](https://github.com/dA505819/MCMC_for_forensic_science/blob/master/images/densplot.png)





