# PolyBin3D
PolyBin3D is a Python code that estimates the binned power spectrum, bispectrum, and trispectrum for 3D fields such as the distributions of matter and galaxies, using the algorithms of [Philcox 2020](https://arxiv.org/abs/2012.09389), [Philcox 2021](https://arxiv.org/abs/2107.06287) and [Ivanov et al. 2023](https://arxiv.org/abs/2302.04414). It is a sister code to [PolyBin](https://github.com/oliverphilcox/PolyBin), which computes the polyspectra of data on the two-sphere and is a modern reimplementation of the former [Spectra-Without-Windows](https://github.com/oliverphilcox/Spectra-Without-Windows) code. 

For each statistic, two estimators are available: the standard (ideal) estimators, which do not take into account the mask, and window-deconvolved estimators. In the second case, we require computation of a Fisher matrix; this depends on binning and the mask, but does not need to be recomputed for each new simulation. 

PolyBin contains the following modules:
- `pspec`: Binned power spectra
- `bspec`: Binned bispectra

The basic usage is the following:
```
# Import code
import PolyBin3D as pb
import numpy as np

# Load base class
base = pb.PolyBin3D(boxsize, # dimensions of box 
                    gridsize, # dimensions of Fourier-space grid, 
                    boxcenter=[0,0,0], # center of simulation box
                    pixel_window='tsc', # pixel window function
                    sightline='global') # redshift-space axis                    

# Load power spectrum class
pspec = pb.PSpec(base, 
                 k_bins, # k-bin edges
                 lmax, # Legendre multipoles
                 mask, # real-space mask
                 applySinv, # filter to apply to data
                )

# Compute Fisher matrix and shot-noise using Monte Carlo simulations (should usually be parallelized)
fish, shot_num = pspec.compute_fisher(10, N_cpus=1, verb=True)

# Compute windowed power spectra
Pk_ideal = pspec.Pk_ideal(data) 

# Compute power spectrum of the data
Pk_unwindowed = pspec.Pk_unwindowed(data, fish=fish, shot_num=shot_num, subtract_shotnoise=False)
```

Further details are described in the tutorials, which descibe
1. [Introduction](Tutorial%201%20-%20Pk%20from%20Simulations%20.ipynb) to PolyBin3D, and computing the power spectrum from simulations
2. [Validation](Tutorial%202%20-%20Validating%20the%20Unwindowed%20Estimators.ipynb) of the window-deconvolved power spectrum estimators on simulations
3. [Application](Tutorial%203%20-%20BOSS%20Pk%20Multipoles.ipynb) of the power spectrum esitmators to the BOSS DR12 dataset.


## Authors
- [Oliver Philcox](mailto:ohep2@cantab.ac.uk) (Columbia / Simons Foundation)


## Dependencies
- Python 2/3
- numpy, scipy
- fftw [for FFTs]
- Nbodykit [not required, but useful for testing]


## References
**Code references:**
1. Philcox, O. H. E., "Cosmology Without Windows: Quadratic Estimators for the Galaxy Power Spectrum", (2020) ([arXiv](https://arxiv.org/abs/2012.09389))
2. Philcox, O. H. E., "Cosmology Without Window Functions: Cubic Estimators for the Galaxy Bispectrum", (2021) ([arXiv](https://arxiv.org/abs/2107.06287))
3. Ivanov, M. M., Philcox, O. H. E., et al. "Cosmology with the Galaxy Bispectrum Multipoles: Optimal Estimation and Application to BOSS Data" (2023) ([arXiv](https://arxiv.org/abs/2302.04414))

**Some works using data from PolyBin3D (or its predecessor)**
- Philcox & Ivanov (2021, [Phys. Rev. D](https://doi.org/10.1103/PhysRevD.105.043517), [arXiv](https://arxiv.org/abs/2112.04515)): Combined constraints on LambdaCDM from the BOSS power spectrum and bispectrum.
- Cabass et al. (2022, [arXiv](https://arxiv.org/abs/2201.07238)): Constraints on single-field inflation from the BOSS power spectrum and bispectrum.
- Cabass et al. (2022, [arXiv](https://arxiv.org/abs/2204.01781)): Constraints on multi-field inflation from the BOSS power spectrum and bispectrum.
- Nunes et al. (2022, [arXiv](https://arxiv.org/abs/2203.08093)): Constraints on dark-sector interactions from the BOSS galaxy power spectrum.
- Rogers et al. (2023, [arXiv](https://arxiv.org/abs/2301.08361)): Ultra-light axions and the S8 tension: joint constraints from the cosmic microwave background and galaxy clustering.
- Ivanov et al. (2023, [arXiv](https://arxiv.org/abs/2302.04414)): Cosmology with the Galaxy Bispectrum Multipoles: Optimal Estimation and Application to BOSS Data.
- Moretti et al. (2023, [arXiv](https://arxiv.org/abs/2306.09275)): Constraints on the growth index and neutrino mass from the BOSS power spectrum.
- He et al. (2023, [arXiv](https://arxiv.org/abs/2309.03956)): Self-Interacting Neutrinos in Light of Large-Scale Structure Data.
- Camarena et al. (2023, [arXiv](https://arxiv.org/abs/2309.03941)): The two-mode puzzle: Confronting self-interacting neutrinos with the full shape of the galaxy power spectrum 
