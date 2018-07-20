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
 
#### Load and prepare some data
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
y <- c(data.matrix(beaver1[4]))
```
Try and fail to encode
```r
> cx = enc(k$pk, X)
Error in enc.Rcpp_FandV_pk(k$pk, X) : Only integers can be encrypted.
```
**Ooops**

Everything to integer
```r
X[, 3] <- as.integer(X[, 3] * 100)
```

```r
> head(X)
     day time temp
[1,] 346  840 3633
[2,] 346  850 3634
[3,] 346  900 3635
[4,] 346  910 3642
[5,] 346  920 3654
[6,] 346  930 3669
```

Split into training and test sets
```r
n <- dim(X)[1]
b <- logical(n)
b[sample(n, 30)] = TRUE
X_test <- X[b, ]
y_test <- y[b]
X_train <- X[!b, ]
y_train <- y[!b]
```


Encode
```r
cX_train = enc(k$pk, X_train)
cy_train = enc(k$pk, y_train)
cX_test = enc(k$pk, X_test)
cy_test = enc(k$pk, y_test)
```

#### Fit encrpyted data
```r
snb1.fit <- SNB.fit(X_train, y_train, paired=T, pk=k$pk)    # fit with plain texts
snb2.fit <- SNB.fit(cX_train, cy_train, paired=T, pk=k$pk)  # fit with cihper texts
```
Check the models
```r
> snb1.fit
$counts.y
[1] 108   6

$coeffs
   [,1]         [,2]       [,3]
a 29490 -13047173000 -374538600
b   -96      1289520      99264
d  2093   6345608900    4831740

attr(,"class")
[1] "SNB"
attr(,"pairing")
[1] "paired"
attr(,"crypt")
[1] "unencrypted"


> snb2.fit
$counts.y
Vector of 2 Fan and Vercauteren cipher texts

$coeffs
Matrix of 3 x 3 Fan and Vercauteren cipher texts

attr(,"class")
[1] "SNB"
attr(,"pairing")
[1] "paired"
attr(,"crypt")
[1] "encrypted"

```


#### Predict

Problems
```r
> snb2.predProb <- predict(model=snb2.fit, newX=cX_test, type="prob", sk=k$sk)
Error in base::colSums(x, na.rm = na.rm, dims = dims, ...) : 
  'x' must be an array of at least two dimensions
```
However plain text works
```r
> snb1.predProb <- predict(model=snb1.fit, newX=X_test, type="prob", sk=k$sk)
> snb1.predProb
 [1] 0.5766593 0.6816162 0.6988766 0.7345798 0.6784009 0.7105245 0.7534201
 [8] 0.7609618 0.7438212 0.7152311 0.6883512 0.6653319 0.7185305 0.7239851
[15] 0.7371426 0.7575045 0.7606394 0.7885784 0.7736251 0.7595293 0.7448454
[22] 0.7432697 0.8757474 0.8204972 0.8302251 0.8207843 0.7259565 0.7086159
[29] 0.6920834 0.7374551
attr(,"class")
[1] "SNB.predProb"
```


More problems
```r
snb2.predRaw <- predict(model=snb2.fit, newX=cX_test, type="raw", sk=k$sk)   # runs without error
```
The output is gibberish
```r
> snb2.predRaw
$counts.y
[1] 81  3

$coeffs.e
Big Integer ('bigz') 30 x 3 matrix:
      [,1]                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   
 [1,] -8585653106385629922581737219154971685907543448890229398864090778603025497168545072223269599612436893174121280883042548275512947525720704913073491815303635586233491632525864972821052794951708231525617283442066715884668880416311846770244943752732352003719375787903642193894932952304502517888156498949304984747875527837148745431199437564090611277031502164592274217042495020881558402495728896819168449075931305169035360915803931773955563209290044352252105403854582712488440839017053056568090013728804337987129104175451511216658728218667899030072840435073538356275889352903092960614130672020287159787613518965487940287060917346398682439211217859976646424857617306028179507078999207157998629976173853625830595613797047250748494054997144979581291535976881892183808961393859964815177699258103546662936715205683725366430404682482297662229848253209754196345060183125510402279702929681962538583147212667910526276646370324612158841576302113661972895394650335762863361456530070368879075730181682467789435795866498710443114698882181173176725906631812633998693286458342827050579798182687886137610844536198749611084358214554563674351727407571845262814503995812794544598440428076732989787400677848128785118412508615346785178342579593957998122658406359936 
...

$coeffs.d
Big Integer ('bigz') object of length 3:
[1] 155714771   3707377100  38640291163

attr(,"class")
[1] "SNB.predRaw"

```
