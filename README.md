# Easy way to scrap data 

Let’s say you are searching the web for some raw data you need for a project and you stumble across a webpage 

But the bad news is that the data lives inside a web page and there’s no API that you can use to grab the raw data. So now you have to waste 30 minutes throwing together a crappy script to download and parse out the data. It’s not hard, but it’s a waste of time that you could spend on something useful. And somehow 30 minutes always ends up being 2 hours.

Luckily, there’s a super simple answer. The Pandas library has a built-in method to extract tabular data from html pages called read_html():

```python
import pandas as pd

tables = pd.read_html("http://apps.sandiego.gov/sdfiredispatch/")

print(tables[0])
```
It’s that simple! Pandas will find any significant html tables on the page and return each one as a new DataFrame object.

To upgrade our program from toy to real, let’s tell Pandas that row 0 of the table has column headers and ask it to convert text-based dates into time objects:

```python
import pandas as pd

calls_df, = pd.read_html("http://apps.sandiego.gov/sdfiredispatch/", header=0, parse_dates=["Call Date"])

print(calls_df)
```

And how that the data lives in a DataFrame, the world is yours. Wish the data was available as json records? That’s just one more line of code!

```python
import pandas as pd

calls_df, = pd.read_html("http://apps.sandiego.gov/sdfiredispatch/", header=0, parse_dates=["Call Date"])

print(calls_df.to_json(orient="records", date_format="iso"))
```

You can even save the data right to a CSV or XLS file:

```python
import pandas as pd

calls_df, = pd.read_html("http://apps.sandiego.gov/sdfiredispatch/", header=0, parse_dates=["Call Date"])

calls_df.to_csv("calls.csv", index=False)
```
None of this is rocket science or anything, but I use it so often that I thought it was worth sharing. Have fun!


# Question you might have

calls_df, = pd.read_html(…`

What is the purpose of the comma after the variable name?

--> This is tuple unpacking. The expression on the right side of the equals sign returns a tuple of values, and we can easily unpack it into variables without having to write an extra line of code.
