---
title: Mocking in Software Tests
date: December 1, 2018
toc: false
---

This week I learned about mocking code behavior when writing tests. This is really cool, so I thought I would write a little about it. 

<!-- excerpt -->

Recently, I wrote a [Python wrapper](https://jrbourbeau.github.io/openbrewerydb-python/) for the [Open Brewery DB](https://www.openbrewerydb.org/) API. At first, the tests I wrote for my Python wrapper included code that actually requested data from the Open Brewery DB external server. This wasn't ideal because the tests took a long time to run (a few seconds for each test, but this can add up during development when you're running tests frequently). In addition, if the API server went down for any period of time, then the tests for the Python wrapper would start to fail, even if there were no changes made to the code.

This post walks through writing a test for a function that makes an HTTP request, while avoiding actually making the request.


## Example HTTP request

Below is a `get_org_repo_data` function that uses the [Requests](http://docs.python-requests.org/en/master/) library to make an HTTP request to the [GitHub API](https://developer.github.com/v3/repos/#list-organization-repositories) to get information about a GitHub organization's repositories. 

```python
# example.py
import requests

def get_org_repo_data(org):
    url = f'https://api.github.com/orgs/{org}/repos'
    r = requests.get(url)
    if r.status_code == 200:
        return r.json()
    else:
        return None
```

The function returns either JSON data about the organization's repositories, or `None` if the request response `status_code` indicates there was an issue with the request.

When writing tests for `get_org_repo_data`, we'll want to make sure that `None` is returned when the `status_code` for the request response is not `200`. This could be done, for instance, by passing `get_org_repo_data` the name of a non-existent GitHub organization.

```python
# test_example.py
from example import get_org_repo_data

def test_get_org_repo_data_invalid():
    result = get_org_repo_data(org='this-is-an-invalid-org')
    assert result is None
```

Because the organization `'this-is-an-invalid-org'` doesn't exist on GitHub, the request response `status_code` will be `404` and `get_org_repo_data` will return `None`. This test will pass. However, every time the test is run, it will make an HTTP request to the GitHub API, which introduces some of the problems mentioned at the beginning of this post.

Is there a better way?


## Mocking `requests.get`

Mocking is a way to replace the normal functionality of some piece of code. The `unittest.mock` library in the Python standard library has a [`Mock` class](https://docs.python.org/3/library/unittest.mock.html#the-mock-class) and [`patch` function decorator](https://docs.python.org/3/library/unittest.mock.html#unittest.mock.patch) that will help us do this (note that `unittest.mock` was added in Python version 3.3).

Instances of the `Mock` class are callable and allow you to assign return values and class attributes as you wish.

```python
from unittest.mock import Mock

a = Mock(status_code=200)
assert a.status_code == 200
a.return_value = 'foo'
assert a() == 'foo'
```

`Mock` objects are super flexible, you can give them any attributes you want. The `patch` function decorator can be used to replace a specified function with a mock object. So setting the `return_value` for the mock object allows you to modify the patched function to behave however you want it to. 

In the `test_get_org_repo_data_invalid` test, we can use `Mock` and `patch` together to have the `requests.get` function return a mock object with a `status_code` attribute that indicates an error. The updated test that mocks `requests.get` looks like:

```python
# test_example.py
from example import get_org_repo_data
from unittest.mock import Mock, patch

@patch('example.requests.get')
def test_get_org_repo_data_invalid(mock_get):
    mock_get.return_value = Mock(status_code=404)
    result = get_org_repo_data(org='this-is-an-invalid-org')
    assert result is None
```

Here we used the `patch` decorator to replace the `requests.get` function in `example.py` with a mock object that we've called `mock_get` (note that the mock object gets passed as an extra argument to the decorated test function). We then assign the return value of `mock_get` to be another mock object with a `status_code` attribute of `404`. Now when `test_get_org_repo_data_invalid` is run we are still testing that `None` is returned, but we did so without actually making an HTTP request!


Here we used the `unittest.mock` library to mock an HTTP request. However, `Mock` and `patch` can be used to mock any component of a codebase! This is a useful technique to have in your testing toolbox.


## Resources

- [`unittest.mock` library docs](https://docs.python.org/3/library/unittest.mock.html)
- [Mocking External APIs in Python](https://realpython.com/testing-third-party-apis-with-mocks/) &mdash; Great article on Real Python that goes in depth about mocking in Python tests
- [pytest-remotedata](https://github.com/astropy/pytest-remotedata) &mdash; pytest plugin for dealing with tests that require data from the internet 
