# **Post-Class Self-Study: Optional Pandas Topics**

**Context:** This document covers advanced data manipulation techniques found in the course notebook. These topics are useful for specific data alignment and cleaning scenarios but were not covered in the main 3-hour session.

## **1\. Reindexing**

reindex is the fundamental data alignment method in pandas. It is used to create a new object with the data conformed to a new index.

### **Series Reindexing**

reindex can be used to rearrange the data according to the new index, introducing missing values if any index values were not already present.

```

import pandas as pd
import numpy as np

obj = pd.Series([4.5, 7.2, -5.3, 3.6], index=['d', 'b', 'a', 'c'])
print("Original Series:")
print(obj)

# Reindex with new labels
obj2 = obj.reindex(['a', 'b', 'c', 'd', 'e'])
print("\nReindexed Series:")
print(obj2)

```

### **DataFrame Reindexing**

You can reindex the (row) index or columns of a dataframe.

```

frame = pd.DataFrame(np.arange(9).reshape((3, 3)),
                     index=['a', 'c', 'd'],
                     columns=['Ohio', 'Texas', 'California'])

# Reindex rows
frame2 = frame.reindex(['a', 'b', 'c', 'd'])
print("Reindexed Rows:")
print(frame2)

# Reindex columns
states = ["Texas", "Utah", "California"]
print("\nReindexed Columns:")
print(frame.reindex(columns=states))

```

### **Interpolation**

The method option allows us to do interpolation or filling of values when reindexing. For example, we can fill the missing values with the last known value (forward fill). This is useful for time series data.

```

obj3 = pd.Series(["blue", "purple", "yellow"], index=[0, 2, 4])
print(obj3.reindex(np.arange(6), method="ffill"))

```

## **2\. Dropping Entries**

You can drop entries from an axis using the drop method. It will return a new object with the indicated value or values deleted from an axis.

### **Dropping from Series**

```

obj = pd.Series(np.arange(5.), index=["a", "b", "c", "d", "e"])
new_obj = obj.drop("c")
print(new_obj)

# Drop multiple items
print(obj.drop(["d", "c"]))

```

### **Dropping from DataFrame**

With DataFrame, index values can be deleted from either axis.

```

data = pd.DataFrame(np.arange(16).reshape((4, 4)),
                    index=["Ohio", "Colorado", "Utah", "New York"],
                    columns=["one", "two", "three", "four"])

# Drop rows (default axis=0)
print(data.drop(["Colorado", "Ohio"]))

# Drop columns (axis=1 or axis="columns")
print(data.drop("two", axis=1))

```

## **3\. Arithmetic and Data Alignment**

One of Pandas' most powerful features is how it handles arithmetic between objects with different indexes.

### **Data Alignment**

When adding two DataFrames, if index pairs are not the same, the respective index in the result will be the union of the index pairs.

```

df1 = pd.DataFrame(np.arange(9.).reshape((3, 3)), columns=list("bcd"),
                   index=["Ohio", "Texas", "Colorado"])

df2 = pd.DataFrame(np.arange(12.).reshape((4, 3)), columns=list("bde"),
                   index=["Utah", "Ohio", "Texas", "Oregon"])

# Adding them results in NaNs where labels don't overlap
print(df1 + df2)

```

### **Filling Values**

You can fill missing values with a special value (like 0\) when an axis label is found in one object but not the other using the .add method.

```

df1 = pd.DataFrame(np.arange(12.).reshape((3, 4)), columns=list("abcd"))
df2 = pd.DataFrame(np.arange(20.).reshape((4, 5)), columns=list("abcde"))
df2.loc[1, "b"] = np.nan

# Add with fill_value=0
# This substitutes 0 for any missing values in the operation
print(df1.add(df2, fill_value=0))

```

### **Combining Overlapping Data**

Sometimes you want to combine two datasets where values in one are preferred over the other. combine\_first allows you to patch missing data in one object with data from another.

```

df1 = pd.DataFrame({
    "A": [1.0, np.nan, 3.0, 5.0, np.nan],
    "B": [np.nan, 2.0, 3.0, np.nan, 6.0]
})

df2 = pd.DataFrame({
    "A": [5.0, 2.0, 4.0, np.nan, 3.0, 7.0],
    "B": [np.nan, np.nan, 3.0, 4.0, 6.0, 8.0],
})

# Patch missing values in df1 with values from df2
print(df1.combine_first(df2))

```
