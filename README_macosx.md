# sms-tools on MacOSX

OSX? "which we do not support but that should work"

Nevertheless, here are some guides for running on MacOSX. You mileage may vary.


## Using a Virtual Envirpnment (virtualenv)

(this works particularly well for MacOSX users)

Let's assume you aready have [virtualenv installed](https://virtualenv.pypa.io/en/stable/installation/). The purpose of virtualenv is to isolate software installations to their own "virtual environment",  which can help avoid dependency conflicts and environment-related bugs.

It seems (so far) the sms-tools runs quite happily with virtualenv.

However, special considerations are required due to restrictions of the matplotlib library used in the python code - see [Working with Matplotlib in Virtual environments](http://matplotlib.org/faq/virtualenv_faq.html).

The simplest technique that seems to work, is to *not* activate the virtual environment while using `sms-tools`, but instead point the `PYTHONHOME` path to the virtual environment folder.

### Setup - PYTHONHOME approach

First, choose a place to create the virtualenv folder.  One level above the sms-tools folder is a good idea, and `venv` is as good a name as any.

Create the venv folder, activate it and make sure it has an up-to-date pip installed (not required, but generally a good idea):

```
$ virtualenv venv
$ source venv/bin/activate
(venv)$ pip install --upgrade pip
(venv)$ deactivate
```

Note the critical step: `deactivate` the virtualenv.

Instead, set the `PYTHONHOME` environment variable to point to `venv` (absolute path)

```
$ export PYTHONHOME=$(cd venv; pwd)
# or set it explicitly:
$ export PYTHONHOME=/some/path/to/venv
```

Install the sms-tools dependencies (from `requirements.txt`), and compile the `utilFunctions_C`:

```
$ cd sms-tools
$ pip install -r $PYTHONHOME/../sms-tools/requirements.txt
$ cd $PYTHONHOME/../sms-tools/software/models/utilFunctions_C
$ python compileModule.py build_ext --inplace
```

Now test it out with `models_GUI.py`.

```
$ cd $PYTHONHOME/../sms-tools/software/models_interface
$ python models_GUI.py
```
.. and the sms-tools models GUI should pop up and be fully functional.

Note: if there is an error, check to make sure the version of `models_GUI.py` contains the fix for [Matplotlib crashing on Mac OS X](https://github.com/MTG/sms-tools/issues/36)
