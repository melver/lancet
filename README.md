# Lancet

**A tool to help you launch simulations, organise the output and dissect the results.**

```bash
git clone git://github.com/jlstevens/lancet.git
```
In common with many projects on [IOAM](https://github.com/ioam), Lancet uses [param](https://github.com/ioam/param) so make sure to install it with ```pip install param```.

## Introduction

Lancet is designed to help you organise the output of your research tools, store it and dissect the data you have collected. The output of a single simulation or analysis rarely contains all the data you need, Lancet helps you generate data from many runs and analyse it using your own Python code.

Parameter spaces often need to be explored for the purpose of plotting, tuning or analysis. Lancet helps you extract the information you care about from potentially enormous volumes of data generated by such parameter exploration.

## Features

* A simple, useful core with advanced functionality strictly optional. Use what you need without learning about all components.

* Topographica components and core actively being used for research.

* Succinctly express the high dimensional parameter spaces you wish to explore without nested loops:

```python
>>> from lancet import LinearArgs
>>> pspace = ( LinearArgs('exc',1, 20, steps=100)
             * LinearArgs('inh',-10, -20, steps=100)  # Uses * operator for the Cartesian Product
             * LinearArgs('gNa',0.05, 0.10,  steps=10) )
>>> len(pspace) # 3-dimensional parameter space of 100000 points.
100000 
```

* Review your chosen settings *before* running hundreds of jobs:

```python
>>> for kwargs in pspace(log_file='mylog'):
...:   slow_simulation_and_analysis(**kwargs)

0: exc=1.0, inh=-10.0, gNa=0.05
1: exc=1.0, inh=-10.0, gNa=0.0556
...
99998: exc=20.0, inh=-20.0, gNa=0.0944
99999: exc=20.0, inh=-20.0, gNa=0.1
Continue? [y, N]:
```

* Automatically log your work to file, serialised as JSON:

```$ less mylog```
```json
0 {"gNa": 0.050000000000000003, "inh": -10.0, "exc": 1.0}
1 {"gNa": 0.055555555555555559, "inh": -10.0, "exc": 1.0}
2 {"gNa": 0.061111111111111116, "inh": -10.0, "exc": 1.0}
...
```
  
* Easily use external tools outside of Python to launch hundreds of concurrent simulations, locally or on a cluster (Grid Engine). Quickly switch between working locally and working on a cluster.

```python
from lancet import Launcher, QLauncher

cluster = True  # False to run locally
Launcher = QLauncher if cluster else Launcher
```

* Get the appropriate review for your chosen workflow, according to the components you chose, keeping your output in one place:

```python
from lancet import review_and_launch

@review_and_launch(Launcher, output_directory='./factors', review=True)
def model_analysis():
  # pspace = LinearArgs('exc',1, 20, 10) * LinearArgs('inh',-10, -20, 10)
  " Set up your launch settings and the tool you want to use here "
  ... 
  return launcher
```

<!-- Consider using factor N as an example -->

* Core classes designed to be extended to match your workflow. Optional components available for tight integration with the [Topographica simulator](https://github.com/ioam/topographica). These allow you to analyse your model both within simulations as they run and across simulations once all the data is collected.

* Ongoing work on (optional) tools for easily storing your data using HDF5 (using [PyTables](http://www.pytables.org/)), automated version control and analysis.

<!--
## Background

Python has gained significant traction in the research community as a way of prototyping ideas and succinctly expressing simulations and analysis in a free, open language. To illustrate, Python has rapidly gained popularity in neuroinformatics and computational neuroscience with journals publishing dedicating issues on the use of the language [Frontiers in Neuroinformatics](http://www.frontiersin.org/neuroinformatics/researchtopics/Python_in_neuroscience/8). A selection of neuroscientific tools using Python can be found at [neuralensemble.org](http://neuralensemble.org/) and lancet itself was originally written as part of the [Topographica neural simulator](https://github.com/ioam/topographica).
-->

## Contributors

[Marco Elver](https://github.com/melver) <me [at] marcoelver.com>: Python 3 fork, cleaned up many aspects of the design.