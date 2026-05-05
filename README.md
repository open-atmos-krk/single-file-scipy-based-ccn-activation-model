# ☁️  [KISS](https://en.wikipedia.org/wiki/KISS_principle) SciPy-based [CCN](https://en.wikipedia.org/wiki/Cloud_condensation_nuclei) activation model

[![PyPI version](https://badge.fury.io/py/ccnact.svg)](https://pypi.org/project/ccnact)

## 📌 overview

`ccnact` is a simple, yet complete, adiabatic/hydrostatic air-parcel framework employing
moving-sectional/particle-resolved aerosol-cloud microphysics, featuring:

- 🧮 integration using [SciPy's interface to LSODA](https://docs.scipy.org/doc/scipy/reference/generated/scipy.integrate.LSODA.html) stiff ODE solver
- 📝 ODE system based on [Arabas & Shima 2017](https://doi.org/10.5194/npg-24-535-2017) (extended to polydisperse aerosol size spectrum)
- 🏁 [κ-Köhler](https://doi.org/10.5194/acp-7-1961-2007) wet radii equilibration of input dry-size spectrum with [SciPy's elementwise root finder](https://docs.scipy.org/doc/scipy/reference/optimize.elementwise.html)
- 🌪️ capability of resolving aerosol activation, deactivation, drop growth, evaporation and ripening
- 📈 mulit-modal lognormal (using [SciPy's stats routines](https://docs.scipy.org/doc/scipy/reference/generated/scipy.stats.lognorm.html)) spectrum specification (concentration at STP)
- 🔌 portable across platforms and architectures (CI tested on Linux, macOS & Windows, on Intel and ARM CPUs)
- ⚙️ single-function interface allowing to modify every single constant, and returning a tuple of:
  - concentration of activated droplets (at STP)
  - maximal supersaturation
- 🧩 effective interfacing options for [Matlab](https://www.mathworks.com/help/matlab/call-python-libraries.html), [IDL](https://www.nv5geospatialsoftware.com/docs/Python.html), [Julia](https://github.com/JuliaPy/PythonCall.jl), etc 
- ⚖️ unit-aware implemetation using [Pint](https://pint.readthedocs.io/) (dimensional analysis enabled for tests only)
- 🚀 subsecond execution times for common parameter settings
- 🔗 [KISS design](https://en.wikipedia.org/wiki/KISS_principle): depends on [SciPy](https://scipy.org/), [NumPy](https://numpy.org/) & [Pint](https://pint.readthedocs.io/) only; single ~500 LOC file (physics + setup + tests)

The last five points were the key motivating factors for the development -
  the project originated from a search for a simple, lightweight (in dependencies) and fast 
  CCN activation air-parcel model with concise code, automated testing and no hardcoded constants.

## 💻 notes for users

To install the package, try: `pip install ccnact`

Using from Python:
```python
from ccnact import parcel
help(parcel)
n_act, s_max = parcel(...)
```

Interfacing from Matlab (using the [built-in Python bridge](https://www.mathworks.com/help/matlab/call-python-libraries.html)):
```matlab
ccnact = py.importlib.import_module('ccnact');
ccnact.parcel(pyargs(...
   'MAC', 1,...
   'n_bins', int32(100),...
   'p', 101300,...
   'T', 300,...
   'RH', .99,...
   'dt', 1,...
   'nt', int32(100),...
   'w', 2,...
   'sigma', 0.072,...
   'kappa', py.tuple({1}),...
   'meanr', py.tuple({3e-8}),...
   'gstdv', py.tuple({1.5}),...
   'n_tot', py.tuple({1e9})...
))
```

## ⚙ notes for developers

To execute the tests after checking out from git: `pip install -e .[dev]; pytest ccnact.py` 

To set-up [pre-commit](https://pre-commit.com/): `pip install pre-commit; pre-commit install`
