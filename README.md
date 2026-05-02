# Single-file SciPy-based CCN activation model

## notes for users

To install the package, try: `pip install git+https://github.com/open-atmos-krk/ccnact.git`

A hello-world run:
```python
from ccnact import parcel
n_act, s_max = parcel(...)
```

## notes for developers

To execute the tests: `pip install -e .[dev]; pytest ccnact.py` 

To set-up **pre-commit**: `pip install pre-commit; pre-commit install`
