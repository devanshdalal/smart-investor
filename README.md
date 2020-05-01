## Inteligent Systematic Investments.

Evaluations of Manoj Kumar Jain's [Fixed-Fixed-Double Strategies](https://www.youtube.com/watch?v=kcHLgN8jUZ8)

```python
import glob
import pandas as pd
import matplotlib.pyplot as plt
plt.rcParams['figure.figsize'] = [14, 7]
```


```python
# for f in glob.glob('nse/*.csv'):
#     print(f)
l = sorted(os.listdir('nse'))
for f in l:
    print(f)
```

    2011.csv
    2012.csv
    2013.csv
    2014.csv
    2015.csv
    2016.csv
    2017.csv
    2018.csv
    2019.csv



```python
# Config options
cwd = 'nse'
```


```python
nifty = pd.concat([pd.read_csv(cwd + '/' + f) for f in sorted(os.listdir(cwd))], ignore_index = True)
```


```python
nifty.describe()
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
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Shares Traded</th>
      <th>Turnover (Rs. Cr)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>count</td>
      <td>2157.000000</td>
      <td>2157.000000</td>
      <td>2157.000000</td>
      <td>2157.000000</td>
      <td>2.157000e+03</td>
      <td>2157.000000</td>
    </tr>
    <tr>
      <td>mean</td>
      <td>7888.824200</td>
      <td>7926.308530</td>
      <td>7838.329694</td>
      <td>7882.328581</td>
      <td>2.010675e+08</td>
      <td>9130.983635</td>
    </tr>
    <tr>
      <td>std</td>
      <td>2108.947982</td>
      <td>2110.800172</td>
      <td>2100.736612</td>
      <td>2105.622145</td>
      <td>9.692724e+07</td>
      <td>4529.455523</td>
    </tr>
    <tr>
      <td>min</td>
      <td>4623.150000</td>
      <td>4623.150000</td>
      <td>4531.150000</td>
      <td>4544.200000</td>
      <td>6.555703e+06</td>
      <td>297.890000</td>
    </tr>
    <tr>
      <td>25%</td>
      <td>5837.950000</td>
      <td>5874.200000</td>
      <td>5798.150000</td>
      <td>5840.550000</td>
      <td>1.405172e+08</td>
      <td>6069.120000</td>
    </tr>
    <tr>
      <td>50%</td>
      <td>7959.850000</td>
      <td>7992.000000</td>
      <td>7922.800000</td>
      <td>7954.900000</td>
      <td>1.757243e+08</td>
      <td>7819.050000</td>
    </tr>
    <tr>
      <td>75%</td>
      <td>9755.750000</td>
      <td>9818.300000</td>
      <td>9714.400000</td>
      <td>9765.550000</td>
      <td>2.309406e+08</td>
      <td>10837.200000</td>
    </tr>
    <tr>
      <td>max</td>
      <td>12052.650000</td>
      <td>12103.050000</td>
      <td>12005.850000</td>
      <td>12088.550000</td>
      <td>7.411532e+08</td>
      <td>35131.190000</td>
    </tr>
  </tbody>
</table>
</div>




```python
nifty.head()
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
      <th>Date</th>
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Shares Traded</th>
      <th>Turnover (Rs. Cr)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>0</td>
      <td>03-Jan-2011</td>
      <td>6177.45</td>
      <td>6178.55</td>
      <td>6147.20</td>
      <td>6157.60</td>
      <td>96028639</td>
      <td>4477.36</td>
    </tr>
    <tr>
      <td>1</td>
      <td>04-Jan-2011</td>
      <td>6172.75</td>
      <td>6181.05</td>
      <td>6124.40</td>
      <td>6146.35</td>
      <td>181727905</td>
      <td>7678.55</td>
    </tr>
    <tr>
      <td>2</td>
      <td>05-Jan-2011</td>
      <td>6141.35</td>
      <td>6141.35</td>
      <td>6062.35</td>
      <td>6079.80</td>
      <td>139614193</td>
      <td>6606.21</td>
    </tr>
    <tr>
      <td>3</td>
      <td>06-Jan-2011</td>
      <td>6107.00</td>
      <td>6116.15</td>
      <td>6022.30</td>
      <td>6048.25</td>
      <td>152338978</td>
      <td>7050.18</td>
    </tr>
    <tr>
      <td>4</td>
      <td>07-Jan-2011</td>
      <td>6030.90</td>
      <td>6051.20</td>
      <td>5883.60</td>
      <td>5904.60</td>
      <td>171809106</td>
      <td>8325.79</td>
    </tr>
  </tbody>
</table>
</div>




```python
nifty["52 week high"] = pd.Series.rolling(nifty.High, window=200, min_periods=1).max()
nifty["52 week low"] = pd.Series.rolling(nifty.Low, window=200, min_periods=1).min()
```


```python
nifty[["52 week high", "52 week low", "Close"]].plot()
# nifty[["High"]].plot()
```




    <matplotlib.axes._subplots.AxesSubplot at 0x7fb6e9ec7e80>




![png](output_7_1.png)

```python
import glob
import sys
import os
import enum
import json
import pandas as pd
import matplotlib.pyplot as plt
from datetime import datetime
plt.rcParams['figure.figsize'] = [14, 7]
```


```python
import import_ipynb
import drivers
import prepare
```

    importing Jupyter notebook from drivers.ipynb
    importing Jupyter notebook from prepare.ipynb



```python
nifty = prepare.MergedDf()
```


```python
nifty.describe()
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
      <th>Open</th>
      <th>High</th>
      <th>Low</th>
      <th>Close</th>
      <th>Shares Traded</th>
      <th>Turnover (Rs. Cr)</th>
      <th>P/E</th>
      <th>P/B</th>
      <th>Div Yield</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>5225.000000</td>
      <td>5225.000000</td>
      <td>5225.000000</td>
      <td>5225.000000</td>
      <td>5.225000e+03</td>
      <td>5225.000000</td>
      <td>5225.000000</td>
      <td>5225.000000</td>
      <td>5225.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>4931.530211</td>
      <td>4964.677742</td>
      <td>4892.778804</td>
      <td>4929.321809</td>
      <td>1.515437e+08</td>
      <td>6250.129768</td>
      <td>19.882100</td>
      <td>3.539041</td>
      <td>1.419129</td>
    </tr>
    <tr>
      <th>std</th>
      <td>3247.573752</td>
      <td>3256.301398</td>
      <td>3231.571643</td>
      <td>3243.876880</td>
      <td>1.191109e+08</td>
      <td>4840.834747</td>
      <td>4.159403</td>
      <td>0.798190</td>
      <td>0.400195</td>
    </tr>
    <tr>
      <th>min</th>
      <td>853.000000</td>
      <td>877.000000</td>
      <td>849.950000</td>
      <td>854.200000</td>
      <td>1.394931e+06</td>
      <td>40.120000</td>
      <td>10.680000</td>
      <td>1.920000</td>
      <td>0.590000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>1667.450000</td>
      <td>1688.250000</td>
      <td>1644.400000</td>
      <td>1668.750000</td>
      <td>6.926587e+07</td>
      <td>2620.680000</td>
      <td>17.010000</td>
      <td>3.020000</td>
      <td>1.160000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>4877.850000</td>
      <td>4930.250000</td>
      <td>4833.050000</td>
      <td>4875.050000</td>
      <td>1.305892e+08</td>
      <td>5462.340000</td>
      <td>19.940000</td>
      <td>3.470000</td>
      <td>1.320000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>7588.550000</td>
      <td>7635.550000</td>
      <td>7532.450000</td>
      <td>7580.200000</td>
      <td>1.900432e+08</td>
      <td>8149.000000</td>
      <td>22.660000</td>
      <td>3.800000</td>
      <td>1.540000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>12274.900000</td>
      <td>12293.900000</td>
      <td>12252.750000</td>
      <td>12271.800000</td>
      <td>1.414837e+09</td>
      <td>54081.530000</td>
      <td>29.900000</td>
      <td>6.550000</td>
      <td>3.180000</td>
    </tr>
  </tbody>
</table>
</div>




```python
nifty.loc['1999-01-04']
```




    Open                      896.40
    High                      905.45
    Low                       895.75
    Close                     897.80
    Shares Traded        32224833.00
    Turnover (Rs. Cr)         811.39
    P/E                        11.72
    P/B                         2.08
    Div Yield                   1.81
    Name: 1999-01-04 00:00:00, dtype: float64




```python
# debt = DebtCorpus()
# print(debt.Deposit(datetime(2000, 1, 1), 100))
# print(debt.Withdraw(datetime(2010, 1, 1), 100))
# debt.Get(datetime(2020, 1, 1))


# vanilla strategy with params

# monthly_sip = 100
# default_exposure = 0.5
# green_pe = 15
# red_pe = 28

# Every month, invest monthly_sip * default_exposure in index and invest monthly_sip * (1 - default_exposure) in debt.
# If nifty pe > red_pe, pull out all money from index to debt.
# if nifty pe < green_pe, pull out all money from debt to index.
```


```python
nifty.index[-1].to_pydatetime()
```




    datetime.datetime(2019, 12, 31, 0, 0)




```python
def EvaluateStrategy(df, params):
    print('params:', json.dumps(params.__dict__, indent=2))
    push_num_installments = int(params.push_num_installments)
    pull_num_installments = int(params.pull_num_installments)
    # strategy
    curr_month = -1
    e = drivers.EquityCorpus(df)
    d = drivers.DebtCorpus()
    total_invested = 0
    num_installments = 0
    size_installment = 0;
    for ind in df.index:
        if ind.month != curr_month:
            curr_month = ind.month
            index_sip = params.monthly_sip * params.default_exposure
            debt_sip = params.monthly_sip * (1 - params.default_exposure)
            current_pe = df['P/E'][ind]
            if (current_pe < params.green_pe):
                # we are in bear market.
                if (0 == num_installments):
                    debt_funds = d.Get(ind)
                    # print('debt_funds', debt_funds, ind)
                    size_installment = debt_funds / params.push_num_installments
                to_invest = size_installment if num_installments < params.push_num_installments else d.Get(ind)
                # print('to_invest', to_invest, size_installment, d.Get(ind))
                debt_sip -= to_invest
                index_sip += to_invest
                num_installments+=1
            elif (current_pe > params.red_pe):
                # we are in bull market
                equity_funds = e.Get(ind)
                if (0 == num_installments):
                    # print('equity_funds', equity_funds, ind)
                    size_installment = equity_funds / params.pull_num_installments
                to_redeem = min(size_installment, equity_funds)
                # print('to_redeem', to_redeem, size_installment, e.Get(ind), ind)
                index_sip -= to_redeem
                debt_sip += to_redeem
                num_installments+=1
            else:
                num_installments = 0
            assert abs(index_sip + debt_sip - params.monthly_sip) < 0.01,\
                'index_sip:' + str(index_sip) + ', debt_sip: ' + str(debt_sip) + ', monthly_sip:' + str(params.monthly_sip) 
            if (index_sip > 0):
                # print('deposit in equity', index_sip, ind)
                e.Deposit(ind, index_sip)
            elif (index_sip < 0):
                # print('withdraw from equity', index_sip, ind)
                e.Withdraw(ind, - index_sip)
            if (debt_sip > 0):
                d.Deposit(ind, debt_sip)
            elif (debt_sip < 0):
                d.Withdraw(ind, - debt_sip)
            total_invested += params.monthly_sip
    start_date = df.index[0].to_pydatetime()
    end_date = df.index[-1].to_pydatetime()
    # print('start-end', start_date, end_date)
    # print('total_invested', total_invested)
    # print('e.Get()', e.Get(end_date))
    # print('d.Get()', d.Get(end_date))
    returns = (e.Get(end_date) + d.Get(end_date)) / total_invested
    print('returns', returns)
    return returns
```


```python
# # Debug Strategy
# params = drivers.Parameters(monthly_sip=100,
#                             default_exposure=0.0,
#                             green_pe=12,
#                             red_pe=22,
#                             pull_num_installments=2.0,
#                             push_num_installments=2.0)
# EvaluateStrategy(nifty, params)
```


```python
# Always and only equity investor
params = drivers.Parameters(monthly_sip=100,
                            default_exposure=1,
                            green_pe=-1,
                            red_pe=100,
                            pull_num_installments=12,
                            push_num_installments=12)
EvaluateStrategy(nifty, params)
```

    params: {
      "monthly_sip": 100,
      "default_exposure": 1,
      "green_pe": -1,
      "red_pe": 100,
      "pull_num_installments": 12,
      "push_num_installments": 12
    }
    returns 4.4901087169736345





    4.4901087169736345




```python
# Always and only debt investor
params = drivers.Parameters(monthly_sip=100,
                            default_exposure=0,
                            green_pe=-1,
                            red_pe=100,
                            pull_num_installments=12,
                            push_num_installments=12)
EvaluateStrategy(nifty, params)
```

    params: {
      "monthly_sip": 100,
      "default_exposure": 0,
      "green_pe": -1,
      "red_pe": 100,
      "pull_num_installments": 12,
      "push_num_installments": 12
    }
    returns 2.326306709798203





    2.326306709798203




```python
# Mixed investor, no rebalancing.
params = drivers.Parameters(monthly_sip=100,
                            default_exposure=0.5,
                            green_pe=-1,
                            red_pe=100,
                            pull_num_installments=12,
                            push_num_installments=12)
EvaluateStrategy(nifty, params)
```

    params: {
      "monthly_sip": 100,
      "default_exposure": 0.5,
      "green_pe": -1,
      "red_pe": 100,
      "pull_num_installments": 12,
      "push_num_installments": 12
    }
    returns 3.4082077133859188





    3.4082077133859188




```python
# Mixed investor, with rebalancing.
params = drivers.Parameters(monthly_sip=100,
                            default_exposure=0.5,
                            green_pe=18,
                            red_pe=28,
                            pull_num_installments=12,
                            push_num_installments=12)
EvaluateStrategy(nifty, params)
```

    params: {
      "monthly_sip": 100,
      "default_exposure": 0.5,
      "green_pe": 18,
      "red_pe": 28,
      "pull_num_installments": 12,
      "push_num_installments": 12
    }
    returns 4.791885658856569





    4.791885658856569




```python
# A good strategy
params = drivers.Parameters(monthly_sip=100,
                            default_exposure=0.15789473684210525,
                            green_pe=16.63157894736842,
                            red_pe=24.526315789473685,
                            pull_num_installments=2.0,
                            push_num_installments=2.0)
EvaluateStrategy(nifty, params)
```

    params: {
      "monthly_sip": 100,
      "default_exposure": 0.15789473684210525,
      "green_pe": 16.63157894736842,
      "red_pe": 24.526315789473685,
      "pull_num_installments": 2.0,
      "push_num_installments": 2.0
    }
    returns 11.413761644520301





    11.413761644520301


from scipy import optimize

def f(z, *params):
    de, gpe, rpe, pli, psi = z
    p = drivers.Parameters(monthly_sip=100,
                           default_exposure=de,
                           green_pe=gpe,
                           red_pe=rpe,
                           pull_num_installments=pli,
                           push_num_installments=psi)
    return - 1 * EvaluateStrategy(nifty, p)

rranges = ((0.0, 1.0), (12, 20), (22, 30), slice(2, 12), slice(2, 12))
resbrute = optimize.brute(f, rranges, args=None, full_output=True,
                              finish=optimize.fmin)
