---
title: "Getting interactive help in Python"
description: "meta description"
image: "images/post/getting-help-in-python.png"
date: 2022-02-09T14:00:00+02:00
categories: ["python"]
type: "regular" # available types: [featured/regular]
draft: true
mermaid: true
---

Python is a really friendly language for beginners, but might get a bit complex once you go beyond the basics. In this post, I will compile some ways to get help while programming in Python.

#### Quick searching for keywords in Google taking you to Stack Overflow

One of the most common ways to find solutions to your current problem is to google it. Usually the top result comes from Stack Overflow. No surprises here.

You can search for the problem you are facing and add `+site:stackoverflow.com` to limit your results to StackOverflow domain.

#### Built-in help system

Python includes a built-in help system that you can call to find details about functions, objects, etc. The main challenge is that you need to know already what you are looking for.

For example, if you want to know more about the pivot_table() function in pandas, you need to do in you terminal:

```python
>>> import pandas as pd
>>> help(pd.pivot_table)
```

You will get this very comprehensive output:

```console
Help on function pivot_table in module pandas.core.reshape.pivot:

pivot_table(data: 'DataFrame', values=None, index=None, columns=None, aggfunc: 'AggFuncType' = 'mean', fill_value=None, margins=False, dropna=True, margins_name='All', observed=False, sort=True) -> 'DataFrame'
    Create a spreadsheet-style pivot table as a DataFrame.

    The levels in the pivot table will be stored in MultiIndex objects
    (hierarchical indexes) on the index and columns of the result DataFrame.

    Parameters
    ----------
    data : DataFrame
    values : column to aggregate, optional
    index : column, Grouper, array, or list of the previous
        If an array is passed, it must be the same length as the data. The
        list can contain any of the other types (except list).
        Keys to group by on the pivot table index.  If an array is passed,
        it is being used as the same manner as column values.
    columns : column, Grouper, array, or list of the previous
        If an array is passed, it must be the same length as the data. The
        list can contain any of the other types (except list).
        Keys to group by on the pivot table column.  If an array is passed,
        it is being used as the same manner as column values.
    aggfunc : function, list of functions, dict, default numpy.mean
        If list of functions passed, the resulting pivot table will have
        hierarchical columns whose top level are the function names
        (inferred from the function objects themselves)
        If dict is passed, the key is column to aggregate and value
        is function or list of functions.
    fill_value : scalar, default None
        Value to replace missing values with (in the resulting pivot table,
        after aggregation).
    margins : bool, default False
        Add all row / columns (e.g. for subtotal / grand totals).
    dropna : bool, default True
        Do not include columns whose entries are all NaN.
    margins_name : str, default 'All'
        Name of the row / column that will contain the totals
        when margins is True.
    observed : bool, default False
        This only applies if any of the groupers are Categoricals.
        If True: only show observed values for categorical groupers.
        If False: show all values for categorical groupers.

        .. versionchanged:: 0.25.0

    sort : bool, default True
        Specifies if the result should be sorted.

        .. versionadded:: 1.3.0

    Returns
    -------
    DataFrame
        An Excel style pivot table.

    See Also
    --------
    DataFrame.pivot : Pivot without aggregation that can handle
        non-numeric data.
    DataFrame.melt: Unpivot a DataFrame from wide to long format,
        optionally leaving identifiers set.
    wide_to_long : Wide panel to long format. Less flexible but more
        user-friendly than melt.
```

Exploring the `versionchanged` and `versionadded` parameters at the official Python [documentation guide](https://devguide.python.org/documenting/), it seems that the observed parameter changed in version 0.25.0 and sort was added on version 1.3.0 of pandas.

#### Quick searching for functions on the official documentation of libraries

If we search online for the function plus the library name, usually we will get the official documentation as the first result. For example, if we search for `pandas pivot_table` we will get a link to the [function documentation](https://pandas.pydata.org/docs/reference/api/pandas.pivot_table.html).

One advantage of the official docs compared to the built-in help is that they include additional examples or pointers to relevant guides.

![Pandas library documentation](pivot-table-documentation-pandas-website.png)

#### Using your IDE to search for information about functions and current variables

[//]: # "TODO: Sección: Describir como se busca info en Spyder y vscode"

Spyder is a multi-platform open source data science IDE that comes bundled with Anaconda. It offers support out of the box to ![get contextual help using Ctrl+I](images/post/getting-interactive-help-in-python/get-contextual-help-shortcut.png). This is the 
![output of the read_csv function](images/post/getting-interactive-help-in-python/output-of-read-csv-funtion.png)

Depending on your IDE, it might include certain features to make it easier to write code.

#### Conclusion

There are several quick ways to search for Python documentation depending on your context. I still need to explore some of the new AI powered engines like Github Copilot or Kite, but for the moment these are the tools I use the most.
