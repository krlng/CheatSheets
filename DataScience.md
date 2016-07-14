
#Data Science

##Datasets

* [Usa.gov](https://github.com/usagov/1.USA.gov-Data)

* [Movielens](http://grouplens.org/datasets/movielens/) - 1M Ratings on movie with user information

* [Baby Names in the US](https://www.ssa.gov/oact/babynames/limits.html)

## Pandas

### Dataframe

```python
df = DataFrame(data, columns=['year', 'state', 'pop'], index=['a', 'b', 'd'])  # Define Dataframe with columns and indices using dictionary of equal length
df['state'] = df.state          # select a column
df.ix['three']                  # select row 
frame2['pop'] = np.arange(5.)   # assign values to column
del df['pop']                   # delete a column
df.T                            # transpose df
df.values # returns data in a 2D ndarray

pop = {'Nevada': {2001: 2.4, 2002: 2.9}, 'Ohio': {2000: 1.5, 2002: 3.6}} # Create nested dict as datasource
df2 = DataFrame(pop)                 # uses years as indices, and assigns values if present

df2.ix[['1999', '2001', '2002'],  ['Nevada', 'California']] # reindex both axis, using NAs for 1st row and 2nd column
df2[:2]                              # only use first two rows
df2[df2['Nevada'] > 5]               # Dataframe where value in nevada column is bigger than 5
df2 < 5                              # shows df with True for values smaller than 5, otherwise False
df.sort_index(by=['Nevada', 'Ohio']) # Sort by columns, first Nevada than Ohio

f = lambda x: x.max()-x.min()
df.apply(f)                      # Apply function to each row
format = lambda x: '%.2f' % x
df.applymap(format)              # Element-wise application of a function



```



```python
import pandas as pd
import matplotlib.pyplot as plt
from datetime import datetime
dateparse = lambda x: datetime.strptime(x, ""%Y-%m-%d")
df = pd.read_csv("query_result.csv", parse_dates=["p_daystring_utc"], date_parser=dateparse)
df.columns=["country","date","count"]
plt.plot(df)

```