# Python Tools 

**This repo integrating different python tools.** 🐍

This repo should turn into the base structure of my python projects in order to raise their quality.

---
## Tools

### [Poetry](https://python-poetry.org/docs/basic-usage/)

> Poetry is a tool for dependency management and packaging in Python. It allows you to declare the libraries your project depends on, and it will manage (install/update) them for you.

#### Usage -

1. Linux -
```shell
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
```

2. Windows -
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
    * make sure to **upload pyproject.toml & poetry.lock to your vcs** in order to keep the actual app environment updated
    * poetry.lock file prevents you from auto getting the latest versions of your deps
    
### Flake8
