## Create the project stub

This section creates a boilerplate Django Rest Framework app with a dedicated virtual-env and some favorite tools and PyCharm integration - could be used for any DRF project.

At repo root:

```sh
# Install pyenv, if not already installed
brew install pyenv
brew install pyenv-virtualenv

# Install our base python version - for use with pyenv virtual-envs
pyenv install 3.7.0

# You MUST create the virtual-env specifying the python version to avoid
# 'command not found' errors when running some external tools.
pyenv virtualenv --python=python3.7 3.7.0 dgi-3.7.0

# Create .python-version which automatically activates this venv on dir entry
pyenv local dgi-3.7.0

# Install Django
pip install --upgrade pip
pip install Django

# Constrain Django to the version we just installed
echo "Django==2.1.1" > constraints.txt
echo "-c constraints.txt\n\nDjango" > requirements.txt

# Create the Django starter app
mkdir src
django-admin startproject project src
```

The final command puts all source in `src` and project management files in `src/project`.

Open the project in PyCharm and:

1. Create a new `.gitignore` file using the Python template.
    1. Remove the `.python-version` entry
1. Open PyCharm Preferences -> Project: be -> Project Interpreter
    1. Click the 'Settings' gear icon right of the Project Interpreter
    1. Select 'Existing environment'
    1. Select 'dgi-3.7.0'
    1. OK all dialogs
1. Commit all files as an 'initial commit'

At this point, the project is setup for editing in PyCharm and is runnable from `be/src` with `./manage.py runserver`.

## Add Operational support

Create a `requirements-dv.txt` and add some favorites:

```
-r requirements.txt

black
coverage
ipython
mypy
pre-commit
pylint
pytest
```

Now, the local dev setup is `pip install -r requirements-dv.txt`

### Setup pylint

Generate a boilerplate `.pylintrc` with: `pylint --generate-rcfile > .pylintrc`

Add pylint support to PyCharm (2018.2.3):  Open PyCharm preferences, select External Tools, and click the add button.

* Name: pylint
* Description: A Python source code analyzer which looks for programming errors, helps enforcing a coding standard and sniffs for some code smells.
* Program: []output from 'pyenv which pylint']
* Arguments: $FilePath$
* Working Directory: $FileDir$
* Synchronize files after execution: checked

Now you can pylint any file or directory with a right-click and External Tools -> pylint

### Setup coverage

Add the default `.coveragerc` and tweek it a little.

### Setup black

See `pyproject.toml`

### Setup pre-commit

See `.pre-commit-config.yaml` installed at repo root.

Run `pre-commit run --all-files` to test your configuration.

Run `pre-commit install` to install pre-commit into your git hooks. pre-commit will now run on every commit.

To update to the latest version of your hook repos, run `pre-commit autoupdate`.

### Add common scripts

* `pip-clean.sh` - remove all dependencies for testing pip install.
* `check-types.sh` - run `mypy` to check type annotations
* `format.sh` - run `black` to format all files


### Add DRF (ReST support)

```
ᐅ pip install djangorestframework
Collecting djangorestframework
  Using cached https://files.pythonhosted.org/packages/90/30/ad1148098ff0c375df2a30cc4494ed953cf7551fc1ecec30fc951c712d20/djangorestframework-3.8.2-py2.py3-none-any.whl
Installing collected packages: djangorestframework
Successfully installed djangorestframework-3.8.2
ᐅ pip freeze
Django==2.1.1
djangorestframework==3.8.2
pytz==2018.5
```

* Add `djangorestframework==3.8.2` to constraints.txt
* Add `djangorestframework` to requirements.txt

**NOTE**: we are only constraining the major frameworks instead of all dependencies. We could lock this down more tightly by `pip freeze > constraints.txt` after every install. In practice, if a framework updates a dependency without changing its version, it is an important bug fix that does not break backwards compatibility. Those changes haven't bit me yet, so I err on the liberal side.

### Common commands

* `pip install -r requirements-dv.txt`: Install all local dev dependencies.
* `./scripts/pip-clean.sh`: Remove all python virtual-env installed dependencies to test prod/dv venv setup.
* `pre-commit run --all-files`: pre-commit check - runs all validations so commits don't fail.


At this point, we have a pretty workable project stub. There is more that could be added but it is largely project specific.

Commit code.

## Create the first app (endpoint)



## TODO

* Add a docker postgres that persists data to a local data dir
* Figure out a basic stock schema
* Define the basic stock model
* Run migrations
* Add celery
* Figure out a financial vehicle source
* Add a celery task to identify dividend yielding stocks
* Run the task to seed the database
* Figure out a schema for dividend historical data
* Create a celery task to gather that data for each stock
* Define ReST operations to present that data to fe
* Figure out how to display that data in a meaningful way - start the vue fe
* Figure out how to value dgi stocks
* Figure out how to display best values
