# Backman Lab Work

## PWS Analysis Code
How does this PWS (Partial Wave Spectroscopy) data analysis code work? It starts with the following, using Python:
```python
from matplotlib import pyplot as plt
import pandas as pd
import seaborn as sns
import numpy as np
import statistics as stats
import os
```
This first step imports all necessary programs for function of code

To import necessary files for analysation of images:
```python
filenames = [("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/Control12hRep1.csv"), ("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/Control12hRep2.csv"), ("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/Control12hRep3.csv")]

for file in filenames:
    pwsDf = pd.read_csv(file)
```
- Note: As of now, direct path specification is necessary for proper code function. Will update later.

To organize all files into panda csv dataframes for easier work later:
```python
df1 = pd.read_csv("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/Control0hRep1.csv")
df2 = pd.read_csv("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/Control0hRep2.csv")
df3 = pd.read_csv("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/Control0hRep3.csv")
df4 = pd.read_csv("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/Control12hRep1.csv")
df5 = pd.read_csv("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/Control12hRep2.csv")
df6 = pd.read_csv("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/Control12hRep3.csv")
df7 = pd.read_csv("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/DXM0hRep1.csv")
df8 = pd.read_csv("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/DXM0hRep2.csv")
df9 = pd.read_csv("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/DXM0hRep3.csv")
df10 = pd.read_csv("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/DXM12hRep1.csv")
df11 = pd.read_csv("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/DXM12hRep2.csv")
df12 = pd.read_csv("/Users/estevegeorge/Documents/sites/NUWorkDocs/CelecoxibTreatmentExperiment/DXM12hRep3.csv")
```
- Note: See above note for path explanation

To use numpy and stats to analyze and print mean and standard deviation of previous files:
```python
mean_df4 = np.mean(df4["RMS"])
print("Mean of Control 12h Rep 1 = " + str(mean_df4))
stdev_df4 = stats.stdev(df4["RMS"])
print("Standard Deviation of Control 12h Rep 1 = " + str(stdev_df4))

mean_df5 = np.mean(df5["RMS"])
print("Mean of Control 12h Rep 2 = " + str(mean_df5))
stdev_df5 = stats.stdev(df5["RMS"].head())
print("Standard Deviation of Control 12h Rep 2 = " + str(stdev_df5))

mean_df6 = np.mean(df6["RMS"])
print("Mean of Control 12h Rep 3 = " + str(mean_df6))
stdev_df6 = stats.stdev(df6["RMS"].head())
print("Standard Deviation of Control 12h Rep 3 = " + str(stdev_df6))
```

In the same step, the following code can be used to organize the different testing conditions into dataframes for further easier work down the line:
```python
df_control_zeroh = (df1+ df2 + df3)
df_control_twelveh = (df4 + df5 + df6)
df_drug_zeroh = (df7 + df8 + df9)
df_drug_twelveh = (df10 + df11 + df12)
```

The following step concatenates the previous dataframes with their respective RMS values and renames the concatenations(?) as "RMS"
```python
RMS = np.concatenate([df_control_zeroh["RMS"], df_drug_zeroh["RMS"], df_control_twelveh["RMS"], df_drug_twelveh["RMS"]])
```
To organize the data into plottable pyplot figures, the following step is necessary:
```python
PWSData = pd.DataFrame(columns = ['Rep','RMS'])
rep = np.concatenate((np.repeat(1,len(df_control_zeroh['RMS'].values)),np.repeat(2,len(df_control_twelveh['RMS'].values)),np.repeat(3,len(df_drug_zeroh['RMS'].values)),np.repeat(4,len(df_drug_twelveh['RMS'].values))),axis=0)
PWSData['RMS'] = RMS
PWSData['Rep'] = rep
PWSData_no_nan = PWSData.dropna()
```
- Please note: The "RMS" step and the last step can be put into the same "run" box (I don't know what it's called)

The following code plots the previously organized data into a **violin plot** using pyplot. 
```python
ax = sns.violinplot(x='Rep',y='RMS',data=PWSData_no_nan)

ax.set_title("Effect of DXM Treatment on RMS")
plt.xlabel("Conditions")
plt.ylabel("RMS")

labels = [item.get_text() for item in ax.get_xticklabels()]
labels[0] = 'Control 0h'
labels[1] = 'DXM 0h'
labels[2] = 'Control 12h'
labels[3] = 'DXM 12h'

ax.set_xticklabels(labels)

plt.savefig("Effect of DXM Treatment on RMS Violin Best")
```
- To substitute the **violin plot** for a *different* type of plot, such as a **box plot** or **point plot**, substitute "sns.violinplot" with "sns.boxplot" or "sns.pointplot"
