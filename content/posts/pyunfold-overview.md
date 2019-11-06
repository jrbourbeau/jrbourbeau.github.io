---
title: "PyUnfold: A Python package for iterative unfolding"
date: 2018-07-15
---

Recently Zigfried Hampel-Arias and I have been working on an open-source Python
package for implementing iterative unfolding. The package, [PyUnfold](https://github.com/jrbourbeau/pyunfold), was recently published in the [Journal of Open Source Software](https://joss.theoj.org/) (JOSS) and is meant to
bring iterative unfolding to the Python ecosystem while providing users a straightforward yet extensible API. Some relevant links:

- GitHub repo: [https://github.com/jrbourbeau/pyunfold](https://github.com/jrbourbeau/pyunfold)
- Documentation: [https://jrbourbeau.github.io/pyunfold](https://jrbourbeau.github.io/pyunfold)
- JOSS publication: [http://joss.theoj.org/papers/10.21105/joss.00741](http://joss.theoj.org/papers/10.21105/joss.00741)


### What is unfolding?

In an ideal world, experimentalists would have access to the perfect detector: an apparatus
that makes no error in measuring a desired quantity. However, in real life scenarios,
detectors have:

- Finite resolutions
- Characteristic biases that cannot be eliminated
- Less than 100% detection efficiencies
- Statistical uncertainties

By building a matrix (called the response matrix) that encodes how a detector smears the desired true quantity into
the measured observable(s), a deconvolution can be performed that provides an estimate
of the true variable. This deconvolution process is known as unfolding.


### Unfolding + Python data stack

Unfolding is a commonly used technique in the high-energy physics (HEP) community. However, the unfolding software tools used in HEP maintain a strong dependence on the [ROOT data analysis framework](https://root.cern.ch/), which is not typically used outside the HEP community. This dependency on ROOT is a hurdle for those who would like to use one of these unfolding packages, but don't want to learn an entire new analysis framework to do so.

Thus one of the main goals for PyUnfold is to provide an unfolding toolkit for members of _all scientific disciplines_ in an easy-to-use package. To accomplish this, PyUnfold is built on top of the familiar Python data stack (i.e. NumPy, SciPy, and pandas), thus broadening its scope to a general scientific audience.


### Key features

In addition to bringing unfolding to a more general audience, PyUnfold also implements several new features that members of the unfolding community have been advocating for. Some key features of PyUnfold include:

- Support for custom, user defined initial prior probability distributions.
- Unfolding stopping criteria based on test statistic calculations comparing unfolded distributions from one iteration to the next.
- Tunable spline regularization as a means of ensuring that unfolded distributions do not suffer from growing fluctuations potentially arising from the finite binning of the response matrix.
- Support for multidimensional unfolding.


### Unfolding API

PyUnfold has an `iterative_unfold` function that takes in as input the necessary observed distributions, response matrix, and detection efficiencies and returns the unfolded distribution and corresponding uncertainties.

```python
from pyunfold import iterative_unfold

# Load array-like observed distribution, response matrix,
# and detection efficiencies (e.g. NumPy ndarrays)
data = ...
data_err = ...
response = ...
response_err = ...
efficiencies = ...
efficiencies_err = ...

unfolded_result = iterative_unfold(data=data,
                                   data_err=data_err,
                                   response=response,
                                   response_err=response_err,
                                   efficiencies=efficiencies,
                                   efficiencies_err=efficiencies_err)
```

Additional inputs (e.g. a user-defined initial prior distribution) can also be provided to `iterative_unfold`. See the [PyUnfold API documentation](https://jrbourbeau.github.io/pyunfold/api.html) for complete details.

For example uses of PyUnfold, see the [PyUnfold Tutorial](https://jrbourbeau.github.io/pyunfold/notebooks/tutorial) and [Advanced Techniques](https://jrbourbeau.github.io/pyunfold/advanced) sections of the documentation.
