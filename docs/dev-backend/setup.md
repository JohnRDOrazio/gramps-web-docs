This page lists the steps required to start developing [Gramps Web API](https://github.com/gramps-project/gramps-web-api/). It will be assumed that you are using Ubuntu Linux.

### Python version

The Web API requires Python 3.7 or newer. If you would like to use a version of Python that is not yet available in your version of Ubuntu, you can look into [this repository](https://launchpad.net/~deadsnakes/+archive/ubuntu/ppa) which will allow you to install newer versions. In this case you should also install the relative `-dev` and, if working with virtual environment, `-venv` packages. If for example you install **Python 3.12**:

```
sudo apt install python3.12 python3.12-dev python3.12-venv
```

### Clone the Web API repository

Clone the Web API to your PC (assuming you have set up an SSH key with Github) using

```
git clone git@github.com:gramps-project/gramps-web-api.git
cd gramps-web-api
```

### Install Gramps

The Web API requires the Gramps Python library to be importable. Starting from Gramps 5.2.0, it will be installable via `pip`. Development based on Gramps 5.1.x requires installation via the `apt` package on Ubuntu.

If you would like to work in a virtual environment (recommended), you can create it now, activate it, and install the `gramps` pip package:

```
python3.12 -m venv .venv
source .venv/bin/activate
pip install gramps
```

This will create a virtual environment called **gramps**.

If instead you prefer to work with the system Python interpreter

```
sudo apt install gramps
```

!!! info
    Note that using the `gramps` Python package from Gramps installed with `apt` **requires** using the system Python interpreter, so you **cannot** work in a virtual environment when using this package.


### Install prerequisites

To start development, please install the dependencies by running
```
pip3 install opencv-python
pip3 install -r requirements-dev.txt
```
!!! info
    When working within a virtual environment, **pip** will already use the correct version for the Python interpreter in the virtual environment, so you can just use `pip` instead of `pip3`.

### Install the library in editable mode

If working with the system Python interpreter, run
```
pip3 install -e . --user
```

If working within the virtual environment, there is no need for the `--user` flag

```
pip install -e .
```

### Set up pre-commit hooks

To set up the pre-commit hooks for the repository, run
```
pre-commit install
```
in the repository root. This will e.g. make sure that all source files are nicely formatted with `black`.

### Run tests

!!! info
    If your environment is not already setup for Gramps development, you may need to install a number of system packages for tests to pass:
    ```bash
    sudo apt update && apt install gettext appstream pkg-config libcairo2-dev gir1.2-gtk-3.0 libgirepository1.0-dev libicu-dev \
    libopencv-dev tesseract-ocr tesseract-ocr-all gir1.2-pango-1.0 graphviz gir1.2-gexiv2-0.10 gir1.2-osmgpsmap-1.0 \
    libpq-dev postgresql-client postgresql-client-common python3-psycopg2 libgl1-mesa-dev libgtk2.0-dev libatlas-base-dev \
    python3-opencv
    ```

To run the unit tests, run
```
pytest
```
in the repository root.

### Generate a configuration file

Example content:

```python
TREE="My Family Tree"
SECRET_KEY="not_secure_enough"
USER_DB_URI="sqlite:///users.sqlite"
```

!!! warning
    Do not use this configuration in production.

See [Configuration](../Configuration.md) for a full list of config options.

!!! warning
    Do not use your production database for development, but use a copy of it or the Gramps example database.


### Add users


You can add a user with owner permissions by running
```
python3 -m gramps_webapi --config path/to/config user add owner owner --role 4
```
This uses username and password `owner`.

!!! info
    When using a virtual environment, **python** will already be set to the version of python used in the virtual environment, so you can just use `python` instead of `python3`.

### Run the app in development mode


Run
```
python3 -m gramps_webapi --config path/to/config run
```
The API will be accesible at `http://127.0.0.1:5000` by default, which displays an empty page.  Access your Gramps data using the API described by [gramps-project.github.io/gramps-web-api](https://gramps-project.github.io/gramps-web-api/). For example, to show people go to `http://127.0.0.1:5000/api/people`

To choose a different port, add the `--port` option.
