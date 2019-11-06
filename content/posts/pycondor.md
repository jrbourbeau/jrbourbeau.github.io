---
title:  HTCondor made easy with PyCondor
date: 2017-02-06
---


[HTCondor](https://research.cs.wisc.edu/htcondor/) is a an open-source framework for high throughput computing developed at the University of Wisconsin&mdash;Madison. It's an incredibly useful and versatile tool that I rely on heavily in my everyday work. However, I've found when the number of jobs becomes large (especially when there are interrelated jobs), actually making the job submission files can become a pain.

This prompted me to make [PyCondor](https://github.com/jrbourbeau/pycondor) (Python HTCondor), a tool that helps build and submit HTCondor jobs in a straight-forward manner with minimal hassle.

With just a couple lines of code, you can get build and submit an HTCondor submission file!

```python
import pycondor

# Setting up a PyCondor Job
job = pycondor.Job('examplejob', 'script.py')
# Write all necessary submit files and submit job to Condor
job.build_submit()
```


Here are some useful links related to the project:

* Documentation: [https://jrbourbeau.github.io/pycondor](https://jrbourbeau.github.io/pycondor)
* GitHub repository: [https://github.com/jrbourbeau/pycondor](https://github.com/jrbourbeau/pycondor)
* PyPI: [https://pypi.python.org/pypi/PyCondor](https://pypi.python.org/pypi/PyCondor)
* Issue tracker: [https://github.com/jrbourbeau/pycondor/issues](https://github.com/jrbourbeau/pycondor/issues)
