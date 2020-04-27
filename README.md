# 
```
 $ wget https://download.pytorch.org/libtorch/nightly/cpu/libtorch-shared-with-deps-latest.zip
 $ unzip libtorch-shared-with-deps-latest.zip
```

```
 $ mkdir example-app/build
 $ cd example-app/build
 $ cmake -DCMAKE_PREFIX_PATH=`readlink -f ../../libtorch` ..
 $ make
```