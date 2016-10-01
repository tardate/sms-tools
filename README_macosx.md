# sms-tools on MacOSX

OSX? "which we do not support but that should work"

Nevertheless, here are some guides for running on MacOSX. You mileage may vary.


## Using a Virtual Envirpnment (virtualenv)

(this works particularly well for MacOSX users)

Let's assume you aready have [virtualenv installed](https://virtualenv.pypa.io/en/stable/installation/). The purpose of virtualenv is to isolate software installations to their own "virtual environment",  which can help avoid dependency conflicts and environment-related bugs.

It seems (so far) the sms-tools runs quite happily with virtualenv.

However, special considerations are required due to restrictions of the matplotlib library used in the python code - see [Working with Matplotlib in Virtual environments](http://matplotlib.org/faq/virtualenv_faq.html).

The simplest technique that seems to work, is to *not* activate the virtual environment while using `sms-tools`, but instead point the `PYTHONHOME` path to the virtual environment folder.


###  Virtual Envirpnment Setup

First, choose a place to create the virtualenv folder.  One level above the sms-tools folder is a good idea, and `venv` is as good a name as any.

Create the venv folder, activate it and make sure it has an up-to-date pip installed (not required, but generally a good idea):

```
$ virtualenv venv
$ source venv/bin/activate
(venv)$ pip install --upgrade pip
```

Install the sms-tools dependencies (from `requirements.txt`), and compile the `utilFunctions_C`:

```
(venv)$ pip install sms-tools/requirements.txt
(venv)$ cd sms-tools/software/models/utilFunctions_C
(venv)$ python compileModule.py build_ext --inplace
(venv)$ cd ../../..
```


### Using the PYTHONHOME approach

Note the critical step: `deactivate` the virtualenv, and set the `PYTHONHOME` environment variable to point to `venv` folder (absolute path)

```
(venv)$ deactivate
$ export PYTHONHOME=$(cd venv; pwd)
# or set it explicitly:
$ export PYTHONHOME=/some/path/to/venv
```

Now test it out with `models_GUI.py`.

```
$ cd $PYTHONHOME/../sms-tools/software/models_interface
$ python models_GUI.py
```
.. and the sms-tools models GUI should pop up and be fully functional.

Note: if there is an error, check to make sure the version of `models_GUI.py` contains the fix for [Matplotlib crashing on Mac OS X](https://github.com/MTG/sms-tools/issues/36)

Testing it with ipython, which needs to be invoked via python using this approach:

```
$ python -m IPython
Python 2.7.11 (default, Dec 26 2015, 17:47:15)
Type "copyright", "credits" or "license" for more information.

IPython 5.1.0 -- An enhanced Interactive Python.
?         -> Introduction and overview of IPython's features.
%quickref -> Quick reference.
help      -> Python's own help system.
object?   -> Details about 'object', use 'object??' for extra details.

In [1]: from scipy.io.wavfile import read
   ...: (fs, x) = read('sounds/flute-A4.wav')
   ...: print('length: {}'.format(x.size/float(fs)))
   ...: import matplotlib.pyplot as plt
   ...: plt.plot(x)
   ...: plt.show()
   ...:
length: 2.14972789116
```

The waveform plot opens in a window.
