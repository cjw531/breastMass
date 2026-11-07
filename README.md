# VICTRE breastMass: 3D In Silico Breast Mass Generator
A procedural 3D stochastic breast mass generator developed by the U.S. FDA (DIDSR) under the Virtual Imaging Clinical Trials for Regulatory Evaluation (VICTRE) project. This tool implements the computational model by Sisternes et al. (Med. Phys., 2015) based on Gaussian Random Spheres (GRS) and fractal branching spicules.

> [!NOTE]
> 
> This repository was originally forked from: https://github.com/DIDSR/breastMass 

## Conda Environment Setup

```
# 1. Create and activate environment
conda create -n breastmass python=3.10 -y
conda activate breastmass

# 2. Install C++ build dependencies (VTK, TBB runtime/headers, Boost)
conda install -c conda-forge vtk tbb-devel boost-cpp -y

# 3. Install Python analysis and visualization packages
pip install numpy scipy matplotlib SimpleITK opencv-python

# 4. Install PyTorch with CUDA 12.1 and tqdm
conda install pytorch torchvision torchaudio pytorch-cuda=12.1 tqdm -c pytorch -c nvidia -c conda-forge --solver=libmamba -y
conda install -c conda-forge tqdm -y

# 5. Set persistent runtime library path for VTK and reload environment
conda env config vars set LD_LIBRARY_PATH=$CONDA_PREFIX/lib
conda deactivate
conda activate breastmass
```

## Build & Compilation

Compile using CMake, ensuring the build system prioritizes the Conda-installed Boost and VTK binaries over system packages:

```
cd breastMass
mkdir -p build && cd build

# Configure CMake with Conda environment paths
cmake -DBOOST_ROOT=$CONDA_PREFIX -DBoost_NO_SYSTEM_PATHS=ON ..

# Build binary
make -j$(nproc)
```

Verify compilation:

```
./breastMass --help
```

## Generate Sample 3D Signal

### Execution

```
./build/breastMass --config sample_mass.cfg
```

### Output
The software calculates maximum radial extent ($r_{\text{max}}$) dynamically and sizes the output volume dimension accordingly: $\text{massDim} = \lceil 4 \cdot r_{\text{max}} / \text{imgRes} \rceil$.

- `mass_<seed>.cfg`: Copy of the parameters used during simulation.
- `mass_<seed>.vti`: VTK XML 3D image format for visualization in ParaView.
- `mass_<seed>_<massDim>.raw`: Flat binary 3D array (`uint8` / `unsigned char`).
    - Shape: (`massDim`, `massDim`, `massDim`)
    - Values: `0` (Background tissue), `1` (Lesion core and spicules).

## Visualization


<hr/>

> [!CAUTION] 
> 
> Below is the README.md from the original repository.

# breastMass

> [!WARNING] 
> 
> This repository has been archived and is no longer maintained.
> The code is provided for historical reference and may contain unpatched or unknown vulnerabilities.
> It should not be used in production systems.

Breast Mass Generation Software

**Documentation and installation instructions are available here:**
[breastMass.readthedocs.io](https://breastMass.readthedocs.io).

The project is in the public domain within the United States, and
copyright and related rights in the work worldwide are waived through
the CC0 1.0 Universal public domain dedication. See the COPYING.txt
file in this repository to view the entire CC0 dedication.

All contributions to this project will be released under the CC0
dedication. By submitting a pull request, you are agreeing to comply
with this waiver of copyright interest.

## Disclaimer

This software and documentation (the "Software") were developed at the
Food and Drug Administration (FDA) by employees of the Federal Government
in the course of their official duties. Pursuant to Title 17, Section 105
of the United States Code, this work is not subject to copyright protection
and is in the public domain. Permission is hereby granted, free of charge,
to any person obtaining a copy of the Software, to deal in the Software
without restriction, including without limitation the rights to use, copy,
modify, merge, publish, distribute, sublicense, or sell copies of the
Software or derivatives, and to permit persons to whom the Software is
furnished to do so. FDA assumes no responsibility whatsoever for use by
other parties of the Software, its source code, documentation or compiled
executables, and makes no guarantees, expressed or implied, about its
quality, reliability, or any other characteristic. Further, use of this
code in no way implies endorsement by the FDA or confers any advantage in
regulatory decisions. Although this software can be redistributed and/or
modified freely, we ask that any derivative works bear some notice that
they are derived from it, and any modified versions bear some notice that
they have been modified.
