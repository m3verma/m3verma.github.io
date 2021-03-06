---
layout: default
---

# Data Loading, Storage, and File Formats

## Reading and Writing Data in Text Format

pandas features a number of functions for reading tabular data as a DataFrame object. Some functions are :

| Function        | Description          |
|:-------------|:------------------|
| read_csv | Load delimited data from a file, URL, or file-like object; use comma as default delimiter |
| read_table | Load delimited data from a file, URL, or file-like object; use tab ('\t') as default delimiter |
| read_fwf | Read data in fixed-width column format (i.e., no delimiters) |
| read_clipboard | Version of read_table that reads data from the clipboard; useful for converting tables from web pages |
| read_excel | Read tabular data from an Excel XLS or XLSX file |
| read_hdf | Read HDF5 files written by pandas |
| read_html | Read all tables found in the given HTML document |
| read_json | Read data from a JSON (JavaScript Object Notation) string representation |
| read_msgpack | Read pandas data encoded using the MessagePack binary format |
| read_pickle | Read an arbitrary object stored in Python pickle format |
| read_sas | Read a SAS dataset stored in one of the SAS system’s custom storage formats |
| read_sql | Read the results of a SQL query (using SQLAlchemy) as a pandas DataFrame |
| read_stata | Read a dataset from Stata file format |
| read_feather | Read the Feather binary file format |

These functions have optional arguments which may fall into few categories :

1. Indexing - Can treat one or more columns as the returned DataFrame, and whether to get column names from the file, the user, or not at all.
2. Type inference and data conversion - This includes the user-defined value conversions and custom list of missing value markers.
3. Datetime parsing - Includes combining capability, including combining date and time information spread over multiple columns into a single column in the result.
4. Iterating - Support for iterating over chunks of very large files.
5. Unclean data issues - Skipping rows or a footer, comments, or other minor things like numeric data with thousands separated by commas.

Handling dates and other custom types can require extra effort. Let’s start with a small comma-separated (CSV) text file :

```python
In [8]: !cat examples/ex1.csv
```
> a,b,c,d,message<br>
> 1,2,3,4,hello<br>
> 5,6,7,8,world<br>
> 9,10,11,12,foo<br>

Since this is comma-delimited, we can use read_csv to read it into a DataFrame :

```python
In [9]: df = pd.read_csv('examples/ex1.csv')
In [10]: df
```
> a   b   c   d message<br>
> 0  1   2   3   4   hello<br>
> 1  5   6   7   8   world<br>
> 2  9  10  11  12     foo<br>

A file will not always have a header row. To read this file, you have a couple of options. You can allow pandas to assign default column names, or you can specify names yourself :

```python
In [13]: pd.read_csv('examples/ex2.csv', header=None)
In [14]: pd.read_csv('examples/ex2.csv', names=['a', 'b', 'c', 'd', 'message'])
```
> a   b   c   d message<br>
> 0  1   2   3   4   hello<br>
> 1  5   6   7   8   world<br>
> 2  9  10  11  12     foo<br>

The parser functions have many additional arguments to help you handle the wide variety of exception file formats that occur. For example, you can skip the first, third, and fourth rows of a file with skiprows :

```python
In [23]: !cat examples/ex4.csv
```
> #hey!<br>
> a,b,c,d,message<br>
> #just wanted to make things more difficult for you<br>
> #who reads CSV files with computers, anyway?<br>
> 1,2,3,4,hello<br>
> 5,6,7,8,world<br>
> 9,10,11,12,foo<br>

```python
In [24]: pd.read_csv('examples/ex4.csv', skiprows=[0, 2, 3])
```
> a   b   c   d message<br>
> 0  1   2   3   4   hello<br>
> 1  5   6   7   8   world<br>
> 2  9  10  11  12     foo<br>

Handling missing values is an important and frequently nuanced part of the file parsing process. Missing data is usually either not present (empty string) or marked by some sentinel value. By default, pandas uses a set of commonly occurring sentinels, such as NA and NULL :

```python
In [25]: !cat examples/ex5.csv
```
> something,a,b,c,d,message<br>
> one,1,2,3,4,NA<br>
> two,5,6,,8,world<br>
> three,9,10,11,12,foo<br>

.The na_values option can take either a list or set of strings to consider missing values. Different NA sentinels can be specified for each column in a dict :

```python
In [31]: sentinels = {'message': ['foo', 'NA'], 'something': ['two']}
In [32]: pd.read_csv('examples/ex5.csv', na_values=sentinels)
```
> something  a   b     c   d message<br>
> 0       one  1   2   3.0   4     NaN<br>
> 1       NaN  5   6   NaN   8   world<br>
> 2     three  9  10  11.0  12     NaN<br>

Some read_csv/read_table function arguments :

| Argument        | Description          |
|:-------------|:------------------|
| path | String indicating filesystem location, URL, or file-like object |
| sep or delimiter | Character sequence or regular expression to use to split fields in each row |
| header | Row number to use as column names; defaults to 0 (first row), but should be None if there is no header row |
| index_col | Column numbers or names to use as the row index in the result; can be a single name/number or a list of them for a hierarchical index |
| names | List of column names for result, combine with header=None |
| skiprows | Number of rows at beginning of file to ignore or list of row numbers (starting from 0) to skip. |
| na_values | Sequence of values to replace with NA. |
| comment | Character(s) to split comments off the end of lines. |
| parse_dates | Attempt to parse data to datetime; False by default. If True, will attempt to parse all columns. Otherwise can specify a list of column numbers or name to parse. If element of list is tuple or list, will combine multiple columns together and parse to date (e.g., if date/time split across two columns). |
| keep_date_col | If joining columns to parse date, keep the joined columns; False by default. |
| converters | Dict containing column number of name mapping to functions (e.g., {'foo': f} would apply the function f to all values in the 'foo' column). |
| dayfirst | When parsing potentially ambiguous dates, treat as international format (e.g., 7/6/2012 -> June 7, 2012); False by default. |
| date_parser | Function to use to parse dates. |
| nrows | Number of rows to read from beginning of file. |
| iterator | Return a TextParser object for reading file piecemeal. |
| chunksize | For iteration, size of file chunks. |
| skip_footer | Number of lines to ignore at end of file. |
| verbose | Print various parser output information, like the number of missing values placed in non-numeric columns. |
| encoding | Text encoding for Unicode (e.g., 'utf-8' for UTF-8 encoded text). |
| squeeze | If the parsed data only contains one column, return a Series. |
| thousands | Separator for thousands (e.g., ',' or '.'). |

### Reading Text Files in Pieces

When processing very large files or figuring out the right set of arguments to correctly process a large file, you may only want to read in a small piece of a file or iterate through smaller chunks of the file. If you want to only read a small number of rows (avoiding reading the entire file), specify that with nrows :

```python
In [36]: pd.read_csv('examples/ex6.csv', nrows=5)
```
> one       two     three      four key<br>
> 0  0.467976 -0.038649 -0.295344 -1.824726   L<br>
> 1 -0.358893  1.404453  0.704965 -0.200638   B<br>
> 2 -0.501840  0.659254 -0.421691 -0.057688   G<br>
> 3  0.204886  1.074134  1.388361 -0.982404   R<br>
> 4  0.354628 -0.133116  0.283763 -0.837063   Q<br>

To read a file in pieces, specify a chunksize as a number of rows. The TextParser object returned by read_csv allows you to iterate over the parts of the file according to the chunksize. For example, we can iterate over ex6.csv, aggregating the value counts in the 'key' column like so :

```python
chunker = pd.read_csv('examples/ex6.csv', chunksize=1000)
tot = pd.Series([])
for piece in chunker:
    tot = tot.add(piece['key'].value_counts(), fill_value=0)
tot = tot.sort_values(ascending=False)
In [40]: tot[:5]
```
> M    338.0<br>
> J    337.0<br>
> F    335.0<br>
> K    334.0<br>
> H    330.0<br>
> dtype: float64<br>

### Writing Data to Text Format

Data can also be exported to a delimited format. Using DataFrame’s to_csv method, we can write the data out to a comma-separated file :

```python
In [43]: data.to_csv('examples/out.csv')
In [44]: !cat examples/out.csv
```
> ,something,a,b,c,d,message<br>
> 0,one,1,2,3.0,4,<br>
> 1,two,5,6,,8,world<br>
> 2,three,9,10,11.0,12,foo<br>

Other delimiters can be used, of course. Missing values appear as empty strings in the output. You might want to denote them by some other sentinel value. With no other options specified, both the row and column labels are written. Both of these can be disabled :

```python
In [46]: data.to_csv(sys.stdout, sep='|')
In [47]: data.to_csv(sys.stdout, na_rep='NULL')
In [48]: data.to_csv(sys.stdout, index=False, header=False)
```

### JSON Data

JSON (short for JavaScript Object Notation) has become one of the standard formats for sending data by HTTP request between web browsers and other applications. It is a much more free-form data format than a tabular text form like CSV. Here is an example :

```python
obj = """
{"name": "Wes",
 "places_lived": ["United States", "Spain", "Germany"],
 "pet": null,
 "siblings": [{"name": "Scott", "age": 30, "pets": ["Zeus", "Zuko"]},
              {"name": "Katie", "age": 38,
               "pets": ["Sixes", "Stache", "Cisco"]}]
}
"""
```

We’ll use json here, as it is built into the Python standard library. To convert a JSON string to Python form, use json.loads :

```python
In [62]: import json
In [63]: result = json.loads(obj)
In [64]: result
```
> {'name': 'Wes',<br>
> 'pet': None,<br>
> 'places_lived': ['United States', 'Spain', 'Germany'],<br>
> 'siblings': [{'age': 30, 'name': 'Scott', 'pets': ['Zeus', 'Zuko']},<br>
>  {'age': 38, 'name': 'Katie', 'pets': ['Sixes', 'Stache', 'Cisco']}]}<br>

json.dumps, on the other hand, converts a Python object back to JSON :

```python
In [65]: asjson = json.dumps(result)
```

The pandas.read_json can automatically convert JSON datasets in specific arrangements into a Series or DataFrame. For example :

```python
In [68]: !cat examples/example.json
```
> [{"a": 1, "b": 2, "c": 3},<br>
> {"a": 4, "b": 5, "c": 6},<br>
> {"a": 7, "b": 8, "c": 9}]<br>

### XML and HTML : Web Scraping

pandas has a built-in function, read_html, which uses libraries like lxml and Beautiful Soup to automatically parse tables out of HTML files as DataFrame objects. To show how this works, we downloaded an HTML file (used in the pandas documentation) from the United States FDIC government agency showing bank failures.1 First, you must install some additional libraries used by read_html :

```python
conda install lxml
pip install beautifulsoup4 html5lib
```

If you are not using conda, pip install lxml will likely also work. The pandas.read_html function has a number of options, but by default it searches for and attempts to parse all tabular data contained within <table> tags. The result is a list of DataFrame objects :

```python
In [73]: tables = pd.read_html('examples/fdic_failed_bank_list.html')
In [74]: len(tables)
```
> 1

```python
In [75]: failures = tables[0]
In [76]: failures.head()
```
> Bank Name             City  ST   CERT  \<br>
> 0                   Allied Bank         Mulberry  AR     91  <br> 
> 1  The Woodbury Banking Company         Woodbury  GA  11297   <br>
> 2        First CornerStone Bank  King of Prussia  PA  35312<br>

XML (eXtensible Markup Language) is another common structured data format supporting hierarchical, nested data with metadata. The New York Metropolitan Transportation Authority (MTA) publishes a number of data series about its bus and train services. Here we’ll look at the performance data, which is contained in a set of XML files. Each train or bus service has a different file (like Performance_MNR.xml for the Metro-North Railroad) containing monthly data as a series of XML records. Using lxml.objectify, we parse the file and get a reference to the root node of the XML file with getroot :

```python
from lxml import objectify
path = 'examples/mta_perf/Performance_MNR.xml'
parsed = objectify.parse(open(path))
root = parsed.getroot()
```

root.INDICATOR returns a generator yielding each <INDICATOR> XML element. For each record, we can populate a dict of tag names (like YTD_ACTUAL) to data values (excluding a few tags):
    
```python
data = []
skip_fields = ['PARENT_SEQ', 'INDICATOR_SEQ',
               'DESIRED_CHANGE', 'DECIMAL_PLACES']
for elt in root.INDICATOR:
    el_data = {}
    for child in elt.getchildren():
        if child.tag in skip_fields:
            continue
        el_data[child.tag] = child.pyval
    data.append(el_data)
In [81]: perf = pd.DataFrame(data)
In [82]: perf.head()
```

## Binary Data Formats

One of the easiest ways to store data (also known as serialization) efficiently in binary format is using Python’s built-in pickle serialization. pandas objects all have a to_pickle method that writes the data to disk in pickle format :

```python
In [87]: frame = pd.read_csv('examples/ex1.csv')
In [88]: frame
```
> a   b   c   d message<br>
> 0  1   2   3   4   hello<br>
> 1  5   6   7   8   world<br>
> 2  9  10  11  12     foo<br>

```python
In [89]: frame.to_pickle('examples/frame_pickle')
```

You can read any “pickled” object stored in a file by using the built-in pickle directly, or even more conveniently using pandas.read_pickle :

```python
In [90]: pd.read_pickle('examples/frame_pickle')
```
> a   b   c   d message<br>
> 0  1   2   3   4   hello<br>
> 1  5   6   7   8   world<br>
> 2  9  10  11  12     foo<br>

### Using HDF5 Format

HDF5 is a well-regarded file format intended for storing large quantities of scientific array data. It is available as a C library, and it has interfaces available in many other languages, including Java, Julia, MATLAB, and Python. The “HDF” in HDF5 stands for hierarchical data format. Each HDF5 file can store multiple datasets and supporting metadata. Compared with simpler formats, HDF5 supports on-the-fly compression with a variety of compression modes, enabling data with repeated patterns to be stored more efficiently. pandas provides a high-level interface that simplifies storing Series and
DataFrame object. The HDFStore class works like a dict and handles the low-level details :

```python
In [92]: frame = pd.DataFrame({'a': np.random.randn(100)})
In [93]: store = pd.HDFStore('mydata.h5')
In [94]: store['obj1'] = frame
In [95]: store['obj1_col'] = frame['a']
```

HDFStore supports two storage schemas, 'fixed' and 'table'. The latter is generally slower, but it supports query operations using a special syntax :

```python
In [98]: store.put('obj2', frame, format='table')
In [99]: store.select('obj2', where=['index >= 10 and index <= 15'])
```
> a<br>
> 10  1.007189<br>
> 11 -1.296221<br>
> 12  0.274992<br>
> 13  0.228913<br>
> 14  1.352917<br>
> 15  0.886429<br>

### Reading Microsoft Excel Files

pandas also supports reading tabular data stored in Excel 2003 (and higher) files using either the ExcelFile class or pandas.read_excel function. Internally these tools use the add-on packages xlrd and openpyxl to read XLS and XLSX files, respectively. You may need to install these manually with pip or conda. To use ExcelFile, create an instance by passing a path to an xls or xlsx file :

```python
In [104]: xlsx = pd.ExcelFile('examples/ex1.xlsx')
```

Data stored in a sheet can then be read into DataFrame with parse:

```python
In [105]: pd.read_excel(xlsx, 'Sheet1')
``` 
> a   b   c   d message<br>
> 0  1   2   3   4   hello<br>
> 1  5   6   7   8   world<br>
> 2  9  10  11  12     foo<br>

To write pandas data to Excel format, you must first create an ExcelWriter, then write data to it using pandas objects’ to_excel method :

```python
In [108]: writer = pd.ExcelWriter('examples/ex2.xlsx')
In [109]: frame.to_excel(writer, 'Sheet1')
In [110]: writer.save()
```

You can also pass a file path to to_excel and avoid the ExcelWriter :

```python
In [111]: frame.to_excel('examples/ex2.xlsx')
```

## Interacting with Web APIs

Many websites have public APIs providing data feeds via JSON or some other format. There are a number of ways to access these APIs from Python; one easy-to-use method that we recommend is the requests package. To find the last 30 GitHub issues for pandas on GitHub, we can make a GET HTTP request using the add-on requests library :

```python
In [113]: import requests
In [114]: url = 'https://api.github.com/repos/pandas-dev/pandas/issues'
In [115]: resp = requests.get(url)
In [116]: resp
```
> <Response [200]>

The Response object’s json method will return a dictionary containing JSON parsed into native Python objects :

```python
In [117]: data = resp.json()
In [118]: data[0]['title']
```
> 'Period does not round down for frequencies less that 1 hour'

## Interacting with Databases

In a business setting, most data may not be stored in text or Excel files. SQL-based relational databases (such as SQL Server, PostgreSQL, and MySQL) are in wide use, and many alternative databases have become quite popular. The choice of database is usually dependent on the performance, data integrity, and scalability needs of an application. Loading data from SQL into a DataFrame is fairly straightforward, and pandas has some functions to simplify the process.

```python
In [121]: import sqlite3
In [122]: query = """
   .....: CREATE TABLE test
     .....: (a VARCHAR(20), b VARCHAR(20),
   .....:  c REAL,        d INTEGER
   .....: );"""
In [123]: con = sqlite3.connect('mydata.sqlite')
In [124]: con.execute(query)
```
> <sqlite3.Cursor at 0x7f6b12a50f10>

```python
In [125]: con.commit()
```

Then, insert a few rows of data :

```python
In [126]: data = [('Atlanta', 'Georgia', 1.25, 6),
   .....:         ('Tallahassee', 'Florida', 2.6, 3),
   .....:         ('Sacramento', 'California', 1.7, 5)]
In [127]: stmt = "INSERT INTO test VALUES(?, ?, ?, ?)"
In [128]: con.executemany(stmt, data)
``` 
> <sqlite3.Cursor at 0x7f6b15c66ce0>

```python
In [129]: con.commit()
```

Most Python SQL drivers (PyODBC, psycopg2, MySQLdb, pymssql, etc.) return a list of tuples when selecting data from a table :

```python
In [130]: cursor = con.execute('select * from test')
In [131]: rows = cursor.fetchall()
In [132]: rows
```
> [('Atlanta', 'Georgia', 1.25, 6),<br>
>  ('Tallahassee', 'Florida', 2.6, 3),<br>
>  ('Sacramento', 'California', 1.7, 5)]<br>
