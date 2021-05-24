# pytest

```python
from palindrome import is_palindrome

def test_palindrome_pass():
    string = 'capac'
    assert is_palindrome(string) == True

def test_palindrome_fail():
    string = 'capaz'
    assert is_palindrome(string) == True
```

```python
def is_palindrome(s):
    return s == s[::-1]
```
Run tests:
```bash
pytest tests/test_palindrome.py
```

Use the verbose option:
```bash
pytest -v tests/test_palindrome.py
```
[Pytest output](assets/pytest-ouput.png)
```
```

```
```

```
```