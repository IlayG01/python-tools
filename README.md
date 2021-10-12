# Python Tools

**This repo integrating different python tools.** 🐍

This repo should turn into the base structure of my python projects in order to raise their quality.

---

## Tools

### [Poetry](https://python-poetry.org/docs/basic-usage/)

> Poetry is a tool for dependency management and packaging in Python. It allows you to declare the libraries your project depends on, and it will manage (install/update) them for you.

#### Usage -

1. linux install -

   ```shell
   curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
   ```

2. windows install -

   ```shell
   (Invoke-WebRequest -Uri https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py -UseBasicParsing).Content | python -
   ```

3. check download with -

   ```shell
   poetry --version
   ```

4. poetry initialization -

   ```shell
   cd root-project
   poetry init
   ```

5. add dependencies -

   ```shell
   poetry add python-lib
   ```

6. download dependencies -

   ```shell
   poetry install
   ```

7. important notes -
    * make sure to **upload pyproject.toml & poetry.lock to your vcs** in order to keep the actual app environment
      updated
    * poetry.lock file prevents you from auto getting the latest versions of your deps

---

### [Flake8](https://flake8.pycqa.org/en/latest/user/index.html)

> Your Tool For Style Guide Enforcement

#### Usage

1. Flake8 install (**inside your virtual environment** - there is different flake8 for each python version) -

   ```shell
   poetry add flake8 || (python -m) pip install flake8
   ```

2. Flake8 run -

   ```shell
   flake8 path/to/file/check.py
   ```

3. available check flags - (select, ignore, quiet, count, max-line-length, help ...)

   ```shell
   flake8 --select E123,W503 path/to/file/check.py || flake8 --ignore E24,W504 path/to/file/check.py || flake8 --help
   ```

4. configuring Flake8 (Project Configuration) -

   ```shell
   touch .flake8
   ```

5. example Configuration file (Notice each command line are without hyphens) -

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

6. [**Flake8 Options**](https://flake8.pycqa.org/en/latest/user/options.html), [**Flake8 Error
   Codes**](https://flake8.pycqa.org/en/latest/user/error-codes.html)

---

### [Alembic](https://alembic.sqlalchemy.org/en/latest/tutorial.html)

> lightweight database migration tool for usage with the SQLAlchemy Database Toolkit for Python.

#### Usage

1. install alembic **inside your virtual environment** -
   ```shell
   pip install alembic
   ```

2. initialize alembic(creating the migration environment) in the project root directory
   ```shell
   alembic init
   ```

3. the migration environment structure
   ```
   project/
      alembic/
         ├── env.py                  # this is a Python script that is run whenever the alembic migration tool is invoked.
         ├── README                  # included with the various environment templates, should have something informative.
         ├── script.py.mako          # Mako template file which is used to generate new migration scripts. Whatever is here is used to generate new files within versions/.
         └── versions/               # this directory holds the individual version scripts
             ├── 3512b954651e_add_account.py
             ├── 2b1ae634e5cd_add_order_id.py
             └── 3adcc9a56557_rename_username_field.py                
   ```

4. alembic available templates
   ```shell
   alembic list_templates
   Available templates:
   
   generic - Generic single-database configuration.
   async - Generic single-database configuration with an async dbapi.
   multidb - Rudimentary multi-database configuration.
   pylons - Configuration that reads from a Pylons project environment.
   
   Templates are used via the 'init' command, e.g.:
   
     alembic init --template pylons ./scripts
   ```

5. [**alembic.ini**](https://alembic.sqlalchemy.org/en/latest/tutorial.html#editing-the-ini-file) file -

   Alembic placed a file alembic.ini into the current directory.

   This is a file that the alembic script looks for when invoked.

   This file can be anywhere, either in the same directory from which the alembic script will normally be invoked, or if
   in a different directory, can be specified by using the --config option to the alembic runner.

   `[alembic]` - this is the section read by Alembic to determine configuration.

   `script_location` - this is the location of the Alembic environment. It is normally specified as a filesystem
   location, either relative or absolute. If the location is a relative path, it’s interpreted as relative to the
   current directory.

   `file_template` - naming scheme used to generate new migration files.

   `sqlalchemy.url` - A URL to connect to the database via SQLAlchemy. This configuration value is only used if the
   env.py file calls upon them;

   **For starting up with just a single database and the generic configuration, setting up the SQLAlchemy URL is all
   that’s needed**


6. create a migration script -

   ```shell
   alembic revision -m "create account table"
   Generating /path/to/project/alembic/versions/1975ea83b712_create_account_table.py...done
   ```
   the created file

   ```python
   """
   create account table
   
   Revision ID: 1975ea83b712
   Revises:
   Create Date: 2020-11-08 11:40:27.089406
   """
   
   # revision identifiers, used by Alembic.
   revision = '1975ea83b712'
   down_revision = None
   branch_labels = None
   
   from alembic import op
   import sqlalchemy as sa
   
   def upgrade():
       pass
   
   def downgrade():
       pass
   ```

   Our job here is to populate the `upgrade()` and `downgrade()` functions with directives that will apply a set of
   changes to our database.

   Typically, `upgrade()` is required while `downgrade()` is only needed if down-revision capability is desired, though
   it’s probably a good idea.

   Another thing to notice is the `down_revision` variable. This is how Alembic knows the correct order in which to
   apply migrations. When we create the next revision, the new file’s down_revision identifier would point to this one:

   ```python
   # revision identifiers, used by Alembic.
   revision = 'ae1027a6acf'
   down_revision = '1975ea83b712'
   ```

   **Every time Alembic runs an operation against the `versions/` directory, it reads all the files in, and composes a
   list based on how the `down_revision` identifiers link together, with the `down_revision = None` representing the
   first file.**


7. upgrade example

   ```python
   def upgrade():
      op.create_table(
        'account',
        sa.Column('id', sa.Integer, primary_key=True),
        sa.Column('name', sa.String(50), nullable=False),
        sa.Column('description', sa.Unicode(200)),
       )
   
   def downgrade():
       op.drop_table('account')
   ```

8. running first migration

   We now want to run our migration. Assuming our database is totally clean, it’s as yet un-versioned.

   The alembic upgrade command will run upgrade operations, proceeding from the current database revision, in this
   example `None`, to the given target revision.

   We can specify `1975ea83b712` as the revision we’d like to upgrade to, but it’s easier in most cases just to tell it
   “the most recent”, in this case `head`.

   result in -

   ```shell
   $ alembic upgrade head
   INFO  [alembic.context] Context class PostgresqlContext.
   INFO  [alembic.context] Will assume transactional DDL.
   INFO  [alembic.context] Running upgrade None -> 1975ea83b712
   ```

9. running second migration

   ```shell
   $ alembic revision -m "Add a column"
   Generating /path/to/yourapp/alembic/versions/ae1027a6acf_add_a_column.py...done
   ```

   let's edit this file

   ```python
   """
   Add a column
   
   Revision ID: ae1027a6acf
   Revises: 1975ea83b712
   Create Date: 2011-11-08 12:37:36.714947
   """
   
   # revision identifiers, used by Alembic.
   revision = 'ae1027a6acf'
   down_revision = '1975ea83b712'
   
   from alembic import op
   import sqlalchemy as sa
   
   def upgrade():
       op.add_column('account', sa.Column('last_transaction_date', sa.DateTime))
   
   def downgrade():
       op.drop_column('account', 'last_transaction_date')
   ```

   running again to head

   ```shell
   $ alembic upgrade head
   INFO  [alembic.context] Context class PostgresqlContext.
   INFO  [alembic.context] Will assume transactional DDL.
   INFO  [alembic.context] Running upgrade 1975ea83b712 -> ae1027a6acf
   ```

   We’ve now added the `last_transaction_date` column to the database.


10. relative migration -

      ```shell
      $ alembic upgrade +2
      $ alembic downgrade -1
      $ alembic upgrade ae10+2
      ```

11. get info -

      ```shell
      $ alembic current
      $ alembic history --verbose
      ```

12. auto generating migrations using `--autogenerate` flag

    first we need to modify `env.py` file so that it gets access to a table metadata object that contains the target.

    from

      ```python
      # add your model's MetaData object here
      # for 'autogenerate' support
      # from myapp import mymodel
      # target_metadata = mymodel.Base.metadata
      target_metadata = None
      ```
    to

      ```phyton
      from myapp.mymodel import Base
      target_metadata = Base.metadata
      ```

    The migration hasn’t actually run yet, of course. We do that via the usual `upgrade` command. We should also go into
    our migration file and alter it as needed, including adjustments to the directives as well as the addition of other
    directives which these may be dependent on - specifically data changes in between creates/alters/drops.


13. [**autogenerate**](https://alembic.sqlalchemy.org/en/latest/autogenerate.html) notes -

    **autogenerate is not intended to be perfect. It is always necessary to manually review and correct the candidate
    migrations that autogenerate produces.**

    Detects -

    * Table additions, removals.
    * Column additions, removals.
    * Change of nullable status on columns.
    * Basic changes in indexes and explicitly-named unique constraints
    * Basic changes in foreign key constraints

    Optionally -
    * Change of column type. if you set the `EnvironmentContext.configure.compare_type` parameter to `True`.
    * Change of server default. if you set the `EnvironmentContext.configure.compare_server_default` parameter to `True`
      .

    Never -

    * Changes of table name.
    * Changes of column name.
    * Anonymously named constraints.
    * [Special SQLAlchemy types such as Enum](https://stackoverflow.com/questions/47206201/how-to-use-enum-with-sqlalchemy-and-alembic)
      when generated on a backend which doesn't support ENUM directly.

14. [Controlling what to be auto-generated](https://alembic.sqlalchemy.org/en/latest/autogenerate.html#comparing-and-rendering-types)
    | [Comparing and rendering types](https://alembic.sqlalchemy.org/en/latest/autogenerate.html#comparing-and-rendering-types)
    | [Applying post-processing and python code formatters to generated](https://alembic.sqlalchemy.org/en/latest/autogenerate.html#applying-post-processing-and-python-code-formatters-to-generated-revisions)

---

### [mypy](https://mypy.readthedocs.io/en/stable/getting_started.html)

> static type checker for Python 3 and Python 2.7. If you sprinkle your code with type annotations, mypy can type check your code and find common bugs.

#### Usage

1. install mypy **inside your virtual environment** -
   ```shell
   pip install mypy
   ```

2. function signatures -

   By default, mypy will not type check dynamically typed functions. This means that with a few exceptions, mypy will
   not report any errors with regular unannotated Python.

    ```python
    def greeting(name):
        return 'Hello ' + name
   
    greeting(3)  # will not report
   
    def typed_greeting(name: str) -> str:
        return 'Hello ' + name
   
    typed_greeting(3)  # will report
    ```

3. when writing typed python and using mypy a lot of small bugs can be prevented. mypy **supports
   python `typing` `collections.abc`** modules - use them.


4. **built-in & third-party libraries** - Mypy uses library stubs to type check code interacting with library modules,
   including the Python standard library.


5. configuring mypy - [available command line](https://mypy.readthedocs.io/en/stable/command_line.html#command-line)

   flags like `--disallow-untyped-defs` `--strict`
   > useful if you’re starting a new project from scratch and want to maintain a high degree of type safety from day one. However, this flag will probably be too aggressive if you either plan on using many untyped third party libraries or are trying to add static types to a large, existing codebase.


6. [mypy cheatsheet](https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html#cheat-sheet-py3)
   | [using mypy with existing codebase](https://mypy.readthedocs.io/en/stable/existing_code.html#existing-code)


7. practical quick usage

    ```shell
    $ mypy file_1.py foo/file_2.py file_3.pyi some/directory
    ```

    ```shell
    $ mypy -m html.parser  # module checker
    ```

    ```shell
    $ mypy -p html  # package checker
    ```

    ```shell
    $ mypy --package p.a --package p.b --module c
    ```

    ```shell
    $ mypy -c 'x = [1, 2]; print(x())'
    ```

    ```shell
    $ mypy --install-types  # if library stubs not installed
    ```

   mypy configuration file - by default it uses the file `mypy.ini`

    ```ini
   # if library doesnt support types and we couldn't find her typed version / rewrite it
   [mypy-library.*]
   ignore_missing_imports = True
   # for every missing type report the best practice is to skip less.
   # import library  # type: ignore
   # [mypy-library.*]
   # [mypy]
    ```

   example

    ```ini
   # Global options:
   [mypy]
   python_version = 2.7
   warn_return_any = True
   warn_unused_configs = True
   
   # Per-module options:
   
   [mypy-mycode.foo.*]
   disallow_untyped_defs = True
   
   [mypy-mycode.bar]
   warn_return_any = False
   
   [mypy-somelibrary]
   ignore_missing_imports = True
    ```
   
    check usefull info [here](https://mypy.readthedocs.io/en/stable/config_file.html#disallow-dynamic-typing)