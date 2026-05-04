# ☁️  [KISS](https://en.wikipedia.org/wiki/KISS_principle) SciPy-based [CCN](https://en.wikipedia.org/wiki/Cloud_condensation_nuclei) activation model

[![PyPI version](https://badge.fury.io/py/ccnact.svg)](https://pypi.org/project/ccnact)

## 📌 overview

- 🧮 integration using [SciPy interface to LSODA ODE solver](https://docs.scipy.org/doc/scipy/reference/generated/scipy.integrate.LSODA.html)
- 📝 ODE system based on [Arabas & Shima 2017](https://doi.org/10.5194/npg-24-535-2017) (extended to polydisperse aerosol size spectrum)
- 🌪️ capable of resolving aerosol activation, deactivation, drop growth, evaporation and ripening
- ⚙️ single-function interface allowing to modify all constants, and returning a tuple of:
  - concentration of activated droplets (at STP) & 
  - maximal supersaturation
- 📈 mulit-modal lognormal spectrum specification (with concentration interpretted as at STP)
- ⚖️  implemeted using [Pint](https://pint.readthedocs.io/) dimensional analysis (physical units consistency checks) enabled for tests only
- 🔗 KISS design: SciPy, NumPy & Pint are the only dependencies, model+tests in a single (and short) .py file
- 🚀 subsecond execution times for common parameter settings

## 💻 notes for users

To install the package, try: `pip install git+https://github.com/open-atmos-krk/ccnact.git`

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

To execute the tests: `pip install -e .[dev]; pytest ccnact.py` 

To set-up [pre-commit](https://pre-commit.com/): `pip install pre-commit; pre-commit install`
