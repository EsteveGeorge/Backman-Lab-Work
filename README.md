# Backman Lab Work

Just added the code. Will update later.

```python
from matplotlib import pyplot as plt
import pandas as pd
import seaborn as sns
import numpy as np
import statistics as stats
import os
```
Imports all necessary programs for function of code

To import necessary files for analysation of code:
```python
filenames = [("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/Control12hRep1.csv"), ("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/Control12hRep2.csv"), ("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/Control12hRep3.csv")]

for file in filenames:
    pwsDf = pd.read_csv(file)
```
- Note: As of now, direct path specification is necessary for proper code function. Will update later.
