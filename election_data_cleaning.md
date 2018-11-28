

```python
#imports libraries and csv file
import pandas as pd
import numpy as np
import datetime as dt
df = pd.read_csv(('P00000001-ALL.csv'), index_col=False)
```

    /anaconda3/lib/python3.6/site-packages/IPython/core/interactiveshell.py:2728: DtypeWarning: Columns (6,11,12,13) have mixed types. Specify dtype option on import or set low_memory=False.
      interactivity=interactivity, compiler=compiler, result=result)


### Data Assessment
- 116 "States" in df
- Contribution date not in datetime
- Some column names are confusing


```python
df.shape
```




    (7440252, 18)




```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>cmte_id</th>
      <th>cand_id</th>
      <th>cand_nm</th>
      <th>contbr_nm</th>
      <th>contbr_city</th>
      <th>contbr_st</th>
      <th>contbr_zip</th>
      <th>contbr_employer</th>
      <th>contbr_occupation</th>
      <th>contb_receipt_amt</th>
      <th>contb_receipt_dt</th>
      <th>receipt_desc</th>
      <th>memo_cd</th>
      <th>memo_text</th>
      <th>form_tp</th>
      <th>file_num</th>
      <th>tran_id</th>
      <th>election_tp</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>C00458844</td>
      <td>P60006723</td>
      <td>Rubio, Marco</td>
      <td>BLUM, MAUREEN</td>
      <td>WASHINGTON</td>
      <td>20</td>
      <td>DC</td>
      <td>STRATEGIC COALITIONS &amp; INITIATIVES LL</td>
      <td>OUTREACH DIRECTOR</td>
      <td>175.0</td>
      <td>15-MAR-16</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>SA17A</td>
      <td>1082559</td>
      <td>SA17.1152124</td>
      <td>P2016</td>
    </tr>
    <tr>
      <th>1</th>
      <td>C00458844</td>
      <td>P60006723</td>
      <td>Rubio, Marco</td>
      <td>DODSON, MARK B. MR.</td>
      <td>ATLANTA</td>
      <td>30</td>
      <td>GA</td>
      <td>MORTGAGE CAPITAL ADVISORS</td>
      <td>PRIVATE MORTGAGE BANKING</td>
      <td>25.0</td>
      <td>16-MAR-16</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>DEBT RETIREMENT</td>
      <td>SA17A</td>
      <td>1082559</td>
      <td>SA17.1153048</td>
      <td>P2016</td>
    </tr>
    <tr>
      <th>2</th>
      <td>C00458844</td>
      <td>P60006723</td>
      <td>Rubio, Marco</td>
      <td>CHILDERS, WILLIAM</td>
      <td>DPO</td>
      <td>AE</td>
      <td>098309998</td>
      <td>DIPLOMAT</td>
      <td>US GOVERNMENT</td>
      <td>100.0</td>
      <td>20-FEB-16</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>SA17A</td>
      <td>1056862</td>
      <td>SA17.1020839</td>
      <td>P2016</td>
    </tr>
    <tr>
      <th>3</th>
      <td>C00458844</td>
      <td>P60006723</td>
      <td>Rubio, Marco</td>
      <td>RUCINSKI, ROBERT</td>
      <td>APO</td>
      <td>AE</td>
      <td>090960009</td>
      <td>US ARMY</td>
      <td>PHYSICIAN</td>
      <td>200.0</td>
      <td>10-MAR-16</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>SA17A</td>
      <td>1082559</td>
      <td>SA17.1078677</td>
      <td>P2016</td>
    </tr>
    <tr>
      <th>4</th>
      <td>C00458844</td>
      <td>P60006723</td>
      <td>Rubio, Marco</td>
      <td>RUCINSKI, ROBERT</td>
      <td>APO</td>
      <td>AE</td>
      <td>090960009</td>
      <td>US ARMY</td>
      <td>PHYSICIAN</td>
      <td>100.0</td>
      <td>08-MAR-16</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>SA17A</td>
      <td>1082559</td>
      <td>SA17.1074981</td>
      <td>P2016</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.cand_nm.value_counts()
```


```python
#drops columns with unnecessary data
df.drop(['cmte_id', 'cand_id', 'contbr_zip', 'receipt_desc', 'memo_cd', 'memo_text', 'form_tp', 'file_num', 'tran_id', 'election_tp'], axis=1, inplace=True)
```


```python
#converts date column to datetime
df['contb_receipt_dt'] = pd.to_datetime(df['contb_receipt_dt'])
#creates new columns for day, month, and year    
df['month'] = df['contb_receipt_dt'].dt.month
df['day'] = df['contb_receipt_dt'].dt.day
df['year'] = df['contb_receipt_dt'].dt.year
```


```python
#changes some column names
df = df.rename(columns={'contb_receipt_amt': 'contribution_amt','contb_receipt_dt': 'contribution_date' })
```


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>cand_nm</th>
      <th>contbr_nm</th>
      <th>contbr_city</th>
      <th>contbr_st</th>
      <th>contbr_employer</th>
      <th>contbr_occupation</th>
      <th>contribution_amt</th>
      <th>contribution_date</th>
      <th>month</th>
      <th>day</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Rubio, Marco</td>
      <td>BLUM, MAUREEN</td>
      <td>WASHINGTON</td>
      <td>20</td>
      <td>STRATEGIC COALITIONS &amp; INITIATIVES LL</td>
      <td>OUTREACH DIRECTOR</td>
      <td>175.0</td>
      <td>2016-03-15</td>
      <td>3</td>
      <td>15</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Rubio, Marco</td>
      <td>DODSON, MARK B. MR.</td>
      <td>ATLANTA</td>
      <td>30</td>
      <td>MORTGAGE CAPITAL ADVISORS</td>
      <td>PRIVATE MORTGAGE BANKING</td>
      <td>25.0</td>
      <td>2016-03-16</td>
      <td>3</td>
      <td>16</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Rubio, Marco</td>
      <td>CHILDERS, WILLIAM</td>
      <td>DPO</td>
      <td>AE</td>
      <td>DIPLOMAT</td>
      <td>US GOVERNMENT</td>
      <td>100.0</td>
      <td>2016-02-20</td>
      <td>2</td>
      <td>20</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Rubio, Marco</td>
      <td>RUCINSKI, ROBERT</td>
      <td>APO</td>
      <td>AE</td>
      <td>US ARMY</td>
      <td>PHYSICIAN</td>
      <td>200.0</td>
      <td>2016-03-10</td>
      <td>3</td>
      <td>10</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Rubio, Marco</td>
      <td>RUCINSKI, ROBERT</td>
      <td>APO</td>
      <td>AE</td>
      <td>US ARMY</td>
      <td>PHYSICIAN</td>
      <td>100.0</td>
      <td>2016-03-08</td>
      <td>3</td>
      <td>8</td>
      <td>2016</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.groupby('cand_nm')['contribution_amt'].sum()
```




    cand_nm
    Bush, Jeb                    3.153619e+07
    Carson, Benjamin S.          2.626076e+07
    Christie, Christopher J.     7.859530e+06
    Clinton, Hillary Rodham      5.154629e+08
    Cruz, Rafael Edward 'Ted'    5.630681e+07
    Fiorina, Carly               6.290227e+06
    Gilmore, James S III         1.025097e+05
    Graham, Lindsey O.           3.671464e+06
    Huckabee, Mike               2.352096e+06
    Jindal, Bobby                1.264593e+06
    Johnson, Gary                3.557858e+06
    Kasich, John R.              1.421977e+07
    Lessig, Lawrence             6.214945e+05
    McMullin, Evan               5.521883e+05
    O'Malley, Martin Joseph      3.975922e+06
    Pataki, George E.            5.046494e+05
    Paul, Rand                   5.911533e+06
    Perry, James R. (Rick)       1.120363e+06
    Rubio, Marco                 3.130206e+07
    Sanders, Bernard             9.223310e+07
    Santorum, Richard J.         1.059324e+06
    Stein, Jill                  2.490936e+06
    Trump, Donald J.             1.243272e+08
    Walker, Scott                5.023928e+06
    Webb, James Henry Jr.        4.392464e+05
    Name: contribution_amt, dtype: float64




```python
#changes names of candidates to be more intuitive
df['cand_nm'] = df['cand_nm'].replace(['Rubio, Marco'], 'Marco Rubio')
#df['cand_nm'] = df['cand_nm'].replace(['Santorum, Richard J.'], 'Rick Santorum')
#df['cand_nm'] = df['cand_nm'].replace(['Perry, James R. (Rick)'], 'Rick Perry')
df['cand_nm'] = df['cand_nm'].replace(['Carson, Benjamin S.'], 'Ben Carson')
df['cand_nm'] = df['cand_nm'].replace(["Cruz, Rafael Edward 'Ted'"], 'Ted Cruz')
#df['cand_nm'] = df['cand_nm'].replace(['Paul, Rand'], 'Rand Paul')
#df['cand_nm'] = df['cand_nm'].replace(['Clinton, Hillary Rodham'], 'Hillary Clinton')
#df['cand_nm'] = df['cand_nm'].replace(['Sanders, Bernard'], 'Bernie Sanders')
#df['cand_nm'] = df['cand_nm'].replace(['Fiorina, Carly'], 'Fiorina Carly')
#df['cand_nm'] = df['cand_nm'].replace(['Huckabee, Mike'], 'Mike Huckabee')
#df['cand_nm'] = df['cand_nm'].replace(['Pataki, George E.'], 'George Pataki')
#df['cand_nm'] = df['cand_nm'].replace(["O'Malley, Martin Joseph"], "Martin O'Malley")
#df['cand_nm'] = df['cand_nm'].replace(['Graham, Lindsey O.'], 'Lindsey Graham')
#df['cand_nm'] = df['cand_nm'].replace(['Bush, Jeb'], 'Jeb Bush')
df['cand_nm'] = df['cand_nm'].replace(['Trump, Donald J.'], 'Donald Trump')
#df['cand_nm'] = df['cand_nm'].replace(['Jindal, Bobby'], 'Bobby Jindal')
#df['cand_nm'] = df['cand_nm'].replace(['Christie, Christopher J.'], 'Chris Christie')
#df['cand_nm'] = df['cand_nm'].replace(['Walker, Scott'], 'Scott Walker')
#df['cand_nm'] = df['cand_nm'].replace(['Stein, Jill'], 'Jill Stein')
#df['cand_nm'] = df['cand_nm'].replace(['Webb, James Henry Jr.'], 'Jim Webb')
df['cand_nm'] = df['cand_nm'].replace(['Kasich, John R.'], 'John Kasich')
#df['cand_nm'] = df['cand_nm'].replace(['Gilmore, James S III'], 'Jim Gilmore')
#df['cand_nm'] = df['cand_nm'].replace(['Lessig, Lawrence'], 'Lawrence Lessig')
#df['cand_nm'] = df['cand_nm'].replace(['Johnson, Gary'], 'Gary Johnson')
#df['cand_nm'] = df['cand_nm'].replace(['McMullin, Evan'], 'Evan McMullin')
```


```python
#array = ['Clinton, Hillary Rodham', 'Sanders, Bernard', 'Trump, Donald J.']
array = ['Marco Rubio','Ben Carson','Ted Cruz','Donald Trump','John Kasich']
df = df.loc[df['cand_nm'].isin(array)]
```


```python
df.shape
```




    (1718831, 11)




```python
#creates csv contribution file
df.to_csv('election_contributions.csv')
```


```python
#imports expenditure df
df = pd.read_csv(('Expenditure_.csv'), index_col=False)
#contr = pd.read_csv('election_final.csv')
```

    /anaconda3/lib/python3.6/site-packages/IPython/core/interactiveshell.py:2728: DtypeWarning: Columns (8) have mixed types. Specify dtype option on import or set low_memory=False.
      interactivity=interactivity, compiler=compiler, result=result)



```python
#drops columns with unnecessary data
df.drop(['cmte_id', 'cand_id', 'recipient_zip', 'memo_cd', 'form_tp', 'file_num', 'tran_id', 'election_tp'], axis=1, inplace=True)
```


```python
#converts date column to datetime
df['disb_dt'] = pd.to_datetime(df['disb_dt'])
#creates new columns for day, month, and year    
df['month'] = df['disb_dt'].dt.month
df['day'] = df['disb_dt'].dt.day
df['year'] = df['disb_dt'].dt.year
```


```python
array = ['Clinton, Hillary Rodham', 'Sanders, Bernard', 'Trump, Donald J.']
df = df.loc[df['cand_nm'].isin(array)]
```


```python
#changes names of candidates to be more intuitive
#df['cand_nm'] = df['cand_nm'].replace(['Rubio, Marco'], 'Marco Rubio')
#df['cand_nm'] = df['cand_nm'].replace(['Carson, Benjamin S.'], 'Ben Carson')
#df['cand_nm'] = df['cand_nm'].replace(["Cruz, Rafael Edward 'Ted'"], 'Ted Cruz')
#df['cand_nm'] = df['cand_nm'].replace(['Paul, Rand'], 'Rand Paul')
df['cand_nm'] = df['cand_nm'].replace(['Clinton, Hillary Rodham'], 'Hillary Clinton')
df['cand_nm'] = df['cand_nm'].replace(['Sanders, Bernard'], 'Bernie Sanders')
#df['cand_nm'] = df['cand_nm'].replace(['Fiorina, Carly'], 'Fiorina Carly')
#df['cand_nm'] = df['cand_nm'].replace(['Huckabee, Mike'], 'Mike Huckabee')
#df['cand_nm'] = df['cand_nm'].replace(['Bush, Jeb'], 'Jeb Bush')
df['cand_nm'] = df['cand_nm'].replace(['Trump, Donald J.'], 'Donald Trump')
#df['cand_nm'] = df['cand_nm'].replace(['Christie, Christopher J.'], 'Chris Christie')
#df['cand_nm'] = df['cand_nm'].replace(['Walker, Scott'], 'Scott Walker')
#df['cand_nm'] = df['cand_nm'].replace(['Stein, Jill'], 'Jill Stein')
#df['cand_nm'] = df['cand_nm'].replace(['Kasich, John R.'], 'John Kasich')
#df['cand_nm'] = df['cand_nm'].replace(['Johnson, Gary'], 'Gary Johnson')
```


```python
#df['transaction_type'] = 'Expenditure'
#contr['transaction_type'] = 'Contribution'
```


```python
df.shape
```




    (192556, 11)




```python
df = df.rename(columns={'disb_amt': 'expenditure_amt','disb_desc': 'expenditure_type', 'disb_dt': 'expenditure_date', 'memo_text': 'expenditure_memo'})
```


```python
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>cand_nm</th>
      <th>recipient_nm</th>
      <th>expenditure_amt</th>
      <th>expenditure_date</th>
      <th>recipient_city</th>
      <th>recipient_st</th>
      <th>expenditure_type</th>
      <th>expenditure_memo</th>
      <th>month</th>
      <th>day</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>42430</th>
      <td>Hillary Clinton</td>
      <td>MARSCH ENTERPRISES</td>
      <td>1500.00</td>
      <td>2016-03-31</td>
      <td>ANCHORAGE</td>
      <td>AK</td>
      <td>RENT</td>
      <td>NaN</td>
      <td>3</td>
      <td>31</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>42431</th>
      <td>Hillary Clinton</td>
      <td>GROH, KEVIN DANIEL</td>
      <td>1242.73</td>
      <td>2016-03-30</td>
      <td>SITKA</td>
      <td>AK</td>
      <td>PAYROLL</td>
      <td>NaN</td>
      <td>3</td>
      <td>30</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>42432</th>
      <td>Hillary Clinton</td>
      <td>TICE, CHARLES</td>
      <td>30.00</td>
      <td>2016-05-13</td>
      <td>ANCHORAGE</td>
      <td>AK</td>
      <td>PHONE</td>
      <td>NaN</td>
      <td>5</td>
      <td>13</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>42433</th>
      <td>Hillary Clinton</td>
      <td>GROH, KEVIN DANIEL</td>
      <td>1345.63</td>
      <td>2016-03-15</td>
      <td>SITKA</td>
      <td>AK</td>
      <td>PAYROLL</td>
      <td>NaN</td>
      <td>3</td>
      <td>15</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>42434</th>
      <td>Hillary Clinton</td>
      <td>MARSCH ENTERPRISES</td>
      <td>1500.00</td>
      <td>2016-03-31</td>
      <td>ANCHORAGE</td>
      <td>AK</td>
      <td>RENT</td>
      <td>NaN</td>
      <td>3</td>
      <td>31</td>
      <td>2016</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.dropna(subset = ['expenditure_type'], inplace=True)
```


```python
df.shape
```




    (192541, 11)




```python
df['expenditure_type'].value_counts()
```




    Travel                                      85599
    Payroll                                     38555
    Phone                                       19755
    Misc Fees                                    5823
    Various Rentals                              4273
    Office Supplies                              3953
    Event Supplies                               3496
    Shipping                                     3360
    Various Consulting                           3237
    Catering                                     3014
    PRINTING                                     1570
    UTILITIES                                    1233
    COMPUTER EQUIPMENT                           1104
    VENUE                                        1002
    LODGING                                       981
    EVENT PRODUCTION                              917
    CATERING                                      828
    SOFTWARE                                      654
    MEDIA PRODUCTION                              523
    AUDIO VISUAL SERVICES                         512
    SUBSCRIPTION                                  475
    TECHNOLOGY SERVICES                           458
    PER DIEM                                      377
    PARKING                                       367
    EVENT PLANNING SERVICES                       341
    POLLING                                       334
    INTERPRETING SERVICES                         320
    ORGANIZING SERVICES                           311
    SECURITY SERVICES                             304
    PHOTOGRAPHY/VIDEO EQUIPMENT                   302
                                                ...  
    MEETING EXPENSE: MEALS [NAGEL: SB23.6629        1
    CREDIT CARD PAYMENTS: MEMO AGGREGATES UN        1
    EVENT EXPENSE: STAGING                          1
    FUEL COSTS, LODGING; FEDERALLY PERMISSIB        1
    VENUE; FEDERALLY PERMISSIBLE FUNDS              1
    GRASSROOTS TRAINING SERVICES                    1
    SIGN LANGUAGE INTERPRETER SERVICES              1
    TRANSPORTATION SERVICES [AMEX: SB23.1808        1
    VOTER ACTIVATION NETWORK                        1
    EVENT INSURANCE                                 1
    SECURITY SERVICES [AMEX SB23.2859923]           1
    SECURITY/FIRE/EMS SERVICES                      1
    SECURITY SERVICES/JANITORIAL                    1
    EVENT EXPENSE: PARKING SERVICES                 1
    EQUIPMENT PURCHASE [AMEX: SB23.2859906]         1
    UTILITIES [AMEX: SB23.288951]                   1
    CATERING EXPENSE [MUNOZ: SB23.362879]           1
    OFFICE EXPENSES                                 1
    EQUIPMENT PURCHASE [AMEX: SB23.362557]          1
    EVENT SERVICES & SUPPLIES                       1
    AIR TAVEL                                       1
    LOCK & KEY SERVICES                             1
    CPR TRAINING; CREDIT                            1
    SECURITY SERVICES [KELLER: SB23.6806]           1
    PHOTOGRAPHY/VIDEO PRODUCTION                    1
    MEETING EXPENSE: MEALS [AMEX: SB23.28895        1
    FIRE MARSHALL DETAIL                            1
    PERMIT                                          1
    PARKING SERVICES [BYERS: SB23.455479]           1
    AUDIO VISUAL SERVICES [AMEX: SB23.248392        1
    Name: expenditure_type, Length: 819, dtype: int64




```python
df.loc[df['expenditure_type'].str.contains('Travel', case=False), 'expenditure_type'] = 'Travel'
df.loc[df['expenditure_type'].str.contains('Fee', case=False), 'expenditure_type'] = 'Misc Fees'
df.loc[df['expenditure_type'].str.contains('Rent', case=False), 'expenditure_type'] = 'Various Rentals'
df.loc[df['expenditure_type'].str.contains('Shipping', case=False), 'expenditure_type'] = 'Shipping'
df.loc[df['expenditure_type'].str.contains('Consult', case=False), 'expenditure_type'] = 'Various Consulting'
df.loc[df['expenditure_type'].str.contains('Payroll', case=False), 'expenditure_type'] = 'Payroll'
df.loc[df['expenditure_type'].str.contains('Phone', case=False), 'expenditure_type'] = 'Phone'
df.loc[df['expenditure_type'].str.contains('Event Supplies', case=False), 'expenditure_type'] = 'Event Supplies'
df.loc[df['expenditure_type'].str.contains('CATERING, FOOD & BEVERAGES', case=False), 'expenditure_type'] = 'Catering' 
df.loc[df['expenditure_type'].str.contains('OFFICE SUPPLIES', case=False), 'expenditure_type'] = 'Office Supplies' 

```


```python
df.to_csv('expenditures.csv')
```


```python
expend = df
frames = [contr, expend]
concat_df = pd.concat(frames)
```


```python
concat_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>cand_nm</th>
      <th>contb_receipt_amt</th>
      <th>contb_receipt_dt</th>
      <th>contbr_city</th>
      <th>contbr_employer</th>
      <th>contbr_nm</th>
      <th>contbr_occupation</th>
      <th>contbr_st</th>
      <th>day</th>
      <th>expenditure_amt</th>
      <th>expenditure_date</th>
      <th>expenditure_memo</th>
      <th>expenditure_type</th>
      <th>month</th>
      <th>recipient_city</th>
      <th>recipient_nm</th>
      <th>recipient_st</th>
      <th>transaction_type</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.0</td>
      <td>Bernie Sanders</td>
      <td>100.00</td>
      <td>2016-02-28</td>
      <td>BETHESDA</td>
      <td>NOT EMPLOYED</td>
      <td>MICHIE, JAMES</td>
      <td>NOT EMPLOYED</td>
      <td>MD</td>
      <td>28</td>
      <td>NaN</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>2</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Contribution</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.0</td>
      <td>Bernie Sanders</td>
      <td>27.00</td>
      <td>2016-04-05</td>
      <td>AMESBURY</td>
      <td>CATS N JAMMERS MUSIC STUDIOS</td>
      <td>PEIRCE, ERIC</td>
      <td>TEACHER</td>
      <td>MA</td>
      <td>5</td>
      <td>NaN</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Contribution</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2.0</td>
      <td>Donald Trump</td>
      <td>40.00</td>
      <td>2016-07-01</td>
      <td>HARRISBURG</td>
      <td>RETIRED</td>
      <td>HARGETT, JOYCE E MRS.</td>
      <td>RETIRED</td>
      <td>OR</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Contribution</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3.0</td>
      <td>Bernie Sanders</td>
      <td>5.00</td>
      <td>2016-04-25</td>
      <td>GOLDEN</td>
      <td>THE CONFLICT CENTER</td>
      <td>HICKS, MITZI</td>
      <td>NONPROFIT ADMINISTRATOR</td>
      <td>CO</td>
      <td>25</td>
      <td>NaN</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>4</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Contribution</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4.0</td>
      <td>Hillary Clinton</td>
      <td>20.72</td>
      <td>2016-10-07</td>
      <td>LONG BEACH</td>
      <td>SUSAN WALSHE, LMFT, INC</td>
      <td>WALSHE, SUSAN</td>
      <td>MARRIAGE FAMILY THERAPIST</td>
      <td>CA</td>
      <td>7</td>
      <td>NaN</td>
      <td>NaT</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>10</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>Contribution</td>
      <td>2016</td>
    </tr>
  </tbody>
</table>
</div>




```python
#drops columns with unnecessary data
concat_df.drop(['Unnamed: 0'], axis=1, inplace=True)
```


```python
concat_df.groupby(['cand_nm'])['contb_receipt_amt'].count().sort_values()
```




    cand_nm
    Donald Trump       29385
    Bernie Sanders     76193
    Hillary Clinton    94422
    Name: contb_receipt_amt, dtype: int64




```python
concat_df.to_csv('contrib_expendit.csv')
```


```python
import pandas as pd 
df = pd.read_csv(('election_final.csv'), index_col=None)
#expenditures = pd.read_csv(('expenditures.csv'), index_col=None)
```


```python
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Unnamed: 0</th>
      <th>cand_nm</th>
      <th>contbr_nm</th>
      <th>contbr_city</th>
      <th>contbr_st</th>
      <th>contbr_employer</th>
      <th>contbr_occupation</th>
      <th>contb_receipt_amt</th>
      <th>contb_receipt_dt</th>
      <th>month</th>
      <th>day</th>
      <th>year</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Bernie Sanders</td>
      <td>MICHIE, JAMES</td>
      <td>BETHESDA</td>
      <td>MD</td>
      <td>NOT EMPLOYED</td>
      <td>NOT EMPLOYED</td>
      <td>100.00</td>
      <td>2016-02-28</td>
      <td>2</td>
      <td>28</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Bernie Sanders</td>
      <td>PEIRCE, ERIC</td>
      <td>AMESBURY</td>
      <td>MA</td>
      <td>CATS N JAMMERS MUSIC STUDIOS</td>
      <td>TEACHER</td>
      <td>27.00</td>
      <td>2016-04-05</td>
      <td>4</td>
      <td>5</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Donald Trump</td>
      <td>HARGETT, JOYCE E MRS.</td>
      <td>HARRISBURG</td>
      <td>OR</td>
      <td>RETIRED</td>
      <td>RETIRED</td>
      <td>40.00</td>
      <td>2016-07-01</td>
      <td>7</td>
      <td>1</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Bernie Sanders</td>
      <td>HICKS, MITZI</td>
      <td>GOLDEN</td>
      <td>CO</td>
      <td>THE CONFLICT CENTER</td>
      <td>NONPROFIT ADMINISTRATOR</td>
      <td>5.00</td>
      <td>2016-04-25</td>
      <td>4</td>
      <td>25</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Hillary Clinton</td>
      <td>WALSHE, SUSAN</td>
      <td>LONG BEACH</td>
      <td>CA</td>
      <td>SUSAN WALSHE, LMFT, INC</td>
      <td>MARRIAGE FAMILY THERAPIST</td>
      <td>20.72</td>
      <td>2016-10-07</td>
      <td>10</td>
      <td>7</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>5</th>
      <td>5</td>
      <td>Hillary Clinton</td>
      <td>KOSMOWSKI, ERIC</td>
      <td>WASHINGTON</td>
      <td>DC</td>
      <td>FRONTIERBRIDGE</td>
      <td>INVESTOR</td>
      <td>250.00</td>
      <td>2015-10-13</td>
      <td>10</td>
      <td>13</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>6</th>
      <td>6</td>
      <td>Hillary Clinton</td>
      <td>THOMPSON, TRACY</td>
      <td>BERKELEY</td>
      <td>CA</td>
      <td>PALO ALTO FOUNDATION MEDICAL GROUP</td>
      <td>PHYSICIAN</td>
      <td>38.00</td>
      <td>2016-01-12</td>
      <td>1</td>
      <td>12</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>7</th>
      <td>7</td>
      <td>Bernie Sanders</td>
      <td>MICHAELS, DAN</td>
      <td>SAINT CLOUD</td>
      <td>MN</td>
      <td>ST. CLOUD STATE UNIVERSITY</td>
      <td>TECH SUPPORT</td>
      <td>5.00</td>
      <td>2016-05-28</td>
      <td>5</td>
      <td>28</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>8</th>
      <td>8</td>
      <td>Bernie Sanders</td>
      <td>JAMES, BRANDON</td>
      <td>SPRINGFIELD</td>
      <td>OH</td>
      <td>SELF-EMPLOYED</td>
      <td>RETAIL</td>
      <td>38.54</td>
      <td>2015-09-05</td>
      <td>9</td>
      <td>5</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>9</th>
      <td>9</td>
      <td>Hillary Clinton</td>
      <td>ROBINSON, LAWRENCE</td>
      <td>MEMPHIS</td>
      <td>TN</td>
      <td>WALGREENS</td>
      <td>PHARMACIST</td>
      <td>25.00</td>
      <td>2016-02-04</td>
      <td>2</td>
      <td>4</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>10</th>
      <td>10</td>
      <td>Bernie Sanders</td>
      <td>SIRIEIX, DAVID</td>
      <td>NEW YORK</td>
      <td>NY</td>
      <td>741MPH INC.</td>
      <td>ADVERTISING</td>
      <td>3.00</td>
      <td>2016-04-02</td>
      <td>4</td>
      <td>2</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>11</th>
      <td>11</td>
      <td>Hillary Clinton</td>
      <td>HAYNES, ELIZABETH</td>
      <td>UPPER MARLBORO</td>
      <td>MD</td>
      <td>SELF-EMPLOYED</td>
      <td>ORTHODONTIST</td>
      <td>25.00</td>
      <td>2016-10-10</td>
      <td>10</td>
      <td>10</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>12</th>
      <td>12</td>
      <td>Donald Trump</td>
      <td>TRIVITS, SHERYL</td>
      <td>PARSONSBURG</td>
      <td>MD</td>
      <td>INFORMATION REQUESTED</td>
      <td>VETERINARIAN</td>
      <td>60.00</td>
      <td>2016-08-09</td>
      <td>8</td>
      <td>9</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>13</th>
      <td>13</td>
      <td>Hillary Clinton</td>
      <td>MCKAY, SIDWAY</td>
      <td>BOULDER</td>
      <td>CO</td>
      <td>CONCENTRA</td>
      <td>PHYSICAL THERAPIST</td>
      <td>100.00</td>
      <td>2015-11-09</td>
      <td>11</td>
      <td>9</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>14</th>
      <td>14</td>
      <td>Bernie Sanders</td>
      <td>POELKER, RYNE</td>
      <td>CHICAGO</td>
      <td>IL</td>
      <td>NONE</td>
      <td>NOT EMPLOYED</td>
      <td>15.00</td>
      <td>2016-03-16</td>
      <td>3</td>
      <td>16</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>15</th>
      <td>15</td>
      <td>Hillary Clinton</td>
      <td>DUKE, JOAN</td>
      <td>BALTIMORE</td>
      <td>MD</td>
      <td>HCIC</td>
      <td>MANAGEMENT CONSULTANT</td>
      <td>25.00</td>
      <td>2016-10-24</td>
      <td>10</td>
      <td>24</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>16</th>
      <td>16</td>
      <td>Hillary Clinton</td>
      <td>ARZIO, JACKLYN</td>
      <td>WALNUT CREEK</td>
      <td>CA</td>
      <td>CGI</td>
      <td>SENIOR VP, REAL ESTATE</td>
      <td>10.00</td>
      <td>2016-02-05</td>
      <td>2</td>
      <td>5</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>17</th>
      <td>17</td>
      <td>Bernie Sanders</td>
      <td>PREVITI, CHRIS</td>
      <td>PRIMOS SECANE</td>
      <td>PA</td>
      <td>RETIRED</td>
      <td>RETIRED</td>
      <td>15.00</td>
      <td>2016-05-31</td>
      <td>5</td>
      <td>31</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>18</th>
      <td>18</td>
      <td>Hillary Clinton</td>
      <td>WILLAUER, CAROL</td>
      <td>PORTLAND</td>
      <td>ME</td>
      <td>RETIRED</td>
      <td>MANAGER</td>
      <td>25.00</td>
      <td>2016-09-29</td>
      <td>9</td>
      <td>29</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>19</th>
      <td>19</td>
      <td>Bernie Sanders</td>
      <td>LAITNER, JOHN</td>
      <td>TUCSON</td>
      <td>AZ</td>
      <td>SELF</td>
      <td>ECONOMIST</td>
      <td>50.00</td>
      <td>2016-02-24</td>
      <td>2</td>
      <td>24</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>20</th>
      <td>20</td>
      <td>Bernie Sanders</td>
      <td>YAMAMOTO, RICHARD</td>
      <td>MILILANI</td>
      <td>HI</td>
      <td>NOT EMPLOYED</td>
      <td>RETIRED</td>
      <td>35.00</td>
      <td>2016-02-12</td>
      <td>2</td>
      <td>12</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>21</th>
      <td>21</td>
      <td>Bernie Sanders</td>
      <td>MEYER, EILEEN</td>
      <td>BALTIMORE</td>
      <td>MD</td>
      <td>UNIVERSITY OF MARYLAND BALTIMORE COUNT</td>
      <td>ASSISTANT PROFESSOR</td>
      <td>27.00</td>
      <td>2016-04-14</td>
      <td>4</td>
      <td>14</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>22</th>
      <td>22</td>
      <td>Bernie Sanders</td>
      <td>JANISCH, ANNE</td>
      <td>SAINT PAUL</td>
      <td>MN</td>
      <td>UNIVERSITY OF MINNESOTA</td>
      <td>CLINICAL RESEARCH ASSOCIATE</td>
      <td>25.00</td>
      <td>2016-01-31</td>
      <td>1</td>
      <td>31</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>23</th>
      <td>23</td>
      <td>Bernie Sanders</td>
      <td>NIX, CAROL</td>
      <td>INDEPENDENCE</td>
      <td>WV</td>
      <td>NONE</td>
      <td>NOT EMPLOYED</td>
      <td>35.00</td>
      <td>2016-02-22</td>
      <td>2</td>
      <td>22</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>24</th>
      <td>24</td>
      <td>Donald Trump</td>
      <td>FUECHTMANN, MICHAEL</td>
      <td>ROSELLE</td>
      <td>IL</td>
      <td>RETIRED</td>
      <td>RETIRED</td>
      <td>20.00</td>
      <td>2016-06-25</td>
      <td>6</td>
      <td>25</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>25</th>
      <td>25</td>
      <td>Hillary Clinton</td>
      <td>RAY, CATHY</td>
      <td>SILVER SPRING</td>
      <td>MD</td>
      <td>PRINCE GEORGES COUNTY PUBLIC SCHOOLS</td>
      <td>TEACHER</td>
      <td>25.00</td>
      <td>2016-06-07</td>
      <td>6</td>
      <td>7</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>26</th>
      <td>26</td>
      <td>Hillary Clinton</td>
      <td>LEHR, KAREN</td>
      <td>MORRISTOWN</td>
      <td>NJ</td>
      <td>HIGH FOCUS CENTERS</td>
      <td>RN, LSW</td>
      <td>5.00</td>
      <td>2016-10-21</td>
      <td>10</td>
      <td>21</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>27</th>
      <td>27</td>
      <td>Donald Trump</td>
      <td>WALRAVEN, DANA</td>
      <td>ALVARADO</td>
      <td>TX</td>
      <td>COOK CHILDREN'S HEALTH CARE SYSTEM</td>
      <td>MANAGER</td>
      <td>83.59</td>
      <td>2016-09-10</td>
      <td>9</td>
      <td>10</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>28</th>
      <td>28</td>
      <td>Hillary Clinton</td>
      <td>ACKAH-YENSU, DENIS</td>
      <td>CHARLOTTE</td>
      <td>NC</td>
      <td>AXUM CAPITAL PARTNERS</td>
      <td>PRIVATE EQUITY</td>
      <td>250.00</td>
      <td>2016-09-30</td>
      <td>9</td>
      <td>30</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>29</th>
      <td>29</td>
      <td>Bernie Sanders</td>
      <td>SEBENIUS, CLARE</td>
      <td>LOS ANGELES</td>
      <td>CA</td>
      <td>SELF</td>
      <td>CREATIVE DIRECTOR</td>
      <td>100.00</td>
      <td>2016-04-06</td>
      <td>4</td>
      <td>6</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>199970</th>
      <td>199970</td>
      <td>Bernie Sanders</td>
      <td>AFZELIUS, JANET</td>
      <td>LAKE OSWEGO</td>
      <td>OR</td>
      <td>INFINITY REHAB</td>
      <td>SLP</td>
      <td>1.66</td>
      <td>2016-04-15</td>
      <td>4</td>
      <td>15</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199971</th>
      <td>199971</td>
      <td>Hillary Clinton</td>
      <td>OVERETT, ADAM</td>
      <td>NEW YORK</td>
      <td>NY</td>
      <td>LOGICPREP</td>
      <td>TUTOR</td>
      <td>25.00</td>
      <td>2016-09-30</td>
      <td>9</td>
      <td>30</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199972</th>
      <td>199972</td>
      <td>Hillary Clinton</td>
      <td>PAULIN, KATE</td>
      <td>NEW YORK</td>
      <td>NY</td>
      <td>360I</td>
      <td>ADVERTISING</td>
      <td>100.00</td>
      <td>2016-10-02</td>
      <td>10</td>
      <td>2</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199973</th>
      <td>199973</td>
      <td>Bernie Sanders</td>
      <td>TORRES, ORLANDO</td>
      <td>WESTON</td>
      <td>FL</td>
      <td>NOT EMPLOYED</td>
      <td>NOT EMPLOYED</td>
      <td>35.00</td>
      <td>2016-02-22</td>
      <td>2</td>
      <td>22</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199974</th>
      <td>199974</td>
      <td>Bernie Sanders</td>
      <td>ALFORD, KYLIE</td>
      <td>ARDEN</td>
      <td>NC</td>
      <td>NCDOJ</td>
      <td>FORENSIC SCIENTIST</td>
      <td>27.00</td>
      <td>2016-04-03</td>
      <td>4</td>
      <td>3</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199975</th>
      <td>199975</td>
      <td>Bernie Sanders</td>
      <td>BURROUGHS, KEITH</td>
      <td>GULF SHORES</td>
      <td>AL</td>
      <td>NONE</td>
      <td>NOT EMPLOYED</td>
      <td>5.00</td>
      <td>2015-12-30</td>
      <td>12</td>
      <td>30</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>199976</th>
      <td>199976</td>
      <td>Hillary Clinton</td>
      <td>MCGARRAH, ROBERT</td>
      <td>BETHESDA</td>
      <td>MD</td>
      <td>AFL-CIO</td>
      <td>ATTORNEY</td>
      <td>50.00</td>
      <td>2016-02-20</td>
      <td>2</td>
      <td>20</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199977</th>
      <td>199977</td>
      <td>Bernie Sanders</td>
      <td>LEE, JANETTE</td>
      <td>FORT LEE</td>
      <td>NJ</td>
      <td>ADASIA COMMUNICATIONS, INC.</td>
      <td>COPYWRITER</td>
      <td>15.00</td>
      <td>2016-02-27</td>
      <td>2</td>
      <td>27</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199978</th>
      <td>199978</td>
      <td>Hillary Clinton</td>
      <td>MCGINLEY MYERS, NANCY</td>
      <td>SAINT PAUL</td>
      <td>MN</td>
      <td>UNIVERSITY OF ST. THOMAS MINNESOTA</td>
      <td>INSTRUCTIONAL DESIGNER</td>
      <td>25.00</td>
      <td>2016-09-26</td>
      <td>9</td>
      <td>26</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199979</th>
      <td>199979</td>
      <td>Donald Trump</td>
      <td>ROBINSON, CARL</td>
      <td>LORANGER</td>
      <td>LA</td>
      <td>NON PROFIT</td>
      <td>BUSINESS MANGER</td>
      <td>28.00</td>
      <td>2016-06-21</td>
      <td>6</td>
      <td>21</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199980</th>
      <td>199980</td>
      <td>Donald Trump</td>
      <td>WIEHE, EVERETT</td>
      <td>LAFAYETTE</td>
      <td>CO</td>
      <td>INFORMATION REQUESTED</td>
      <td>INFORMATION REQUESTED</td>
      <td>400.00</td>
      <td>2016-10-21</td>
      <td>10</td>
      <td>21</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199981</th>
      <td>199981</td>
      <td>Hillary Clinton</td>
      <td>DUBOVSKY, ANTHONY</td>
      <td>BERKELEY</td>
      <td>CA</td>
      <td>UNIVERSITY OF CALIFORNIA, BERKELEY</td>
      <td>PAINTER</td>
      <td>15.00</td>
      <td>2016-11-05</td>
      <td>11</td>
      <td>5</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199982</th>
      <td>199982</td>
      <td>Bernie Sanders</td>
      <td>PANTALE, JOANNE</td>
      <td>MARCELLUS</td>
      <td>MI</td>
      <td>BAND G DISCOUNT</td>
      <td>SALES ASSOCIATE</td>
      <td>54.00</td>
      <td>2016-04-11</td>
      <td>4</td>
      <td>11</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199983</th>
      <td>199983</td>
      <td>Hillary Clinton</td>
      <td>HELD, VIRGINIA</td>
      <td>NEW YORK</td>
      <td>NY</td>
      <td>INFORMATION REQUESTED</td>
      <td>INFORMATION REQUESTED</td>
      <td>150.00</td>
      <td>2016-04-26</td>
      <td>4</td>
      <td>26</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199984</th>
      <td>199984</td>
      <td>Hillary Clinton</td>
      <td>SILVESTRI, STEPHEN</td>
      <td>MIAMI BEACH</td>
      <td>FL</td>
      <td>WANDA</td>
      <td>CREATIVE DIRECTOR</td>
      <td>25.00</td>
      <td>2016-11-03</td>
      <td>11</td>
      <td>3</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199985</th>
      <td>199985</td>
      <td>Hillary Clinton</td>
      <td>MEYER-STROM, PAUL</td>
      <td>PORTLAND</td>
      <td>OR</td>
      <td>NW PERMANENTE</td>
      <td>PSYCHIATRIST</td>
      <td>10.00</td>
      <td>2016-08-15</td>
      <td>8</td>
      <td>15</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199986</th>
      <td>199986</td>
      <td>Hillary Clinton</td>
      <td>TIBBALS, CLINTON</td>
      <td>FREDERICK</td>
      <td>CO</td>
      <td>U.S. NAVY</td>
      <td>RETIRED</td>
      <td>25.00</td>
      <td>2016-05-21</td>
      <td>5</td>
      <td>21</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199987</th>
      <td>199987</td>
      <td>Hillary Clinton</td>
      <td>ISAACS, ARABELLA</td>
      <td>BROOKLYN</td>
      <td>NY</td>
      <td>INFORMATION REQUESTED</td>
      <td>INFORMATION REQUESTED</td>
      <td>50.00</td>
      <td>2016-06-16</td>
      <td>6</td>
      <td>16</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199988</th>
      <td>199988</td>
      <td>Donald Trump</td>
      <td>FARRIS, OWEETA W MS.</td>
      <td>MEMPHIS</td>
      <td>TN</td>
      <td>INFORMATION REQUESTED</td>
      <td>INFORMATION REQUESTED</td>
      <td>16.00</td>
      <td>2016-07-06</td>
      <td>7</td>
      <td>6</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199989</th>
      <td>199989</td>
      <td>Bernie Sanders</td>
      <td>APPLEGATE, JOEL</td>
      <td>SPANISH FORK</td>
      <td>UT</td>
      <td>PRUDENTIAL UTAH ELITE REAL ESTATE</td>
      <td>REALTOR</td>
      <td>11.00</td>
      <td>2016-05-18</td>
      <td>5</td>
      <td>18</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199990</th>
      <td>199990</td>
      <td>Hillary Clinton</td>
      <td>SYNMOIE, PATRICK</td>
      <td>BROOKLYN</td>
      <td>NY</td>
      <td>NEW YORK CITY CLERK'S OFFICE</td>
      <td>ATTORNEY</td>
      <td>25.00</td>
      <td>2015-10-23</td>
      <td>10</td>
      <td>23</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>199991</th>
      <td>199991</td>
      <td>Bernie Sanders</td>
      <td>BERTA, ANASTASIA</td>
      <td>SAN DIEGO</td>
      <td>CA</td>
      <td>SELF</td>
      <td>DESIGNER</td>
      <td>100.00</td>
      <td>2016-01-18</td>
      <td>1</td>
      <td>18</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199992</th>
      <td>199992</td>
      <td>Bernie Sanders</td>
      <td>TAYLOR, GEOFF</td>
      <td>NORTHAMPTON</td>
      <td>MA</td>
      <td>NOT EMPLOYED</td>
      <td>NOT EMPLOYED</td>
      <td>108.00</td>
      <td>2015-11-15</td>
      <td>11</td>
      <td>15</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>199993</th>
      <td>199993</td>
      <td>Hillary Clinton</td>
      <td>KINDER, JANET</td>
      <td>SUNNYVALE</td>
      <td>CA</td>
      <td>SUNNYVALE SCHOOL DISTRICT</td>
      <td>SPECIAL EDUCATION TEACHER</td>
      <td>100.00</td>
      <td>2016-10-18</td>
      <td>10</td>
      <td>18</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199994</th>
      <td>199994</td>
      <td>Hillary Clinton</td>
      <td>YOUNGWERTH, CHERYL</td>
      <td>SCOTTSDALE</td>
      <td>AZ</td>
      <td>SELF-EMPLOYED</td>
      <td>DESIGNER</td>
      <td>50.00</td>
      <td>2016-09-29</td>
      <td>9</td>
      <td>29</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199995</th>
      <td>199995</td>
      <td>Hillary Clinton</td>
      <td>EPSTEIN, JOSH</td>
      <td>BROOKLYN</td>
      <td>NY</td>
      <td>FIRST LOOK MEDIA</td>
      <td>MEDIA</td>
      <td>200.00</td>
      <td>2016-10-20</td>
      <td>10</td>
      <td>20</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199996</th>
      <td>199996</td>
      <td>Bernie Sanders</td>
      <td>ELLIS, JASON</td>
      <td>GLENDALE</td>
      <td>AZ</td>
      <td>SALT RIVER PROJECT</td>
      <td>COMPUTER ANALYST</td>
      <td>22.00</td>
      <td>2016-03-31</td>
      <td>3</td>
      <td>31</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199997</th>
      <td>199997</td>
      <td>Donald Trump</td>
      <td>CONNER, STEVEN</td>
      <td>GREENVILLE</td>
      <td>OH</td>
      <td>INFORMATION REQUESTED</td>
      <td>INFORMATION REQUESTED</td>
      <td>4.00</td>
      <td>2016-08-05</td>
      <td>8</td>
      <td>5</td>
      <td>2016</td>
    </tr>
    <tr>
      <th>199998</th>
      <td>199998</td>
      <td>Hillary Clinton</td>
      <td>SELDNER, ELAINE</td>
      <td>MILLSTONE TOWNSHIP</td>
      <td>NJ</td>
      <td>RETIRED</td>
      <td>TEACHER</td>
      <td>1.00</td>
      <td>2015-10-15</td>
      <td>10</td>
      <td>15</td>
      <td>2015</td>
    </tr>
    <tr>
      <th>199999</th>
      <td>199999</td>
      <td>Hillary Clinton</td>
      <td>DONAGHY, MARY</td>
      <td>NEW YORK</td>
      <td>NY</td>
      <td>SELF-EMPLOYED</td>
      <td>VIDEO PRODUCTION/ACTING</td>
      <td>5.00</td>
      <td>2016-06-07</td>
      <td>6</td>
      <td>7</td>
      <td>2016</td>
    </tr>
  </tbody>
</table>
<p>200000 rows × 12 columns</p>
</div>




```python
expenditures.drop(['Unnamed: 0'], axis=1, inplace=True)
```


```python
df = df.rename(columns={'contb_receipt_amt': 'contribution_amt', 'contb_receipt_dt': 'contribution_date'})
```


```python
df['contbr_st'].unique()
```




    array(['MD', 'MA', 'OR', 'CO', 'CA', 'DC', 'MN', 'OH', 'TN', 'NY', 'IL',
           'PA', 'ME', 'AZ', 'HI', 'WV', 'NJ', 'TX', 'NC', 'WA', 'FL', 'NH',
           'VA', 'MO', 'MS', 'GA', 'KS', 'IN', 'KY', 'CT', 'VT', 'UT', 'IA',
           'MI', 'RI', 'AK', 'WI', 'ND', 'NE', 'LA', 'OK', 'NV', 'WY', 'AL',
           'NM', 'ZZ', 'SC', 'SD', 'DE', 'MT', 'AR', 'PR', 'ON', 'ID', 'VI',
           'AE', 'AS', 'AP', 'AA', 'GU', 'BC', 'ST', 'BR', 'MP', 'CH', 'NS',
           'QC', 'AB'], dtype=object)




```python
df['contbr_st'] = df['contbr_st'].replace(['LA'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['BC'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['ZZ'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['PR'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['XX'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['AP'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['AE'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['AA'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['MP'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['VI'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['ON'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['GU'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['NS'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['AU'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['AS'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['IS'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['QC'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['SO'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['FF'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['AB'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['CH'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['ST'], 'OTHER')
df['contbr_st'] = df['contbr_st'].replace(['BR'], 'OTHER')
```


```python
df['contbr_st'].nunique()
```




    51




```python
expenditures['recipient_st'].nunique()
```




    60




```python
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['KA'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['LA'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['MP'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['NB'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['ON'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['PR'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['SA'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['UK'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['VI'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['AB'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['AM'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['BC'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['ZZ'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['AU'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['GB'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['GU'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['QC'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['D.'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['LO'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['AS'], 'OTHER')
expenditures['recipient_st'] = expenditures['recipient_st'].replace(['FF'], 'OTHER')
```


```python
expenditures['recipient_st'].nunique()
```




    51




```python
df['contbr_occupation'] = df['contbr_occupation'].replace(['INFORMATION REQUESTED PER BEST EFFORTS'], 'UNKNOWN')
df['contbr_occupation'] = df['contbr_occupation'].replace(['INFORMATION REQUESTED'], 'UNKNOWN')
df['contbr_occupation'] = df['contbr_occupation'].replace(['RN'], 'REGISTERED NURSE')
df['contbr_occupation'] = df['contbr_occupation'].replace(['EDUCATOR'], 'TEACHER')
```


```python
df.loc[df['contbr_occupation'].str.contains('Not Employed', case=False), 'contbr_occupation'] = 'Unemployed'
df.loc[df['contbr_occupation'].str.contains('Retired', case=False), 'contbr_occupation'] = 'Retired'
df.loc[df['contbr_occupation'].str.contains('Unknown', case=False), 'contbr_occupation'] = 'Unknown'
df.loc[df['contbr_occupation'].str.contains('Attorney', case=False), 'contbr_occupation'] = 'Attorney'
df.loc[df['contbr_occupation'].str.contains('teacher', case=False), 'contbr_occupation'] = 'Teacher'
df.loc[df['contbr_occupation'].str.contains('Professor', case=False), 'contbr_occupation'] = 'Professor'
df.loc[df['contbr_occupation'].str.contains('PHYSICIAN', case=False), 'contbr_occupation'] = 'Physician'
df.loc[df['contbr_occupation'].str.contains('Consultant', case=False), 'contbr_occupation'] = 'Consultant'
df.loc[df['contbr_occupation'].str.contains('NURSE', case=False), 'contbr_occupation'] = 'RN' 
df.loc[df['contbr_occupation'].str.contains('ENGINEER', case=False), 'contbr_occupation'] = 'Engineer' 
```


```python
df['contbr_occupation'].value_counts()
```




    Unemployed                            21498
    Retired                               16199
    Unknown                                9101
    Teacher                                7682
    Attorney                               7412
    Engineer                               6288
    Professor                              5667
    Consultant                             5333
    Physician                              4242
    RN                                     3920
    LAWYER                                 1991
    SALES                                  1843
    MANAGER                                1764
    WRITER                                 1623
    ARTIST                                 1206
    SELF EMPLOYED                           999
    CEO                                     993
    PSYCHOLOGIST                            985
    OWNER                                   950
    STUDENT                                 891
    ACCOUNTANT                              861
    BUSINESS OWNER                          844
    SOCIAL WORKER                           838
    REALTOR                                 834
    PRESIDENT                               748
    MARKETING                               723
    PROJECT MANAGER                         710
    EXECUTIVE                               686
    HOMEMAKER                               674
    DIRECTOR                                666
                                          ...  
    PAYCHOLOGIST                              1
    DEPUTY EDITOR                             1
    PHOTO ANALYST                             1
    WIFE                                      1
    MARKETING SYSTEMS SPECIALIST              1
    OPERATING ROOM ASSISTANT                  1
    DIRECTOR OF EDUCATIONAL TECHNOLOGY        1
    AVIATION MAINTENANCE                      1
    PART TIME RECEPTIONIST                    1
    PT PROP MGR                               1
    SENIOR ASSOCIATE DIRECTOR                 1
    AUTO SALES.                               1
    COMMUNITY COLLEGE PRESIDENT               1
    SUPERINTENDENT OF PUBLIC WORKS            1
    EMMANUEL COLLEGE                          1
    VETERAN ADVOCATE                          1
    NERIUM BRAND PARTNER                      1
    CHILD CARE CENTER DIRECTOR                1
    COUNTY VETERANS SERVICES OFFICER          1
    SYSTEMS ADMIN / OPERATIONS                1
    TUTOR AND MUSICIAN                        1
    ASSOCIATE STUDIO MANAGER                  1
    SR NOC TECHNICIAN                         1
    GLOBAL PRODUCT MANAGER                    1
    RETAILOR                                  1
    TECHNICAL MARKETING                       1
    SPECIAL ED. BEHAVIOR TECH                 1
    COMMUNITY PARKS TEAM MEMBER               1
    SOFTWARE INFRASTRUCTURE ARCHITECT         1
    INTERNET BUSINESS OWNER                   1
    Name: contbr_occupation, Length: 20628, dtype: int64




```python
expenditures['cand_nm'].unique()
```




    array(['Hillary Clinton', 'Bernie Sanders', 'Donald Trump'], dtype=object)




```python
df.to_csv('full_contributions.csv')
expenditures.to_csv('full_expenditures.csv')
```


```python
import pandas as pd
df = pd.read_csv('full_contributions.csv')
```


```python
df['month'].value_counts()
```




    10    26809
    3     22316
    7     21848
    4     20429
    9     19105
    8     17896
    5     16063
    6     15970
    2     14174
    11    13996
    1      6575
    12     4819
    Name: month, dtype: int64



REFERENCES
> https://stackoverflow.com/questions/20297317/python-dataframe-pandas-drop-column-using-int
> https://stackoverflow.com/questions/21902080/python-pandas-not-reading-first-column-from-csv-file
> https://pandas.pydata.org/pandas-docs/stable/generated/pandas.read_csv.html
> https://chrisalbon.com/python/data_wrangling/pandas_saving_dataframe_as_csv/
> https://stackoverflow.com/questions/27060098/replacing-few-values-in-a-pandas-dataframe-column-with-another-value
> https://stackoverflow.com/questions/11346283/renaming-columns-in-pandas
> https://stackoverflow.com/questions/44068913/python-pandas-seed-for-random-generator
> https://stackoverflow.com/questions/38085547/random-sample-of-a-subset-of-a-dataframe-in-pandas
> https://stackoverflow.com/questions/12555323/adding-new-column-to-existing-dataframe-in-python-pandas
> https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.groupby.html
> https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.sort_values.html
> https://stackoverflow.com/questions/34913546/remove-low-counts-from-pandas-data-frame-column-on-condition
> https://stackoverflow.com/questions/35678083/pandas-delete-rows-of-a-dataframe-if-total-count-of-a-particular-column-occurs
> https://erikrood.com/Python_References/rows_cols_python.html
> https://datascience.stackexchange.com/questions/12645/how-to-count-the-number-of-missing-values-in-each-row-in-pandas-dataframe
>https://stackoverflow.com/questions/13413590/how-to-drop-rows-of-pandas-dataframe-whose-value-in-certain-columns-is-nan
