# C3DataUtilities

* Read problem data
* Check problem data formatting and properties
* Read solution data
* Check solution data formatting and properties
* Evaluate solution to problem, i.e. constraint feasibility, objective value, properties of interest

# Installation

Currently relies on https://github.com/Smart-DS/Bid-DS-data-model. Follow installation directions in that repository, primarily:

```
cd Bid-DS-data-model
pip install -e .
```

The validators branch (https://github.com/Smart-DS/Bid-DS-data-model/tree/validators) contains the most complete set of input data checking, but this will later be merged into main. To use the validators branch, do:

```
cd Bid-DS-data-model
git checkout validators
```

# Checking problem data formatting and properties

Once Bid-DS-data-model is installed, one can do:

```
cd C3DataUtilities
python check_data.py <problem_data_file_name>
```

This will read a problem data file, check it against properties specified in the data format document and problem formulation document, and print out some information about the problem dimensions. If the data fails any of the required properties, an exception will be raised to report this. It is not guaranteed that all failed properties will be reported. If there is at least one failure, then it is guaranteed that at least one failure will be reported.

The operative parts of ```check_data.py``` for data checking are:

```
from datamodel.input.data import InputDataFile
```

and

```
problem_data = InputDataFile.load(problem_data_file_name)
```

and

```
check_data_model(problem_data)
```

```datamodel``` is the Python module contained in Bid-DS-data-model. If no errors are raised by ```InputDataFile.load```, then ```problem_data``` is a Pydantic model object containing the problem data, and no errors were found at read time. ```check_data_model``` performs additional data checks on the Pydantic model after reading the data file, and any errors it finds are also raised.

If a data file contains multiple errors of a given type or errors of multiple types, the data checker reports as many errors as possible, subject to simple coding. However, some errors can mask others, so it is not guaranteed that all errors will be reported. E.g., if any errors are found by ```InputDataFile.load``` then the subsequent call to ```check_data_model``` is skipped, so errors that would be found only by ```check_data_model``` will not be reported.
