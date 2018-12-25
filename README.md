#Assignment-3
#Intermediate R Review
#Dataset:National Alzheimer Coordination Center 
#BS 803 Statistical Programming for Biostatisticians

#1) Read in SAS Dataset Using ‘sas7bdat’ Package
```{r}
library(sas7bdat)

  hw3data <- read.sas7bdat("C:/Users/miche/Documents/exercise1.sas7bdat")
```

#2) Create Normal Dataset; NACCUDSD = 1

#Note [] extracts a list; [[]] extracts 'element(s)' within the list. 

```{r}
normal <- hw3data[which(hw3data$NACCUDSD==1),]
```

#3) Drop Variables: NACCUDSD & naccyrs

```{r}
vars <- names(normal) %in% c("NACCUDSD", "naccyrs")
normal <- normal[!vars]
names(normal)
## [1] "NACCID"   "vnumber"  "SEX"      "EDUC"     "NACCAGEB" "NACCZMMS"
## [7] "NACCZLMI" "NACCZLMD" "NACCZDFT"
```

#4) Recode Missing Values with NA in Neuropsychological Scores

```{r}
vars2 <- names(normal) %in% c("NACCZMMS","NACCZLMI", "NACCZLMD", "NACCZDFT")
normal[, vars2][normal[,vars2] == 99 | normal[, vars2] == -99] <- NA
```

#5) New Variable as mean of the neuro scores; Cognition

```{r}
normal$Cognition<- apply(normal[, vars2], 1, mean, na.rm = T)
```

#6) Transpose Dataset from Long to Wide

```{r}
dim(normal) 

normal.sort <- normal[order(normal$vnumber),]

normal_wide <- reshape(normal.sort, 
             timevar = "vnumber",
             idvar = c("NACCID", "SEX", "EDUC", "NACCAGEB"),
             direction = "wide")
dim(normal_wide) 

normal_wide.sort <- normal_wide[order(normal_wide$NACCID),]
```


