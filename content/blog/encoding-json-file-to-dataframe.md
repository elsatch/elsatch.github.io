---
title: "Encoding order matters when converting JSON files into a dataframe"
description: "meta description"
image: "images/post/encoding-json-file-to-dataframe/encoding-json-file-to-dataframe.png"
date: 2022-02-21T15:30:00+02:00
categories: ["python", "data ingestion","data engineering"]
type: "regular" # available types: [featured/regular]
draft: false
mermaid: false
---
This week I started researching on [Fablabs.io](Fablabs.io) huge dataset of Fablab spaces all around the world. The website offers an API and also a direct link to download all records. It's time to load it and start the analysis!

#### Error loading the file because of the encoding

All lab information is stored in a JSON file. There are several methods to read this format either in Python or in Pandas. My first choice for exploring new datasets is the spyder IDE as it offers a Variable Explorer that shows the contents as soon as you load the file in the main window.

I run the following lines, expecting it to load the file into a dataframe:

```python
import pandas as pd

file = '21-02-2022-fablab-list.json'

with open(file) as fablabs_io_list:
    df = pd.read_json(fablabs_io_list)
    print(df.info())
```

But I got an error:

```console
UnicodeDecodeError: 'charmap' codec can't decode byte 0x8d in position 17867: character maps to <undefined>
```

Given there are fablabs all around the world, I suspected one the characters in the file could be causing this problem. I double checked the pandas documentation and fortunately, read_json() supports custom encoding.

```console
read_json(path_or_buf=None, orient=None, typ='frame', dtype: 'DtypeArg | None' = None, convert_axes=None, convert_dates=True, keep_default_dates: 'bool' = True, numpy: 'bool' = False, precise_float: 'bool' = False, date_unit=None, encoding=None, encoding_errors: 'str | None' = 'strict', lines: 'bool' = False, chunksize: 'int | None' = None, compression: 'CompressionOptions' = 'infer', nrows: 'int | None' = None, storage_options: 'StorageOptions' = None)
    Convert a JSON string to pandas object.

    Parameters
    ----------
    path_or_buf : a valid JSON str, path object or file-like object
        Any valid string path is acceptable. The string could be a URL. Valid
        URL schemes include http, ftp, s3, and file. For file URLs, a host is
        expected. A local file could be:
        ``file://localhost/path/to/table.json``.

        If you want to pass in a path object, pandas accepts any
        ``os.PathLike``.

        By file-like object, we refer to objects with a ``read()`` method,
        such as a file handle (e.g. via builtin ``open`` function)
        or ``StringIO``.
```

I included the parameter `encoding='utf8'` to the call of read_json(), but I still got the same error message!

```python
with open(file) as fablabs_io_list:
    df = pd.read_json(fablabs_io_list, encoding='utf8')
```

#### Solving the encoding error when loading the JSON file

I spent a few minutes reading the docs, [Stack Overflow](https://stackoverflow.com/questions/57136931/why-do-i-get-a-unicode-encoding-error-in-the-middle-of-reading-a-file-with-pytho) and exploring the objects at the Spyder IDE. I got the following results at the Variable Explorer window:

![Spyder Variable Explorer Output](images/post/encoding-json-file-to-dataframe/spyder-variable-explorer-output.png)

The open context manager returns an object of type TextIOWrapper but the read_json() function expects a string, path or file-like object. Am I passing the right argument to it?

I tried passing the encoding argument to the open call instead of passing it to the read_json() function and it worked!

This is my complete code to import the json file into a pandas dataframe.

```python
with open(file, "r", encoding='utf8') as fablabs_io_list:
    df = pd.read_json(fablabs_io_list)
    print(df.info())
```

This is the output of the info() method on the dataframe:

```console
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 2049 entries, 0 to 2048
Data columns (total 23 columns):
 #   Column           Non-Null Count  Dtype  
---  ------           --------------  -----  
 0   id               2049 non-null   int64  
 1   name             2049 non-null   object 
 2   kind_name        2049 non-null   object 
 3   parent_id        701 non-null    float64
 4   blurb            2002 non-null   object 
 5   description      2020 non-null   object 
 6   slug             2049 non-null   object 
 7   avatar_url       1710 non-null   object 
 8   header_url       1359 non-null   object 
 9   address_1        2021 non-null   object 
 10  address_2        2019 non-null   object 
 11  city             2049 non-null   object 
 12  county           2049 non-null   object 
 13  postal_code      2047 non-null   object 
 14  country_code     2049 non-null   object 
 15  latitude         1835 non-null   float64
 16  longitude        1835 non-null   float64
 17  address_notes    2020 non-null   object 
 18  phone            2020 non-null   object 
 19  email            2020 non-null   object 
 20  capabilities     2049 non-null   object 
 21  activity_status  1490 non-null   object 
 22  links            2049 non-null   object 
dtypes: float64(3), int64(1), object(19)
memory usage: 368.3+ KB
None
```

#### Debuging the problem

Once I managed to get data loaded, I tried to debug the problem. Does it return an error if I pass the file directly to the read_json function?

I launched my shell and tried loading the file, including the `encoding='utf8' parameter:

```console
python
Python 3.9.1 (tags/v3.9.1:1e5d33e, Dec  7 2020, 17:08:21) [MSC v.1927 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import pandas as pd
>>> pd.read_json('21-02-2022-fablab-list.json', encoding='utf8')
        id                    name kind_name  ...                                       capabilities activity_status                                              links
0     2386            Al Jazri Lab   fab_lab  ...  [three_d_printing, cnc_milling, circuit_produc...          active  [{'id': 31737, 'url': 'https://www.facebook.co...
1       17                    PiNG   fab_lab  ...                                                 []            None  [{'id': 52, 'url': 'http://fablab.pingbase.net'}]
2       20  FabLab INSA Strasbourg   fab_lab  ...                                                 []            None      [{'id': 55, 'url': 'http://www.ideaslab.fr'}]
3      793           FabLab Zagreb   fab_lab  ...  [three_d_printing, cnc_milling, circuit_produc...          active      [{'id': 1323, 'url': 'http://www.fablab.hr'}]
4     2379                Exchange   fab_lab  ...  [three_d_printing, cnc_milling, circuit_produc...         planned  [{'id': 30145, 'url': 'https://laportelibrary....
...    ...                     ...       ...  ...                                                ...             ...                                                ...
2044  1587        TechWorks Amman    fab_lab  ...  [three_d_printing, cnc_milling, circuit_produc...          active  [{'id': 29176, 'url': 'https://youtu.be/zLt3aO...
2045  1538               MakersLab   fab_lab  ...  [three_d_printing, cnc_milling, circuit_produc...          active  [{'id': 4968, 'url': 'http://www.makerslab.nl/'}]
2046  1730       BISCAST ManFabLab   fab_lab  ...  [three_d_printing, cnc_milling, laser, precisi...          active                                                 []
2047  2210         FabLab L'Aquila   fab_lab  ...  [three_d_printing, cnc_milling, circuit_produc...          active  [{'id': 30617, 'url': 'https://www.instagram.c...
2048  2230                  ZapLab   fab_lab  ...  [three_d_printing, cnc_milling, circuit_produc...          active  [{'id': 30791, 'url': 'https://www.youtube.com...

[2049 rows x 23 columns]
```

So it only fails when I open the file externally and pass it to the read_json function. It works if I pass the encoding parameter to the open call or the read_json(). Why does it happen this way? I might need to return back to determine it when I am more fluent in Python.

[//]: # "TODO: Review what does this error happen when passing the content from the open call."

_Photo by Hunter Harritt on Unsplash._
