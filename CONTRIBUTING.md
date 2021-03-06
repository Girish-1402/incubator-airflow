# Contributing

Contributions are welcome and are greatly appreciated! Every
little bit helps, and credit will always be given.


# Table of Contents
  * [TOC](#table-of-contents)
  * [Types of Contributions](#types-of-contributions)
      - [Report Bugs](#report-bugs)
      - [Fix Bugs](#fix-bugs)
      - [Implement Features](#implement-features)
      - [Improve Documentation](#improve-documentation)
      - [Submit Feedback](#submit-feedback)
  * [Documentation](#documentation)
  * [Development and Testing](#development-and-testing)
      - [Setting up a development environment](#setting-up-a-development-environment)
      - [Pull requests guidelines](#pull-request-guidelines)
      - [Testing Locally](#testing-locally)
  * [Changing the Metadata Database](#changing-the-metadata-database)


## Types of Contributions

### Report Bugs

Report bugs through [Apache Jira](https://issues.apache.org/jira/browse/AIRFLOW)

Please report relevant information and preferably code that exhibits
the problem.

### Fix Bugs

Look through the Jira issues for bugs. Anything is open to whoever wants
to implement it.

### Implement Features

Look through the [Apache Jira](https://issues.apache.org/jira/browse/AIRFLOW) for features. Any unassigned "Improvement" issue is open to whoever wants to implement it.

We've created the operators, hooks, macros and executors we needed, but we
made sure that this part of Airflow is extensible. New operators,
hooks, macros and executors are very welcomed!

### Improve Documentation

Airflow could always use better documentation,
whether as part of the official Airflow docs,
in docstrings, `docs/*.rst` or even on the web as blog posts or
articles.

### Submit Feedback

The best way to send feedback is to open an issue on [Apache Jira](https://issues.apache.org/jira/browse/AIRFLOW)

If you are proposing a feature:

-   Explain in detail how it would work.
-   Keep the scope as narrow as possible, to make it easier to
    implement.
-   Remember that this is a volunteer-driven project, and that
    contributions are welcome :)

## Documentation

The latest API documentation is usually available
[here](https://airflow.incubator.apache.org/). To generate a local version,
you need to have set up an Airflow development environment (see below). Also
install the `doc` extra.

    pip install -e .[doc]

Generate the documentation by running:

    cd docs && ./build.sh

Only a subset of the API reference documentation builds. Install additional
extras to build the full API reference.

## Development and Testing

### Set up a development env using Docker

Go to your Airflow directory and start a new docker container. You can choose between Python 2 or 3, whatever you prefer.

```
# Start docker in your Airflow directory
docker run -t -i -v `pwd`:/airflow/ python:2 bash

# Go to the Airflow directory
cd /airflow/

# Install Airflow with all the required dependencies,
# including the devel which will provide the development tools
pip install -e ".[hdfs,hive,druid,devel]"

# Init the database
airflow initdb

nosetests -v tests/hooks/test_druid_hook.py

  test_get_first_record (tests.hooks.test_druid_hook.TestDruidDbApiHook) ... ok
  test_get_records (tests.hooks.test_druid_hook.TestDruidDbApiHook) ... ok
  test_get_uri (tests.hooks.test_druid_hook.TestDruidDbApiHook) ... ok
  test_get_conn_url (tests.hooks.test_druid_hook.TestDruidHook) ... ok
  test_submit_gone_wrong (tests.hooks.test_druid_hook.TestDruidHook) ... ok
  test_submit_ok (tests.hooks.test_druid_hook.TestDruidHook) ... ok
  test_submit_timeout (tests.hooks.test_druid_hook.TestDruidHook) ... ok
  test_submit_unknown_response (tests.hooks.test_druid_hook.TestDruidHook) ... ok

  ----------------------------------------------------------------------
  Ran 8 tests in 3.036s

  OK
```

The Airflow code is mounted inside of the Docker container, so if you change something using your favorite IDE, you can directly test is in the container.

### Set up a development env using Virtualenv

Please install python(2.7.x or 3.4.x), mysql, and libxml by using system-level package
managers like yum, apt-get for Linux, or homebrew for Mac OS at first.
It is usually best to work in a virtualenv and tox. Install development requirements:

    cd $AIRFLOW_HOME
    virtualenv env
    source env/bin/activate
    pip install -e .[devel]
    tox

Feel free to customize based on the extras available in [setup.py](./setup.py)

### Pull Request Guidelines

Before you submit a pull request from your forked repo, check that it
meets these guidelines:

1. The pull request should include tests, either as doctests, unit tests, or
both. The airflow repo uses [Travis CI](https://travis-ci.org/apache/incubator-airflow)
to run the tests and [codecov](https://codecov.io/gh/apache/incubator-airflow)
to track coverage. You can set up both for free on your fork. It will
help you making sure you do not break the build with your PR and that you help
increase coverage.
2. Please [rebase your fork](http://stackoverflow.com/a/7244456/1110993),
squash commits, and resolve all conflicts.
3. Every pull request should have an associated
[JIRA](https://issues.apache.org/jira/browse/AIRFLOW/?selectedTab=com.atlassian.jira.jira-projects-plugin:summary-panel).
The JIRA link should also be contained in the PR description.
4. Preface your commit's subject & PR's title with **[AIRFLOW-XXX]**
where *XXX* is the JIRA number. We compose release notes (i.e. for Airflow releases) from all commit titles in a release.
By placing the JIRA number in the commit title and hence in the release notes,
Airflow users can look into JIRA and Github PRs for more details about a particular change.
5. Add an [Apache License](http://www.apache.org/legal/src-headers.html)
 header to all new files
6. If the pull request adds functionality, the docs should be updated as part
of the same PR. Doc string are often sufficient.  Make sure to follow the
Sphinx compatible standards.
7. The pull request should work for Python 2.7 and 3.4. If you need help
writing code that works in both Python 2 and 3, see the documentation at the
[Python-Future project](http://python-future.org) (the future package is an
Airflow requirement and should be used where possible).
8. As Airflow grows as a project, we try to enforce a more consistent
style and try to follow the Python community guidelines. We track this
using [landscape.io](https://landscape.io/github/apache/incubator-airflow/),
which you can setup on your fork as well to check before you submit your
PR. We currently enforce most [PEP8](https://www.python.org/dev/peps/pep-0008/)
and a few other linting rules. It is usually a good idea to lint locally
as well using [flake8](https://flake8.readthedocs.org/en/latest/)
using `flake8 airflow tests`. `git diff upstream/master -u -- "*.py" | flake8 --diff` will return any changed files in your branch that require linting.
9. Please read this excellent [article](http://chris.beams.io/posts/git-commit/) on
commit messages and adhere to them. It makes the lives of those who
come after you a lot easier.

### Testing locally

#### TL;DR
Tests can then be run with (see also the [Running unit tests](#running-unit-tests) section below):

    ./run_unit_tests.sh

Individual test files can be run with:

    nosetests [path to file]

#### Running unit tests

We *highly* recommend setting up [Travis CI](https://travis-ci.org/) on
your repo to automate this. It is free for open source projects. If for
some reason you cannot, you can use the steps below to run tests.

Here are loose guidelines on how to get your environment to run the unit tests.
We do understand that no one out there can run the full test suite since
Airflow is meant to connect to virtually any external system and that you most
likely have only a subset of these in your environment. You should run the
CoreTests and tests related to things you touched in your PR.

To set up a unit test environment, first take a look at `run_unit_tests.sh` and
understand that your ``AIRFLOW_CONFIG`` points to an alternate config file
while running the tests. You shouldn't have to alter this config file but
you may if need be.

From that point, you can actually export these same environment variables in
your shell, start an Airflow webserver ``airflow webserver -d`` and go and
configure your connection. Default connections that are used in the tests
should already have been created, you just need to point them to the systems
where you want your tests to run.

Once your unit test environment is setup, you should be able to simply run
``./run_unit_tests.sh`` at will.

For example, in order to just execute the "core" unit tests, run the following:

```
./run_unit_tests.sh tests.core:CoreTest -s --logging-level=DEBUG
```

or a single test method:

```
./run_unit_tests.sh tests.core:CoreTest.test_check_operators -s --logging-level=DEBUG
```

For more information on how to run a subset of the tests, take a look at the
nosetests docs.

See also the list of test classes and methods in `tests/core.py`.

### Changing the Metadata Database

When developing features the need may arise to persist information to the the
metadata database. Airflow has [Alembic](https://bitbucket.org/zzzeek/alembic)
built-in to handle all schema changes. Alembic must be installed on your
development machine before continuing.

```
# starting at the root of the project
$ pwd
~/airflow
# change to the airflow directory
$ cd airflow
$ alembic revision -m "add new field to db"
  Generating
~/airflow/airflow/migrations/versions/12341123_add_new_field_to_db.py
```

## Setting up the node / npm javascript environment (ONLY FOR www_rbac)

`airflow/www_rbac/` contains all npm-managed, front end assets.
Flask-Appbuilder itself comes bundled with jQuery and bootstrap.
While these may be phased out over time, these packages are currently not
managed with npm.

### Node/npm versions
Make sure you are using recent versions of node and npm. No problems have been found with node>=8.11.3 and npm>=6.1.3

### Using npm to generate bundled files

#### npm
First, npm must be available in your environment. If it is not you can run the following commands
(taken from [this source](https://gist.github.com/DanHerbert/9520689))
```
brew install node --without-npm
echo prefix=~/.npm-packages >> ~/.npmrc
curl -L https://www.npmjs.com/install.sh | sh
```

The final step is to add `~/.npm-packages/bin` to your `PATH` so commands you install globally are usable.
Add something like this to your `.bashrc` file, then `source ~/.bashrc` to reflect the change.
```
export PATH="$HOME/.npm-packages/bin:$PATH"
```

#### npm packages
To install third party libraries defined in `package.json`, run the
following within the `airflow/www_rbac/` directory which will install them in a
new `node_modules/` folder within `www_rbac/`.

```bash
# from the root of the repository, move to where our JS package.json lives
cd airflow/www_rbac/
# run npm install to fetch all the dependencies
npm install
```

To parse and generate bundled files for airflow, run either of the
following commands. The `dev` flag will keep the npm script running and
re-run it upon any changes within the assets directory.

```
# Compiles the production / optimized js & css
npm run prod

# Start a web server that manages and updates your assets as you modify them
npm run dev
```

#### Upgrading npm packages

Should you add or upgrade a npm package, which involves changing `package.json`, you'll need to re-run `npm install` 
and push the newly generated `package-lock.json` file so we get the reproducible build.

#### Javascript Style Guide

We try to enforce a more consistent style and try to follow the JS community guidelines. 
Once you add or modify any javascript code in the project, please make sure it follows the guidelines 
defined in [Airbnb JavaScript Style Guide](https://github.com/airbnb/javascript).
Apache Airflow uses [ESLint](https://eslint.org/) as a tool for identifying and reporting on patterns in JavaScript,
which can be used by running any of the following commands.

```bash
# Check JS code in .js and .html files, and report any errors/warnings
npm run lint

# Check JS code in .js and .html files, report any errors/warnings and fix them if possible 
npm run lint:fix
```
 
