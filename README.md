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
dec(k$sk, ct2) == 5
[1] TRUE
 ```
 **Splendid**
 
 
 ### Test EncryptedStats
 
 #### Load the EncryptedStats package
 ```r
 > library(EncryptedStats)
 ```
 
#### Load some data
```r
data(beavers)

head(beaver1)
  day time  temp activ
1 346  840 36.33     0
2 346  850 36.34     0
3 346  900 36.35     0
4 346  910 36.42     0
5 346  920 36.55     0
6 346  930 36.69     0

# From dataframe to matrix
X <- data.matrix(beaver1[1:3])
y <- data.matrix(beaver1[4])
```

#### Encode
```r
> cx = enc(k$pk, X)
Error in enc.Rcpp_FandV_pk(k$pk, X) : Only integers can be encrypted.
```
**Ooops**

Everything to integer
```r
X[, 3] <- as.integer(X[, 3] * 100
```

```r
> head(X)
       day time   temp
[1,]   346  840 363300
[2,]   346  850 363400
[3,] 34600  900 363500
[4,]   346  910 364200
[5,]   346  920 365400
[6,]   346  930 366900

```
Encode
```r
cx = enc(k$pk, X)
cy = enc(k$pk, y)
```




 
 
 
 
 
 






