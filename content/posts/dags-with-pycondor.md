---
title: Creating DAGs with PyCondor
date: 2018-01-10
---

Often the need arises to perform a series of tasks that are related to one another. For example, you might have a workflow that's something like "do task A, then do task B, and then do task C". These dependencies, task A must be done before task B and task B must be done before task C, can be encapsulated in a directed acyclic graph (DAG). There are many libraries and frameworks for constructing and executing DAGs. One in particular, [HTCondor](https://research.cs.wisc.edu/htcondor/), I utilize in my work.

This tutorial walks through constructing an example DAG (shown below) using [PyCondor](https://github.com/jrbourbeau/pycondor/), a Python package for building workflows that can be submitted to an HTCondor cluster. This example DAG can be found in the [PyCondor example scripts on GitHub](https://github.com/jrbourbeau/pycondor/blob/master/examples/tutorial.py).
<!-- {: .notice--success} -->

```python
from pycondor import Job, Dagman

# Define the error, output, log, and submit directories
error = 'condor/error'
output = 'condor/output'
log = 'condor/log'
submit = 'condor/submit'

# Instantiate a Dagman
dagman = Dagman(name='tutorial_dagman',
                submit=submit)

# Instantiate Jobs
job_date = Job(name='date_job',
               executable='/bin/date',
               submit=submit,
               error=error,
               output=output,
               log=log,
               dag=dagman)

job_sleep = Job(name='sleep_job',
                executable='/bin/sleep',
                submit=submit,
                error=error,
                output=output,
                log=log,
                dag=dagman)
job_sleep.add_arg('1')
job_sleep.add_arg('2')
job_sleep.add_arg('3')

# Add inter-job relationship
# Ensure that job_sleep finishes before job_date starts
job_sleep.add_child(job_date)

# Write all necessary submit files and submit job to HTCondor
dagman.build_submit()
```

### Job and Dagman objects

The basic building blocks in PyCondor are the Job and Dagman objects. A Job object represents an executable (e.g. a shell command, Python script, etc.) that you would like to run on an HTCondor cluster. While the Dagman object represents a collection of Jobs to be run.

```python
from pycondor import Job, Dagman
```


Both the Job and Dagman objects can be imported directly from `pycondor`.


### Job and Dagman file directories

There are several files that will be generated for both Job and Dagman objects. For each Job and Dagman object, PyCondor will create a submit file. This file will be formatted such that it can be submitted to HTCondor for execution. In addition to submit files, log files and files containing the standard output and error for each Job will also be generated.

```python
# Define the error, output, log, and submit directories
error = 'condor/error'
output = 'condor/output'
log = 'condor/log'
submit = 'condor/submit'
```

For this tutorial, we will explicitly specify the directories that we would like the error, output, log, and submit files to be saved to. If not specified, the current working directory will be used.

Note that these file directories can also be specified by setting the `PYCONDOR_SUBMIT_DIR`, `PYCONDOR_ERROR_DIR`, `PYCONDOR_LOG_DIR`, and `PYCONDOR_OUTPUT_DIR` environment variables. For example, setting `PYCONDOR_SUBMIT_DIR=condor/submit` is equivalent to the above.


### Setting up a Dagman

The Dagman object (short for directed acyclic graph manager) represents a collection of Jobs to be run.

```python
# Instantiate a Dagman
dagman = Dagman(name='tutorial_dagman',
                submit=submit)
```

For a Dagman, only a `name` has to be provided (used to construct the submit, log, etc. file names). While the `submit` parameter is the directory path where the Dagman submit file will be saved (if not provided, it defaults to the current directory).


### Setting up Jobs

Now we're ready to add some Jobs to the Dagman. Both a `name` and an `executable` must be provided to create a Job.

```python
# Instantiate Jobs
job_date = Job(name='date_job',
               executable='/bin/date',
               submit=submit,
               error=error,
               output=output,
               log=log,
               dag=dagman)

job_sleep = Job(name='sleep_job',
                executable='/bin/sleep',
                submit=submit,
                error=error,
                output=output,
                log=log,
                dag=dagman)
job_sleep.add_arg('1')
job_sleep.add_arg('2')
job_sleep.add_arg('3')
```

In this example, `job_date` will run the shell `date` command, and `job_sleep` will run the shell `sleep` command. A Job can be added to a Dagman object by passing a Dagman to the Job `dag` parameter.

In addition to defining an executable for a Job to run, you can also pass arguments to the executable using the Job `add_arg` method. Here, we've added three arguments, `1`, `2`, and `3`, to `job_sleep`. This Job will now run the `sleep` command on each of the provided arguments, i.e. `sleep 1`, `sleep 2`, and `sleep 3`.


### Adding inter-job relationships

One useful feature of Dagman objects is that they can support inter-job relationships between the Jobs they manage. Inter-job relationships in PyCondor can be specified using the Job `add_child` and `add_parent` methods.

```python
# Add inter-job relationship
# Ensure that job_sleep finishes before job_date starts
job_sleep.add_child(job_date)
```

In this example, `job_sleep.add_child(job_date)` sets `job_date` as a child Job of `job_sleep`. This means that `job_date` will start running only after `job_sleep` has finished. Note that `job_sleep.add_child(job_date)` is equivalent to `job_date.add_parent(job_sleep)`.


### Build and submit Dagman

Now that the relationships between the Jobs in our Dagman have been specified, we're ready to build all the appropriate Job and Dagman submit files and submit them to HTCondor for execution.


```python
# Write all necessary submit files and submit job to HTCondor
dagman.build_submit()
```

The Dagman `build_submit` method is used to both build the Job and Dagman submit files as well as submit them to HTCondor. Note that the `build_submit` method is just a convenience method for the `build` Dagman method followed by the `submit_dag` method.


There you go, we've built a DAG using PyCondor! For more examples see the [examples](https://jrbourbeau.github.io/pycondor/examples.html) section of the PyCondor documentation.
