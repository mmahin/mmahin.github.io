## UserWarning: h5py is running against HDF5 1.10.5 when it was built against 1.10.4, this may cause problems

### When occours:
  Generally occours after installing tensorflow.

### Reason:
  When hdf5 version in the conda environment do not match hdf5 in the C++ environment. For example, conda has hdf5 version 1.10.4, but C++ environment has 1.10.5 in this case.</br>
  In this case, When h5py is built within the conda env, it is built against the hdf5 1.10.4 in conda env, then when keras call the underlying tensorflow, which is based on 
  c++ environment of hdf5 1.10.4 so the conflicts exist.
  
### Solutions:

#### Either:

Remove the part after "/" sign or within ().

```
conda remove tensorflow/ keras 
conda install --force hdf5=1.10.4
conda install tensorflow/keras
```

#### Or:

```
conda remove tensorflow / keras  (or pip uninstall tensorflow)
conda remove h5py (or pip uninstall h5py)
conda install h5py (or pip install h5py)
```
