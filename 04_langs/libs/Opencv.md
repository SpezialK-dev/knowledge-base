#c 
# Opencv

Problems under arch linux of not finding the libs. 

the following thing was done in a root shell
```shell
 ln -s /usr/include/opencv4/opencv2/ /usr/local/include/opencv2 
```
we simply need to link relink this maybe. After unlinking and relinking that might fix it 

creating a makefile requires the following changes 

This [Stackoverflow](https://stackoverflow.com/questions/23162399/linking-opencv-libraries-with-g) tells us what we need 

```makefile

compile:
	 g++ -o <programm>-Wall -g ./src/<your sourcecode>.cpp -I `pkgconf --libs --cflags opencv4`
```
with the last part beeing needed to compile 

### encoutering the following error 


```shell
 make
g++ -o <programm> -Wall -g ./src/main.cpp -I `pkgconf --libs --cflags opencv4`
/usr/bin/ld: warning: libhdf5.so.310, needed by /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so, not found (try using -rpath or -rpath-link)
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Dread'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Fcreate'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5T_NATIVE_UCHAR_g'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Tget_size'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5check_version'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Tget_array_dims2'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Tclose'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Dopen2'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5open'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Sset_extent_simple'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5T_NATIVE_INT32_g'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5T_NATIVE_SCHAR_g'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Screate_simple'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Aget_type'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Tcreate'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Awrite'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Fclose'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Tset_strpad'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Gclose'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Aopen'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Tget_class'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Gcreate2'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Aread'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Dclose'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Pset_deflate'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Adelete'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5P_CLS_DATASET_CREATE_ID_g'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Acreate2'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Dget_create_plist'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Aopen_name'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Eset_auto2'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Sselect_hyperslab'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Pset_chunk'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Pget_layout'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5P_CLS_LINK_ACCESS_ID_g'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Pget_chunk'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Tinsert'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Tequal'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Dget_space'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Aget_space'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Fis_hdf5'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Dget_type'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5T_NATIVE_FLOAT_g'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5T_NATIVE_USHORT_g'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Pcreate'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Eget_auto2'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Aclose'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Tset_size'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Tarray_create2'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Pclose'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Sget_simple_extent_ndims'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Screate'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Dwrite'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Dextend'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Tget_super'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Tget_native_type'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Lexists'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5T_NATIVE_INT_g'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5T_C_S1_g'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Tcopy'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Sclose'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5T_NATIVE_SHORT_g'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5T_NATIVE_DOUBLE_g'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Sget_simple_extent_dims'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Dcreate2'
/usr/bin/ld: /usr/lib/gcc/x86_64-pc-linux-gnu/14.2.1/../../../../lib/libopencv_hdf.so: undefined reference to `H5Fopen'
collect2: error: ld returned 1 exit status
make: *** [makefile:4: compile] Error 1
```

this indicates that [some packages](https://bbs.archlinux.org/viewtopic.php?id=287849) are missing in my case 

so I am installing 

```shell
yay -S hdf5-1.14.5-1
```

