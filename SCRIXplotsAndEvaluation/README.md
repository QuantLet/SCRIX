
[<img src="https://github.com/QuantLet/Styleguide-and-FAQ/blob/master/pictures/banner.png" width="880" alt="Visit QuantNet">](http://quantlet.de/index.php?p=info)

## [<img src="https://github.com/QuantLet/Styleguide-and-Validation-procedure/blob/master/pictures/qloqo.png" alt="Visit QuantNet">](http://quantlet.de/) **SCRIXplotsAndEvaluation** [<img src="https://github.com/QuantLet/Styleguide-and-Validation-procedure/blob/master/pictures/QN2.png" width="60" alt="Visit QuantNet 2.0">](http://quantlet.de/d3/ia)

```yaml

Name of Quantlet : SCRIXplotsAndEvaluation

Published in : Unpublished; SCRIX

Description : Generate SCRIX

Keywords : CRIX, SCRIX, index, cryptocurrency, crypto, plot

See also : SCRIXgeneration

Author : Lam Tuyen Nguyen, Wee Song Chua, Simon Trimborn

Submitted : Wed, Aug 17 2016 by Wee Song Chua

Datafile : 'crix.RData, scrix_CFes_return.RData, scrix_CFVaR_return.RData, scrix_equalweight.RData,
scrix_es_return.RData, scrix_markport.RData, scrix_SR_CFes_return.RData,
scrix_SR_CFVaR_return.RData, scrix_sr_es.RData, scrix_sr_var.RData, scrix_var_return.RData'

Example : Plot with the SCRIX family

```

![Picture1](SCRIXplot.png)


### R Code:
```r
## set up basics
# clear workspace
rm(list=ls(all=TRUE))

# set working directory
setwd("C:/Users/user/Desktop/Q Kolleg/smart beta/SmartIndexData/")

# load data
load("crix.RData")
load("scrix_equalweight.RData")
load("scrix_es_return.RData")
load("scrix_sr_es.RData")
load("scrix_sr_var.RData")
load("scrix_var_return.RData")
load("scrix_markport.RData")
load("scrix_CFes_return.RData")
load("scrix_CFVaR_return.RData")
load("scrix_SR_CFes_return.RData")
load("scrix_SR_CFVaR_return.RData")

#all data
alldata = c(crix,scrix_equalweight,scrix_es_return,
             scrix_sr_es,scrix_sr_var, scrix_var_return,
             scrix_markport, scrix_CFes_return, 
             scrix_CFVaR_return, scrix_SR_CFes_return,
             scrix_SR_CFVaR_return)

# set axis and limits 
dates    = seq(as.Date("01/08/2014", format = "%d/%m/%Y"),
               by = "days", length = length(crix) )
op       = par(mar = c(7,4,4,2) + 0.1) ## more space for the labels
lb       = min(alldata) 
ub       = max(alldata) 

# plots
plot(crix, type = "l", col = "black", xaxt = "n", lwd = 1, xlab = "Date", 
     ylab = "Performance of SCRIX",ylim=c(lb, ub))
axis(1, at = c(1,93,185,274,366,458,550), label = names(crix)[c(1,93,185,274,366,458,550)])
lines(scrix_equalweight, type='l',col="red", lwd=1)
#lines(scrix_es, type='l',col="forestgreen", lwd=1)
#lines(scrix_var, type='l',col="blue", lwd=1)
lines(scrix_var_return, type='l',col="blue", lwd=1)
lines(scrix_es_return, type='l',col="forestgreen", lwd=1)
# lines(scrix_sr_var, type='l',col="purple", lwd=1)
# lines(scrix_sr_es, type='l',col="orange", lwd=1)
lines(scrix_markport, type='l',col="chocolate", lwd=1)
par(op) ## reset margin

# ############ compare VaR ES vs CF VaR ES ###################
# lb2       = min(crix,scrix_var_return,scrix_es_return,scrix_CFVaR_return,scrix_CFes_return)
# ub2       = max(crix,scrix_var_return,scrix_es_return,scrix_CFVaR_return,scrix_CFes_return)
# plot(crix, type = "l", col = "black", xaxt = "n", lwd = 1, xlab = "Date", 
#      ylab = "Performance of SCRIX",ylim=c(lb2, ub2))
# axis(1, at = c(1,93,185,274,366,458,550), label = names(crix)[c(1,93,185,274,366,458,550)])
# lines(scrix_var_return, type='l',col="blue", lwd=1)
# lines(scrix_es_return, type='l',col="forestgreen", lwd=1)
# lines(scrix_CFVaR_return, type='l',col="purple", lwd=1)
# lines(scrix_CFes_return, type='l',col="orange", lwd=1)

# ### SR ###
# lb3       = min(crix,scrix_sr_var,scrix_sr_es,scrix_SR_CFes_return,scrix_SR_CFVaR_return)
# ub3       = max(crix,scrix_sr_var,scrix_sr_es,scrix_SR_CFes_return,scrix_SR_CFVaR_return)
# plot(crix, type = "l", col = "black", xaxt = "n", lwd = 1, xlab = "Date", 
#      ylab = "Performance of SCRIX",ylim=c(lb3, ub3))
# axis(1, at = c(1,93,185,274,366,458,550), label = names(crix)[c(1,93,185,274,366,458,550)])
# lines(scrix_sr_var, type='l',col="blue", lwd=1)
# lines(scrix_sr_es, type='l',col="forestgreen", lwd=1)
# lines(scrix_SR_CFVaR_return, type='l',col="purple", lwd=1)
# lines(scrix_SR_CFes_return, type='l',col="orange", lwd=1)

#################### loss comparison ii: MSE, MDA #######################
scrixdata       = list()
scrixdata[[1]]  = scrix_equalweight
scrixdata[[2]]  = scrix_var_return
scrixdata[[3]]  = scrix_es_return
scrixdata[[4]]  = scrix_CFVaR_return
scrixdata[[5]]  = scrix_CFes_return
scrixdata[[6]]  = scrix_sr_var
scrixdata[[7]]  = scrix_sr_es
scrixdata[[8]]  = scrix_SR_CFVaR_return
scrixdata[[9]]  = scrix_SR_CFes_return
scrixdata[[10]] = scrix_markport

## declare variables to store MSE, MDA, MA
mse     = numeric(length(scrixdata))
mda     = numeric(length(scrixdata))
meanAdv = numeric(length(scrixdata))
for (i in 1:length(scrixdata)){
  mse[i] = mean((scrixdata[[i]] - crix)^2)
  mda[i] = mean(sign(diff(crix)) == sign(diff(scrixdata[[i]])))
  meanAdv[i] = sum(scrixdata[[i]]-crix)/length(crix)
}
names(mse)=c("Equal","VaR","ES","CF-VaR","CF-ES","SR-VaR","SR-ES",
             "SR-CF-VaR","SR-CF-ES","MarkPort")
names(mda)=c("Equal","VaR","ES","CF-VaR","CF-ES","SR-VaR","SR-ES",
             "SR-CF-VaR","SR-CF-ES","MarkPort")
names(meanAdv)=c("Equal","VaR","ES","CF-VaR","CF-ES","SR-VaR","SR-ES",
                 "SR-CF-VaR","SR-CF-ES","MarkPort")
```
