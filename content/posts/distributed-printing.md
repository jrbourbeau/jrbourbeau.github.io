---
title: Distributed printing
date: 2023-05-18
author: James Bourbeau
---

_The original version of this post appears on [blog.coiled.io](https://blog.coiled.io/blog/distributed-printing.html)_

_Dask makes it easy to print whether you're running code locally on your laptop, or remotely on a cluster in the cloud._

One of the most basic things programmers do is print text to their screen.

Printing is often used for things like debugging:

```python
# ...
print("Made it here...")
# ...
```

or to signal progress:

```python
for i in range(10):
    print(f"Done with iteration {i}")
```

However, when running code at scale on a Dask cluster even simple `print` calls
can become non-intuitive.

For example, this snippet runs two functions on a Dask/Coiled cluster in the cloud:

```python
import coiled
import dask

# Spin up cluster
cluster = coiled.Cluster()
client = cluster.get_client()

@dask.delayed
def inc(x):
    print(f"Calling inc on {x}")
    return x + 1

@dask.delayed
def add(x, y):
    print(f"Calling add on {x} + {y}")
    return x + y

# Run computations on the cluster
x = inc(1)
y = add(x, 7)
y.compute()
```

Both the `inc` and `add` functions have `print` calls and, naively, I'd expect to see

```
Calling inc on 1
Calling add on 2 + 7
```

printed to my screen when I run this example. However, when you run this code snippet you don't see
any output. That's because those prints happen on the remote Dask workers (in this case on AWS)
where these tasks are run. We can see the result of these prints by looking our
[cluster logs](https://docs.coiled.io/user_guide/logging.html).

<div align="center">

![print-in-worker-logs](/images/print-in-worker-logs.png)

</div>

It's good that these prints work, but it's not a very pleasant experience and probably doesn't
match most user's expectations.

To make printing on a cluster similar to on programmer's local machine, Dask has a
[`dask.distributed.print`](https://distributed.dask.org/en/stable/api.html#distributed.print)
function which mirrors [Python's built-in `print` function](https://docs.python.org/3/library/functions.html#print)
but, when called in a task running on a cluster, the output of the `print`
will be forwarded back to the user's client Python session and printed there.

Here's the same example as above, but with one line added to use Dask's `print`:

```python
import coiled
import dask
from dask.distributed import print  # This line is the only difference

# Spin up cluster
cluster = coiled.Cluster()
client = cluster.get_client()

@dask.delayed
def inc(x):
    print(f"Calling inc on {x}")
    return x + 1

@dask.delayed
def add(x, y):
    print(f"Calling add on {x} + {y}")
    return x + y

# Run computations on the cluster
x = inc(1)
y = add(x, 7)
y.compute()
```

Running this on my laptop prints

```
Calling inc on 1
Calling add on 2 + 7
```

in my IPython session 🎉

When using `print`s inside tasks running on a Dask cluster, know that `dask.distributed.print`
is available and probably what you want to use.

>    Dask also has a [`dask.distributed.warn`](https://distributed.dask.org/en/stable/api.html#distributed.warn)
>    utility, which mirrors [Python's built-in `warnings.warn`](https://docs.python.org/3/library/warnings.html#warnings.warn)
>    that forwards warnings to user's client Python session.