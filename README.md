# Bayesian-Network-HW2

BUAN 6357 – Fall 2017 (Johnston) HW 3 Due 5 Nov 2017 (Sunday) 
 
Problem Description: 
The file “medical_history.Rimg” contains shortened medical condition (synthetic) data on a collection of patients.  The file is available as an R workspace image from directory “C:/data/BUAN6357/hw_3”.  The available fields are H (history of smoking), B (bronchitis is present), L (lung disease is present), F (patient is complaining of fatigue), and C (chest X-ray is positive).  All fields are coded Y or N. 
We want a Bayesian Network which can be used to both identify the conditional dependencies and obtain confidence interval estimates for some of the conditional probabilities.  Use 1 as your RNG seed in a 1000 iteration bootstrap for the following quatities: P(H=”Y” | F=”Y”, C=”Y”), P(H=”Y” | F=”Y”, C=”N”), P(H=”Y” | F=”N”, C=”Y”), P(H=”Y” | F=”N”, C=”N”), P(H=”Y” | F=”Y”), and P(H=”Y” | F=”N”). Use alpha=0.05 . 
 
Deliverables: 
1. Network fitting result   (Name: fit) 2. Bootstrap estimates (6 structures) (Names: pH_Fy_Cy, pH_Fy_Cn, pH_Fn_Cy, pH_Fn_Cn, pH_Fy, and pH_Fn)   
Notes: 
1. Part 1 of the homework is to compute the named deliverables according to the specifications.  Your role in this part is that of “programmer”. 2. Part 2 of the homework is to answer questions about the data and the analysis.  Not all the answers will be immediately available from the deliverables but can be obtained (calculated) from the deliverables.  Your role in this part of the homework is that of “analyst” but you are expected to also be able to extract additional information from the deliverables. 
The following code needs to start your code: 
 setwd(“C:/data/BUAN6357/hw_3”) 
 source(“startHW3.txt”) 
and the following code needs to be the very end of your submission 
 source(“wrapupHW3.txt”) 
Program submissions without these code additions will NOT be considered correct.  These code statements do not need to in your code during development. 



#Part 1

#HW3

setwd("C:/data/BUAN6357/hw_3")  
source("startHW3.txt")

load ("C:/data/BUAN6357/hw_3/medical_history.Rimg") 
#Calling bnlearn package
require(bnlearn)

#getwd()
#dim(medical_history)
#head(medical_history)

set.seed(1)

# build and plot a graph using the hill-climbing algorithm (hc)
struct_hc	<-	hc(medical_history)
#plot(struct_hc)	

# default model with discrete data (character)
fit		<-	bn.fit(struct_hc, medical_history)

set.seed(1)
# bootstrap some ranges
B		<-	1000
pH_Fy_Cy <-	rep(-1,B)
pH_Fy_Cn <-	rep(-1,B)
pH_Fn_Cy <-	rep(-1,B)
pH_Fn_Cn <-	rep(-1,B)
pH_Fy <-	rep(-1,B)
pH_Fn <-	rep(-1,B)


for(i in 1:B) {
    pH_Fy_Cy[i] <- cpquery(fit,(H=="Y"),(F=="Y" & C=="Y"))
    pH_Fy_Cn[i] <- cpquery(fit,(H=="Y"),(F=="Y" & C=="N"))
    pH_Fn_Cy[i] <- cpquery(fit,(H=="Y"),(F=="N" & C=="Y"))
    pH_Fn_Cn[i] <- cpquery(fit,(H=="Y"),(F=="N" & C=="N"))
    pH_Fy[i] <- cpquery(fit,(H=="Y"),(F=="Y"))
    pH_Fn[i] <- cpquery(fit,(H=="Y"),(F=="N"))
  }


#Deliverables
#Network fitting result
fit

#Bootstrap estimates 
pH_Fy_Cy
pH_Fy_Cn
pH_Fn_Cy
pH_Fn_Cn
pH_Fy
pH_Fn


#Part 2
setwd("C:/data/BUAN6357/hw_3")  
source("startHW3.txt")

load ("C:/data/BUAN6357/hw_3/medical_history.Rimg") 
#Calling bnlearn package
require(bnlearn)

#getwd()
#dim(medical_history)
#head(medical_history)

set.seed(1)

# build and plot a graph using the hill-climbing algorithm (hc)
struct_hc	<-	hc(medical_history)
#plot(struct_hc)	

# default model with discrete data (character)
fit		<-	bn.fit(struct_hc, medical_history)

set.seed(1)
# bootstrap some ranges
B		<-	1000
pH_Fy_Cy <-	rep(-1,B)
pH_Fy_Cn <-	rep(-1,B)
pH_Fn_Cy <-	rep(-1,B)
pH_Fn_Cn <-	rep(-1,B)
pH_Fy <-	rep(-1,B)
pH_Fn <-	rep(-1,B)


for(i in 1:B) {
    pH_Fy_Cy[i] <- cpquery(fit,(H=="Y"),(F=="Y" & C=="Y"))
    pH_Fy_Cn[i] <- cpquery(fit,(H=="Y"),(F=="Y" & C=="N"))
    pH_Fn_Cy[i] <- cpquery(fit,(H=="Y"),(F=="N" & C=="Y"))
    pH_Fn_Cn[i] <- cpquery(fit,(H=="Y"),(F=="N" & C=="N"))
    pH_Fy[i] <- cpquery(fit,(H=="Y"),(F=="Y"))
    pH_Fn[i] <- cpquery(fit,(H=="Y"),(F=="N"))
  }


#Deliverables
#Network fitting result
fit

#Bootstrap estimates 
pH_Fy_Cy
pH_Fy_Cn
pH_Fn_Cy
pH_Fn_Cn
pH_Fy
pH_Fn

alpha	<-	0.05
#replace pH_Fy_Cy with the other 5 bootstrap estimates(pH_Fy_Cn,pH_Fn_Cy...) and find the lower bound and
#upper bound values
low	=	quantile(pH_Fy_Cy,   alpha/2) #lower bound value
high	=	quantile(pH_Fy_Cy, 1-alpha/2) #upper bound value


low1	=	quantile(pH_Fy_Cn,   alpha/2) #lower bound value
high1	=	quantile(pH_Fy_Cn, 1-alpha/2) #upper bound value


low2=	quantile(pH_Fn_Cy,   alpha/2) #lower bound value
high2	=	quantile(pH_Fn_Cy, 1-alpha/2) #upper bound value


low3	=	quantile(pH_Fn_Cn,   alpha/2) #lower bound value
high3	=	quantile(pH_Fn_Cn, 1-alpha/2) #upper bound value


low4	=	quantile(pH_Fy,   alpha/2) #lower bound value
high4	=	quantile(pH_Fy, 1-alpha/2) #upper bound value


low5	=	quantile(pH_Fn,   alpha/2) #lower bound value
high5	=	quantile(pH_Fn, 1-alpha/2) #upper bound value
