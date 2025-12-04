# BE-Seminar-demo
Live demonstration of Wakis for the CERN-BE seminar: https://indico.cern.ch/event/1608797/

Involving our in-house python-packages of the impedance assessment workflow: 
* [`Wakis`](https://github.com/ImpedanCEI/wakis) for the 3D electromagnetic wakefield simulations,
* [`IDDEFIX`](https://github.com/ImpedanCEI/IDDEFIX) for partially decayed wake extrapolation,
* [`BIHC`](https://github.com/ImpedanCEI/BIHC) for impedance-induced beam power loss, and

## Getting started
You can install wakis from GitHub to have the latest changes into your conda environment (Python>3.9 and <=3.12):
```bash
pip install wakis['notebook']
pip install bihc neffint iddefix # optional satellite packages
```

### GPU setup

`wakis` uses `cupy` to access GPU acceleration with the supported NVIDIA GPUs (more info in [Cupy's website](https://cupy.dev/)).
Provided a compatible [NVIDIA CUDA GPU](https://developer.nvidia.com/cuda-gpus) or [AMD ROCm GPUs](https://www.amd.com/en/products/graphics/desktops/radeon.html) with the adequate drivers & Toolkit:

```
conda install -c conda-forge cupy cuda-version=11.8
```

### MPI setup
To run multi-CPU parallelized simulations, Wakis needs the following packages:

* OpenMPI installed in your operating system:
* Python package [`mpi4py`](https://mpi4py.readthedocs.io/en/stable/)
* Python package [`ipyparallel`](https://ipyparallel.readthedocs.io/en/latest/tutorial/intro.html) to start a MPI kernel inside notebooks

The preferred install method is through `conda-forge`:

```
conda install -c conda-forge mpi4py openmpi
pip install ipyparallel
```

### Multithreading setup
To achieve multithreading for Wakis `sparse matrix x vector` operations, the Intel-MKL backend has been implemented as an alternative to single-threaded `scipy.sparse.dot`. To install it in your conda environment simply do:

```
conda create --name wakis-env python=3.12 mkl mkl-service
pip install sparse_dot_mkl
```

Wakis will detect that the package is installed and use it as default backend. To control the number of threads and memory pinning, one can add the following lines to your python script **before** the imports:

```python
# [!] Set before importing numpy/scipy/mkl modules
import os
os.environ["OMP_NUM_THREADS"] = str(os.cpu_count())       # Number of OpenMP threads
os.environ["KMP_AFFINITY"] = "balanced,granularity=fine"  # Thread pinning
```