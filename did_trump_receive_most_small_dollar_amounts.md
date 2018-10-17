

```python
#imports libraries and csv file
import pandas as pd
df = pd.read_csv('election_data_wholefile.csv', index_col=False)
```

    /anaconda3/lib/python3.6/site-packages/IPython/core/interactiveshell.py:2728: DtypeWarning: Columns (6,11,12,13) have mixed types. Specify dtype option on import or set low_memory=False.
      interactivity=interactivity, compiler=compiler, result=result)



```python
#subsets data of only Trump contributions
trump = df.query('cand_nm == "Trump, Donald J."')
#subsets Trump data for only under $200
trump_smallcont = trump.query('contb_receipt_amt <=200')
```


```python
#finds sum of <$200 contributions to Trump
trump_smallcont.contb_receipt_amt.sum()
```




    34858234.269999996




```python
#subsets data of only Clinton contributions
hill = df.query('cand_nm == "Clinton, Hillary Rodham"')
#subsets Clinton data for only under $200
hill_smallcont = hill.query('contb_receipt_amt <=200')
```


```python
#finds sum of <$200 contributions to Clinton
hill_smallcont.contb_receipt_amt.sum()
```




    123864908.07000008




```python
#subsets data of only Sanders contributions
bern = df.query('cand_nm == "Sanders, Bernard"')
#subsets Sanders data for only under $200
bern_smallcont = bern.query('contb_receipt_amt <=200')
```


```python
#finds sum of <$200 contributions to Sanders
bern_smallcont.contb_receipt_amt.sum()
```




    61902951.78000003


