# HE-ML

## Homomorphic Encryption in R
Based on http://www.louisaslett.com/EncryptedStats/

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

#### From R install the HomomorphicEncryption package
```r
install.packages("http://www.louisaslett.com/HomomorphicEncryption/dl/HomomorphicEncryption_0.2.tar.gz", repos = NULL, type="source") 
```

#### Finally, from R get the EncryptedStats package
```r
install.packages("http://www.louisaslett.com/EncryptedStats/dl/EncryptedStats_0.5.tar.gz", repos=NULL) 
```

### Test homomorphic encryption

Load the library
```r
library(HomomorphicEncryption)
```
Chose cryptographic backend, e.g.
```r
p <- pars("FandV", lambda=80, L=4)
```
 Generate a keypair
 ```r
 k <- keygen(p)
 k$pk   # public key
 k$sk   # private key
 ```
 Encrypt some data with the public key
 ```r
 ct0 <- enc(k$pk, 2)   # 2
 ct1 <- enc(k$pk, 3)   # 3
 ```
 Add ciphertexts
 ```r
 ct2 <- ct0 + ct1
 ```
 Decrypt with the secret key and check the result!!
 ```r
 > dec(k$sk, ct2) == 5
[1] TRUE
 ```
 Amazing!!
 
 
 






