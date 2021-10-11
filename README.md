# Python Tools

**This repo integrating different python tools.** 🐍

This repo should turn into the base structure of my python projects in order to raise their quality.

---

## Tools

### [Poetry](https://python-poetry.org/docs/basic-usage/)

> Poetry is a tool for dependency management and packaging in Python. It allows you to declare the libraries your project depends on, and it will manage (install/update) them for you.

#### Usage -

1. Linux install -

```shell
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
```

2. Windows install -

```shell
(Invoke-WebRequest -Uri https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -UseBasicParsing).Content | python -
```

3. Check download with -

```shell
poetry --version
```

4. Poetry initialization -

```shell
cd root-project
poetry init
```

5. Add dependencies -

```shell
poetry add python-lib
```

6. Download dependencies -

```shell
poetry install
```

7. Important notes -
    * make sure to **upload pyproject.toml & poetry.lock to your vcs** in order to keep the actual app environment
      updated
    * poetry.lock file prevents you from auto getting the latest versions of your deps

---

### [Flake8](https://flake8.pycqa.org/en/latest/user/index.html)

> Your Tool For Style Guide Enforcement

#### Usage

1. Flake8 install (**inside your venv** - there is different flake8 for each python version) -

```shell
poetry add flake8 || (python -m) pip install flake8
```

2. Flake8 run -

```shell
flake8 path/to/file/check.py
```

3. Available check flags - (select, ignore, quiet, count, max-line-length, help ...)

```shell
flake8 --select E123,W503 path/to/file/check.py || flake8 --ignore E24,W504 path/to/file/check.py || flake8 --help
```

4. Configuring Flake8 (Project Configuration) -

```shell
touch .flake8
```

5. Example Configuration file (Notice each command line are without hyphens) -

```ini
[flake8]
# it's not a bug that we aren't using all of hacking, ignore:
# F812: list comprehension redefines ...
# H202: assertRaises Exception too broad
# H233: Python 3.x incompatible use of print operator
# H301: one import per line
# H306: imports not in alphabetical order (time, os)
# H401: docstring should not start with a space
# H403: multi line docstrings should end on a new line
# H404: multi line docstring should start without a leading new line
# H405: multi line docstring summary not separated with an empty line
# H501: Do not use self.__dict__ for string formatting
ignore = F812,H202,H233,H301,H306,H401,H403,H404,H405,H501
max-line-length = 88
exclude = .git,__pycache__,__init__.py,.mypy_cache,.pytest_cache
```

6. [**Flake8 Options**](https://flake8.pycqa.org/en/latest/user/options.html), [**Flake8 Error Codes**](https://flake8.pycqa.org/en/latest/user/error-codes.html)

