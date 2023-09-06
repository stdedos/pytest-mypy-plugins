# pytest-mypy-plugins test file

*JSON Schema for a pytest-mypy-plugins test file*

## Items

- **Items** *(object)*: Cannot contain additional properties.
  - **`case`** *(string, required)*: Name of the test case, MUST comply to the `^[a-zA-Z0-9_]+$` pattern.

    Examples:
    ```yaml
    case: TestCase1
    ```

    ```yaml
    case: '999'
    ```

    ```yaml
    case: test_case_1
    ```

  - **`main`** *(string, required)*: Portion of the code as if written in `.py` file. Must be valid Python code.

    Examples:
    ```yaml
    main: reveal_type(1)
    ```

  - **`out`** *(string)*: Verbose output expected from `mypy`.

    Examples:
    ```yaml
    out: 'main:1: note: Revealed type is "Literal[1]?"'
    ```

  - **`files`** *(array)*: List of extra files to simulate imports if needed.
    - **Items**: Refer to *[#/definitions/File](#definitions/File)*.

    Examples:
    ```yaml
    -   path: myapp/__init__.py
    -   content: 'def help(): pass'
        path: myapp/utils.py
    ```

  - **`disable_cache`** *(boolean)*: Set to `true` disables `mypy` caching. Default: `false`.
  - **`mypy_config`** *(string)*: Inline `mypy` configuration, passed directly to `mypy`.

    Examples:
    ```yaml
    mypy_config: |
        force_uppercase_builtins = true
        force_union_syntax = true
    ```

  - **`env`** *(array)*: Environmental variables to be provided inside of test run.
    - **Items** *(string)*

    Examples:
    ```yaml
    MYPYPATH=../extras
    ...
    ```

    ```yaml
    DJANGO_SETTINGS_MODULE=mysettings
    ...
    ```

  - **`parametrized`** *(array)*: List of parameters, similar to [`@pytest.mark.parametrize`](https://docs.pytest.org/en/stable/parametrize.html). Each entry **must** have the **exact** same set of keys.
    - **Items**: Refer to *[#/definitions/Parameter](#definitions/Parameter)*.

    Examples:
    ```yaml
    -   rt: int
        val: 1
    -   rt: str
        val: '1'
    ```

  - **`skip`**: An expression set in `skip` is passed directly into [`eval`](https://docs.python.org/3/library/functions.html#eval). It is advised to take a peek and learn about how `eval` works. Expression evaluated with following globals set: `sys`, `os`, `pytest` and `platform`. Default: `false`.
    - **Any of**
      - *boolean*
      - *string*

    Examples:
    ```yaml
    'yes'
    ```

    ```yaml
    true
    ...
    ```

    ```yaml
    sys.version_info > (2, 0)
    ...
    ```

  - **`expect_fail`** *(boolean)*: Mark test case as an expected failure. Default: `false`.
  - **`regex`** *(boolean)*: Allow regular expressions in comments to be matched against actual output. _See pytest_mypy_plugins/tests/test-regex_assertions.yml for examples_. Default: `false`.
  - **`reveal_type`**: Shorthand for<br>
    ```yaml
    main: |
      reveal_type({{ reveal_type }})
    ```

    Must be a syntactically valid Python expression.

    Examples:
    ```yaml
    '1'
    ```

    ```yaml
    1
    ...
    ```

    ```yaml
    true
    ...
    ```

    ```yaml
    sys.version_info
    ...
    ```

## Definitions

- <a id="definitions/File"></a>**`File`** *(object)*
  - **`path`** *(string, required)*: File path.

    Examples:
    ```yaml
    ../extras/extra_module.py
    ...
    ```

    ```yaml
    myapp/__init__.py
    ...
    ```

  - **`content`** *(string)*: File content. Can be empty. Must be valid Python code.

    Examples:
    ```yaml
    'def help(): pass'
    ```

    ```yaml
    |
        def help():
            pass
    ```

- <a id="definitions/Parameter"></a>**`Parameter`** *(object)*: A mapping of keys to values, similar to Python's `Mapping[str, Any]`. Can contain additional properties.

  Examples:
  ```yaml
  rt: str
  val: '1'
  ```

