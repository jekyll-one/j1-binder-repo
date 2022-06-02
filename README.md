# Binder Repo example for the J1 NBI module

Configuration settings and all notebooks used by the J1 NBI Integration
based on a requirements file `requirements.txt`.

## apt.txt

Added the base **OS package** for `ffmeg` to make it usable in Jupyter
Notebooks for e.g `matplotlib` animations.

### NB config

#### matplotlib animation

The method (of the animation instances) to manage the player is controlled
by the `animation` rc parameter.

The rc parameter currently supports values of "none", "html5"
and "jshtml".

*  none:    no player (display) is shown    
*  html5:   use the native HTML5 player widget
*  jshtml:  use the interactive JavaScript widget

The default is none to not display a player. To display the native HTML5
player, # set it to `html5`. For the interactive JavaScript widget to
`jshtml`.

```
rc('animation', html='html5', embed_limit='512')
# rc('animation', html='jshtml', embed_limit='512')
```

**NOTE**: The JavaScript widgets referenced to **jshtml** does currently
**not** work.


## Python environment with a requirements.txt

[![Binder](http://mybinder.org/badge_logo.svg)](http://mybinder.org/v2/gh/jekyll-one/j1-binder-requirements-example/main)

A Binder-compatible repo with a `requirements.txt` file. Access this Binder
at the following URL

http://mybinder.org/v2/gh/jekyll-one/j1-binder-requirements-example/main

### Notes

The `requirements.txt` file should list **all** Python libraries that your
notebooks depend on, and they will be installed (for the resulting Docker
image) using:

```bash
pip install -r requirements.txt
```

The base Binder image contains **no** extra dependencies, so be as explici
as possible in defining the packages that you need. This includes specifying
**explicit** versions wherever possible.

If you do specify strict versions, it is important to do so for **all**
your dependencies, not just direct dependencies. Strictly specifying only
some dependencies is a recipe for environments breaking over time.

[pip-compile](https://github.com/jazzband/pip-tools/) is a handy tool for
combining loosely specified dependencies with a fully frozen environment.
You write a requirements.in with just the dependencies you need and pip-compile
will generate a `requirements.txt` with all the strict packages and versions
that would come from installing that package right now. That way, you only
need to specify what you actually know you need, but you also get a snapshot
of your environment.


#### Install via pip

pip install pip-tools

#### Create a requirements file  `requirements.txt`

First, create a file `requirements.in` that contails all Python libraries
required to run your notebooks like so:

```
# Required Python packages. This file is used by pip-compile to generate the
# requirements file "requirements.txt" to create a Binder-compatible repo.
#
# NOTE: This file should list all Python libraries that your notebooks depend
# ------------------------------------------------------------------------------
altair==4.2.0
bokeh==2.4.3
bqplot==0.11.0
datascience==0.17.2
ffmpeg==1.4
ipywidgets==7.6.3
matplotlib==3.4.2
matplotlib-inline==0.1.2
networkx==2.6.3
numpy==1.20.3
pandas==1.2.4
plotly==4.14.3
scikit-learn==0.24.2
scipy==1.6.3
seaborn==0.11.1
sklearn==0.0
statsmodels==0.12.2
vega-datasets==0.9.0
zipp==3.4.0
```

**NOTE**: The following packages are considered to be unsafe in a
requirements file: `pip`, `setuptools`, etc. Typically some packages are
installed (at least `pip`) for Python but excluded and **no** versions for
those packages are generated. See related comments at the end of that file:

```
# The following packages are considered to be unsafe in a requirements file:
# pip
# setuptools
```

Run:

```bash
pip-compile --output-file requirements.txt
```
in your Binder repo folder the input file `requirements.in` resides.

**NOTE**: If you are on `Windows`, remove all `Win32`-related libraries
from the resulting file `requirements.txt` as they are **not** needed
for a Binder build targetting an `Linux` OS like:
```
pywin32==304
    # via jupyter-core
pywinpty==2.0.5
    # via terminado
```

**NOTE**: Invalid or incompatible packages will break the build process at
Binder and cause unforeseeable results for the resulting Docker image.
