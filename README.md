# jupyter_msal_widget

[![Build Status](https://travis-ci.com/jupyter_msal_widget.svg?branch=master)](https://travis-ci.c/jupyter_msal_widget)
[![codecov](https://codecov.io/gh/jupyter_msal_widget/branch/master/graph/badge.svg)](https://codecov.io/gh/jupyter_msal_widget)

A Custom Jupyter Widget for MSAL authentication.

## Installation

You can install using `pip`:

```bash
pip install jupyter_msal_widget
```

If you are using Jupyter Notebook 5.2 or earlier, you may also need to enable
the nbextension:

```bash
jupyter nbextension enable --py [--sys-prefix|--user|--system] jupyter_msal_widget
```

## Development Installation

Create a dev environment:

```bash
conda create -n jupyter_msal_widget-dev -c conda-forge nodejs yarn python jupyterlab
conda activate jupyter_msal_widget-dev
```

Install the python. This will also build the TS package.

```bash
pip install -e ".[test, examples]"
```

When developing your extensions, you need to manually enable your extensions with the
notebook / lab frontend. For lab, this is done by the command:

```
jupyter labextension develop --overwrite .
yarn run build
```

For classic notebook, you need to run:

```
jupyter nbextension install --sys-prefix --symlink --overwrite --py jupyter_msal_widget
jupyter nbextension enable --sys-prefix --py jupyter_msal_widget
```

Note that the `--symlink` flag doesn't work on Windows, so you will here have to run
the `install` command every time that you rebuild your extension. For certain installations
you might also need another flag instead of `--sys-prefix`, but we won't cover the meaning
of those flags here.

### How to see your changes

#### Typescript:

If you use JupyterLab to develop then you can watch the source directory and run JupyterLab at the same time in different
terminals to watch for changes in the extension's source and automatically rebuild the widget.

```bash
# Watch the source directory in one terminal, automatically rebuilding when needed
yarn run watch
# Run JupyterLab in another terminal
jupyter lab
```

After a change wait for the build to finish and then refresh your browser and the changes should take effect.

#### Python:

If you make a change to the python code then you will need to restart the notebook kernel to have it take effect.

## Deployment

### Versioning

Change version in jupyter_msal_widget/\_version.py (and package.json)

**Erase the /dist directory before each deployment proccess**

### NPM

This requires a .npmrc at your user home directory with password/token

```
//registry.npmjs.org/:_authToken=<token_for_npm>
```

```bash
# Erase the /dist directory if not empty

# To generate the /dist folder
npm run build:prod

# Deploy it to NPM
npm publish
```

### Pip (testpypi & pypi)

This requires a .pypirc at your user home directory with password/token for both repositories

```
[pypi]
username = __token__
password = <token_for_pypi>

[testpypi]
username = __token__
password = <token_for_testpypi>
```

```bash
# Erase the /dist directory if not empty

# If twine is not yet installed
pip install twine

# To generate /dist folder
python setup.py sdist

# Uploads the current /dist to testpypi
twine upload --repository testpypi dist/*

# Test the uploaded component
pip install -i https://test.pypi.org/simple/ jupyter-msal-widget

# If everything looks fine, deploy to pipy
twine upload --repository pypi dist/*
```
