# HEML
[Paper](https://eprint.iacr.org/2018/254)

[github repo](https://github.com/kimandrik/HEML)

## Conclusion

## Installation
Install dependencies
```console
$ sudo apt install libntl*
$ sudo apt install libpthread-*
```

Get, install and test the HEAAN library
```console
$ mkdir HE
$ cd HE
$ git clone https://github.com/kimandrik/HEAAN.git
$ cd HEAAN/HEAAN/lib
$ make all
# uncomment the tests in src/HEAAN.cpp
$ cd ../run
$ make all
$ ./HEAAN
```
Test seem to complete ok

Get, install and test the HEAANBOOT library
```console
$ cd ../..
$ git clone https://github.com/kimandrik/HEAANBOOT.git
$ cd HEAANBOOT/HEAANBOOT/lib
$ make all
# uncomment the tests in src/HEAAN.cpp
$ cd ../run
$ make all
$ ./HEAAN
```
Test seem to complete ok


Get the HEML library
```console
$ cd ../../..
$ git clone https://github.com/kimandrik/HEML.git
```
Edit a makefile (in spite of having "Automatically-generated file. Do not edit!" on the 2nd line)
Replace line 47 in run/makefile
```make
g++ -L/Users/Han/Documents/Git/Programming/HEAANBOOT/HEAANBOOT/lib -L/Users/kimandrik/git/HEAANBOOT/HEAANBOOT/Debug -L/usr/local/lib -pthread -o "HEML" $(OBJS) $(USER_OBJS) $(LIBS)
```

with
```make
g++ -L../../../HEAANBOOT/HEAANBOOT/lib -L../../../HEAANBOOT/HEAANBOOT/Debug -L/usr/local/lib -pthread -o "HEML" $(OBJS) $(USER_OBJS) $(LIBS)
```

and similarly line 31 in run/src/subdir.mk
```make
g++ -I/Users/Han/Documents/Git/Programming/HEAANBOOT/HEAANBOOT/src -I/usr/local/include -I/Users/kimandrik/git/HEAANBOOT/HEAANBOOT/src -O3 -pthread -c -std=c++11 -MMD -MP -MF"$(@:%.o=%.d)" -MT"$(@)" -o "$@" "$<"
```

with
```
g++ -I../../../HEAANBOOT/HEAANBOOT/src -I/usr/local/include -I../../../HEAANBOOT/HEAANBOOT/src -O3 -pthread -c -std=c++11 -MMD -MP -MF"$(@:%.o=%.d)" -MT"$(@)" -o "$@" "$<"
```
        
Compile
```console
$ cd HEML/HEML/lib
$ make all
$ cd ../run
$ make all
```

## Testing 
see ```HEML/HEML/src/HEML.cpp``` for details

test 1
```console
$ ./HEML "../data/data103x1579.txt" 1 7 5 1 -1 1 5 1
```

test 2
```console
# Ciphertext
$ ./HEML "../data/1_training_data_csv" 1 7 5 1 -1 1 1 1 "../data/1_testing_data_csv"
# Plaintext
$ ./HEML "../data/1_training_data_csv" 1 7 5 1 -1 1 0 1 "../data/1_testing_data_csv"
```
both fails:
``RR: conversion of a non-finite double
  Aborted (core dumped)
  ```







