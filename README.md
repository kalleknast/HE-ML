# HE-ML

## Homomorphic Encryption in R

### Install the HomomorphicEncryption R Package 

#### General libraries
```
sudo apt-get install libgmp-dev libflint-dev libmpfr-dev
```
#### Get R
```
sudo apt install r-base-core r-base-dev
```

#### Start R and install some packages
```r
install.packages(c("Rcpp", "RcppParallel", "gmp")) 
```

#### Finally install the HomomorphicEncryption package
```r
install.packages("http://www.louisaslett.com/HomomorphicEncryption/dl/HomomorphicEncryption_0.2.tar.gz", repos = NULL, type="source") 
```




