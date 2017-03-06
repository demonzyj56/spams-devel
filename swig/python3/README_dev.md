# Build SPAMS 2.6 and python3.x

This file describes how to build spams-2.6 for python with python3 and the different tools available (at the end)

----------------------------------------
## Building from sources (you need SWIG and perl):
```
./mkdist -py3
cd dist/spams-python3
inst=$HOME/python3
python3 setup.py install --prefix=$inst
```

You will find in `$inst`:
* `doc/`: a copy of the pdf documentation of spams
* `extdata/`: 2 images used by the tests of dictLearn toolbox
* `test/`: a bunch of test programs where you can find examples of usage of each function of the toolbox

----------------------------------------
## Running test programs :
```
PYV=`python3 -c "import sys;t='{v[0]}.{v[1]}'.format(v=list(sys.version_info[:2]));sys.stdout.write(t)";` # get python current version
export PYTHONPATH=$inst/lib/python${PYV}/site-packages
python3 test_spams.py -h # to get help
python3 test_spams.py  # will run all the tests
python3 test_spams.py linalg # test of linalg functions
python3 test_spams.py name1 name2 ... # run named tests
```

========================================

**This part is for developpers only.**

----------------------------------------
## Interface building for test :
```
./mkpy spams
```
* input: spams.h, spams.i, spamstools.i, py_typemaps.i, numpy.i
* output : _spams_wrap.so spams_wrap.py

**NB:** the script mkpy is for linux, it must be modified (see comments) for MacOS

Normal build (with setup)
```
mkdir inst
swig -c++ -py3 -python -o spams_wrap.cpp spams.i
python3 setup.py install --prefix=inst
```

--------------------
## Tests:
```
cd $inst/test
python3 test_spams.py -h # to get help
python3 test_spams.py  # will run all the tests
python3 test_spams.py linalg # test of linalg functions
python3 test_spams.py name1 name2 ... # run named tests
```

==================================================

## Tools:

* `./mkdist [-h][-f]`<br/>
  make source distribution in `./dist`<br/>
  -f (fast): avoid remaking the doc if it already exists
  -py3 : for python3 compatibility (to do in swig/python3 directory)

* `../mkdoc [-h] [-s] [-t]`<br/>
  make the doc in `./tmp-doc`, except if this directory already exists<br/>
  -s: only sphinx doc; the doc is first injected in `spams.py` from the matlab doc fules and then formatted by sphinx<br/>
  -t: only LaTeX doc; it is the python version from Julien's doc version (doc_spams.tex)

* `./docmatlab2python`<br/>
  script used by `../mkdoc` to extract the doc from the matlab files and format it for sphinx or LaTeX

* `./mkpy [-h] [-cl] [-ns][-g][-D] spams`<br/>
  make the python module on-site<br/>
  -cl: clean previously generated files<br/>
  -ns (noswig): not run swig<br/>
  -g: compile with -g option<br/>
  -D: compile with -DDEBUG<br/>
  -py3 : for python3 compatibility (to do in swig/python3 directory)

* `./clean`<br/>
  Remove all intermediate files created by the different tool scripts

* `./mtopy <in > out`<br/>
  Help for translation matlab -> python<br/>
  /!\\ Partial translation (comment, print)

* `../conv-matlab-array [-r|-p] < data`<br/>
  convert a tabular as printed by matlab in the format readable by python (`-p`) or R (`-r`)

* `../doc2gforge`<br/>
  install the doc (from `./tmp-doc`) on gforge.inria.fr, in `/home/groups/spams-devel/htdocs/python`

* `./win-build.sh`<br/>
  to be run in a command window MinGW on Windows to make a windows binary package (.exe)<br/>
  The package will be 32 or 64bits depending on the system.
  a executer dans une fenetre de commande MinGW sous windows pour

Build details are described in [README_spams2.6_python3.x.md](README_spams2.6_python3.x.md)
