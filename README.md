# pypackage-template

This is a template project to be used as a standard, for any Python python package.

## Repository initialization 

The template needs to be first initialised after creation using the script `bin/init_repo.sh`. 
The script will ask the user: 
    * the name to be used for the package 
    * the name to be used for the source folder
    * a short description of the project
    * the GCP project associated to the python project (if any)
The script will automatically consider the Python version in the current environment (Python versions greater than 
3.6 are supported) and take care of adapting configurations files according to input values and Python version.

**NOTE:** In the process, you will also be asked if you want to *remove the templates*, 
in order to keep your repo minimal and avoid cluttering. 
Please keep in mind that if you do so, you won't be able to re-initialise the files 
in your repository again. 

## Makefile 

The process of installation of requirements, packaging, running checks (static typing, linting, unittests, etc) is 
done using Makefile. In order to setup and environment that compile and then install all requirements 
we can simply issue the following commands

```
make setup    # to install requirments
make dist     # to build the package
make install  # to install package
make checks   # to run all checks
```

For further information on the make commands, type

```
make help
```


## Requirements

This project uses ``pip-tools`` to keep track of requirements. In particular there is a ``requirements`` folder 
containing a ``requirements.in, requirements.txt, requirements_ci.in, requirements_ci.txt`` files corresponding to 
input (``*.in``) and actual (``*.txt``) requirements files for CI and prod environments.


## Toolset for the project

### Coding style
This project uses ``black`` (https://github.com/psf/black) for formatting and enforcing a coding style.
Run ``make format`` to reformat all source code and tests files to adhere to PEP8 standards.

### Linting
We use ``flake8`` (https://github.com/PyCQA/flake8) for static code analysis. The configuration file is included in ``setup.cfg``. 
It coincides with the configuration suggested by the ``black`` developers. Run ``make lint`` to analyse all source code and tests files.

### Type hints
We use ``mypy`` for static type checking. The configuration is included in ``setup.cfg``.
The only settings included in this configuration files are related to the missing typing annotations of some common third party libraries.

### Documentation
This project uses `Sphinx` (https://www.sphinx-doc.org/en/master/) as a tool to create documentation. Run `make docs` to build documentation.
There is a github workflow setup to publish documentation on the repo's Github Page at every push on `main` branch. 
To let this action run smoothly there must exist the `gh_pages`branch and the Github Page must be manually setted (from
github repo web interface > Settings > Pages) to use `gh_pages` as source branch and `/root` as source folder. 
Since this action requires a GITHUB_TOKEN, for its first run in the repo it will be necessary to follow the steps 
detailed [here]( https://github.com/peaceiris/actions-gh-pages#%EF%B8%8F-first-deployment-with-github_token) 
to make the action run fine from then on.

### Semantic Versioning

The present repository had already set up the versioneer using the procedure described below. The configuration is included in ``pyproject.toml``.  
 

#### Verisioneer setup
The following procedure had already been run and is reported only for documentation purposes, no need to re-run it.
1. First of all we need to install the versioneer package from pip
```
$ pip install versioneer
```

2. Then we edit the versioneer section in the ``pyproject.toml``

```
[tool.versioneer]
VCS = git
style = pep440
versionfile_source = {{SRC}}/_version.py
versionfile_build = {{SRC}}/_version.py
tag_prefix = "v"
parentdir_prefix = "{{PROJECT_NAME}}"
```

3. Next, we need to install the versioneer module into our project.
```
versioneer install
```

Running this command will create a few new files for us in our project and also ask us to make some changes to our 
setup.py file. 
We need to import versioneer in the setup, replace the version keyword argument with versioneer.get_version() and add a 
new argument cmdclass=versioneer.get_cmdclass().

```
setuptools.setup(
    ...
    version=versioneer.get_version(),
    cmdclass=versioneer.get_cmdclass(),
    ...
)
```
 for full documentation [versioneer GitHub repo](https://github.com/python-versioneer/python-versioneer)