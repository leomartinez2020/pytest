# pytest

### File organization:

Use venv and pip to install pytest.
In order to call the testable functions from the test folder we need to convert the src file into a package with the help of the file setup.py

```
setup.py
src/
    __init__.py
    palindrome.py
tests/
    test_palindrome.py
```

The file setup.py contains:
```python
from setuptools import setup, find_packages

setup(name="src", packages=find_packages())
```
Then to install the "src" package we run:
```
pip install -e .
```

```python
def is_palindrome(s):
    return s == s[::-1]
```

```python
from src.palindrome import is_palindrome

def test_palindrome_pass():
    string = 'capac'
    assert is_palindrome(string) == True

def test_palindrome_fail():
    string = 'capaz'
    assert is_palindrome(string) == True
```

Run tests:
```bash
pytest tests/test_palindrome.py
```

Use the verbose option:
```bash
pytest -v tests/test_palindrome.py
```
![Pytest output](assets/pytest-ouput.png)

Possible outcomes for a test:

1) PASSED (.)
2) FAILED (F)
3) SKIPPED (s)
4) xfail (x)
5) XPASS (X)
6) ERROR (E)

Run only one test:
```
pytest tests/test_palindrome.py::test_palindrome_pass
```

Use --collect-only to check what tests are going to be executed but not to run them
```
pytest --collect-options tests/test_palindrome.py
```

Use -k to run tests that have a common preffix or suffix
```
pytest -k "pass or fail" tests/
```

## Marking the tests with a decorator

```python
import pytest
from src.palindrome import is_palindrome

@pytest.mark.passing
def test_palindrome_pass():
    string = 'capac'
    assert is_palindrome(string) == True

@pytest.mark.failing
def test_palindrome_fail():
    string = 'capaz'
    assert is_palindrome(string) == True
```

Run the marked function or functions with the command option -m. We have first to register the marks in ta file called pytest.ini which should have this content:

```python
[pytest]
markers =
    passsing: marks tests as passing
    failing: marks tests as failing
```

Running the command:
```
pytest -m passing
pytest -m failing
```
Output for failing test:

![Pytest output](assets/test-mark.png)

## Stopping the test session

The default behavior of pytest is to run all tests even if one of them fails. Use the option -x to exit as soon as a test fails:
```
pytest -x tests/
```

To set a maximum number of test fails, we can use --maxfail
```
pytest --maxfail=2 tests/
```
## Using print statements

You can use -s or the equivalent --capture=no to turn off the capture of output.

The -l flag is used to print the local variables.

The flag -fd points file descriptors 1 and 2 to a temp file:
```
pytest --capture=fd
```

The flags --lf or --last-failed are used to only re-run the failures.

The flags --ff or --failed-first allow re-running the failed tests first and then the rest.

Use -v or --verbose to report more information. Use -q or --quiet to have less information.

## Modyfying tracebacks

--tb=no removes all traceback
--tb=line prints only one line of traceback.
--tb=short prints the assertion and the E line without context.

## Durations

--durations=2 reports the two slowest number of tests. --durations=0 reports everything from the slowest to the fastest.

## Making a package

project
|-- setup.py
|-- src
|     |__ working_code
|       |-- __init__.py
|       |
|       |-- script1.py
|
|-- tests
|     |-- pytest.ini
|     |
|     |-- test_script1.py

In order to call the functions in the code under the src folder, we need to add an empty __init__.py file to indicate this folder is a package. Then in the root project we create a setup.py file with the content:

```
from setuptools import setup, find_packages

setup(name='src', packages=find_packages())
```

Then we install the package with the cli command:
```
pip install -e .
```
Then we can import any function from the package in any test in the test folder:

```
from src.script1 import my_func

def test_my_func():
    pass
```

## Expecting exceptions

We can tell a test to expect an exception when we on purpose use the incorrect arguments in a function, for example.

```
import pytest

def test_zero_division():
    with pytest.raises(ZeroDivisionError):
        y = 10 / 0
```
The test passes if the exception is raised. This is useful to test edge cases.

We can even assert error message expected:
```
import pytest

def test_zero_division():
    with pytest.raises(ZeroDivisionError) as excinfo:
        y = 10 / 0
    assert excinfo.value.args[0] == 'division by zero'
```

