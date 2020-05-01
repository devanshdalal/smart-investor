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
highs = (nifty["52 week high"] == nifty.High).value_counts()
print("Fraction of times market is at highs:", highs[True]/(highs[True] + highs[False]))
lows = (nifty["52 week low"] == nifty.Low).value_counts()
print("Fraction of times market is at lows:", lows[True]/(lows[True] + lows[False]))
```

    Fraction of times market is at highs: 0.09457579972183588
    Fraction of times market is at lows: 0.022253129346314324


<HR>

[vanilla_pe](/vanilla_pe.ipynb)

{
 "cells": [
  {
   "cell_type": "code",
   "execution_count": 1,
   "metadata": {},
   "outputs": [],
   "source": [
    "import glob\n",
    "import sys\n",
    "import os\n",
    "import enum\n",
    "import json\n",
    "import pandas as pd\n",
    "import matplotlib.pyplot as plt\n",
    "from datetime import datetime\n",
    "plt.rcParams['figure.figsize'] = [14, 7]"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 2,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "importing Jupyter notebook from drivers.ipynb\n",
      "importing Jupyter notebook from prepare.ipynb\n"
     ]
    }
   ],
   "source": [
    "import import_ipynb\n",
    "import drivers\n",
    "import prepare"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 3,
   "metadata": {},
   "outputs": [],
   "source": [
    "nifty = prepare.MergedDf()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 4,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/html": [
       "<div>\n",
       "<style scoped>\n",
       "    .dataframe tbody tr th:only-of-type {\n",
       "        vertical-align: middle;\n",
       "    }\n",
       "\n",
       "    .dataframe tbody tr th {\n",
       "        vertical-align: top;\n",
       "    }\n",
       "\n",
       "    .dataframe thead th {\n",
       "        text-align: right;\n",
       "    }\n",
       "</style>\n",
       "<table border=\"1\" class=\"dataframe\">\n",
       "  <thead>\n",
       "    <tr style=\"text-align: right;\">\n",
       "      <th></th>\n",
       "      <th>Open</th>\n",
       "      <th>High</th>\n",
       "      <th>Low</th>\n",
       "      <th>Close</th>\n",
       "      <th>Shares Traded</th>\n",
       "      <th>Turnover (Rs. Cr)</th>\n",
       "      <th>P/E</th>\n",
       "      <th>P/B</th>\n",
       "      <th>Div Yield</th>\n",
       "    </tr>\n",
       "  </thead>\n",
       "  <tbody>\n",
       "    <tr>\n",
       "      <th>count</th>\n",
       "      <td>5225.000000</td>\n",
       "      <td>5225.000000</td>\n",
       "      <td>5225.000000</td>\n",
       "      <td>5225.000000</td>\n",
       "      <td>5.225000e+03</td>\n",
       "      <td>5225.000000</td>\n",
       "      <td>5225.000000</td>\n",
       "      <td>5225.000000</td>\n",
       "      <td>5225.000000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>mean</th>\n",
       "      <td>4931.530211</td>\n",
       "      <td>4964.677742</td>\n",
       "      <td>4892.778804</td>\n",
       "      <td>4929.321809</td>\n",
       "      <td>1.515437e+08</td>\n",
       "      <td>6250.129768</td>\n",
       "      <td>19.882100</td>\n",
       "      <td>3.539041</td>\n",
       "      <td>1.419129</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>std</th>\n",
       "      <td>3247.573752</td>\n",
       "      <td>3256.301398</td>\n",
       "      <td>3231.571643</td>\n",
       "      <td>3243.876880</td>\n",
       "      <td>1.191109e+08</td>\n",
       "      <td>4840.834747</td>\n",
       "      <td>4.159403</td>\n",
       "      <td>0.798190</td>\n",
       "      <td>0.400195</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>min</th>\n",
       "      <td>853.000000</td>\n",
       "      <td>877.000000</td>\n",
       "      <td>849.950000</td>\n",
       "      <td>854.200000</td>\n",
       "      <td>1.394931e+06</td>\n",
       "      <td>40.120000</td>\n",
       "      <td>10.680000</td>\n",
       "      <td>1.920000</td>\n",
       "      <td>0.590000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>25%</th>\n",
       "      <td>1667.450000</td>\n",
       "      <td>1688.250000</td>\n",
       "      <td>1644.400000</td>\n",
       "      <td>1668.750000</td>\n",
       "      <td>6.926587e+07</td>\n",
       "      <td>2620.680000</td>\n",
       "      <td>17.010000</td>\n",
       "      <td>3.020000</td>\n",
       "      <td>1.160000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>50%</th>\n",
       "      <td>4877.850000</td>\n",
       "      <td>4930.250000</td>\n",
       "      <td>4833.050000</td>\n",
       "      <td>4875.050000</td>\n",
       "      <td>1.305892e+08</td>\n",
       "      <td>5462.340000</td>\n",
       "      <td>19.940000</td>\n",
       "      <td>3.470000</td>\n",
       "      <td>1.320000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>75%</th>\n",
       "      <td>7588.550000</td>\n",
       "      <td>7635.550000</td>\n",
       "      <td>7532.450000</td>\n",
       "      <td>7580.200000</td>\n",
       "      <td>1.900432e+08</td>\n",
       "      <td>8149.000000</td>\n",
       "      <td>22.660000</td>\n",
       "      <td>3.800000</td>\n",
       "      <td>1.540000</td>\n",
       "    </tr>\n",
       "    <tr>\n",
       "      <th>max</th>\n",
       "      <td>12274.900000</td>\n",
       "      <td>12293.900000</td>\n",
       "      <td>12252.750000</td>\n",
       "      <td>12271.800000</td>\n",
       "      <td>1.414837e+09</td>\n",
       "      <td>54081.530000</td>\n",
       "      <td>29.900000</td>\n",
       "      <td>6.550000</td>\n",
       "      <td>3.180000</td>\n",
       "    </tr>\n",
       "  </tbody>\n",
       "</table>\n",
       "</div>"
      ],
      "text/plain": [
       "               Open          High           Low         Close  Shares Traded  \\\n",
       "count   5225.000000   5225.000000   5225.000000   5225.000000   5.225000e+03   \n",
       "mean    4931.530211   4964.677742   4892.778804   4929.321809   1.515437e+08   \n",
       "std     3247.573752   3256.301398   3231.571643   3243.876880   1.191109e+08   \n",
       "min      853.000000    877.000000    849.950000    854.200000   1.394931e+06   \n",
       "25%     1667.450000   1688.250000   1644.400000   1668.750000   6.926587e+07   \n",
       "50%     4877.850000   4930.250000   4833.050000   4875.050000   1.305892e+08   \n",
       "75%     7588.550000   7635.550000   7532.450000   7580.200000   1.900432e+08   \n",
       "max    12274.900000  12293.900000  12252.750000  12271.800000   1.414837e+09   \n",
       "\n",
       "       Turnover (Rs. Cr)          P/E          P/B    Div Yield  \n",
       "count        5225.000000  5225.000000  5225.000000  5225.000000  \n",
       "mean         6250.129768    19.882100     3.539041     1.419129  \n",
       "std          4840.834747     4.159403     0.798190     0.400195  \n",
       "min            40.120000    10.680000     1.920000     0.590000  \n",
       "25%          2620.680000    17.010000     3.020000     1.160000  \n",
       "50%          5462.340000    19.940000     3.470000     1.320000  \n",
       "75%          8149.000000    22.660000     3.800000     1.540000  \n",
       "max         54081.530000    29.900000     6.550000     3.180000  "
      ]
     },
     "execution_count": 4,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "nifty.describe()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 5,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "Open                      896.40\n",
       "High                      905.45\n",
       "Low                       895.75\n",
       "Close                     897.80\n",
       "Shares Traded        32224833.00\n",
       "Turnover (Rs. Cr)         811.39\n",
       "P/E                        11.72\n",
       "P/B                         2.08\n",
       "Div Yield                   1.81\n",
       "Name: 1999-01-04 00:00:00, dtype: float64"
      ]
     },
     "execution_count": 5,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "nifty.loc['1999-01-04']"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 6,
   "metadata": {},
   "outputs": [],
   "source": [
    "# debt = DebtCorpus()\n",
    "# print(debt.Deposit(datetime(2000, 1, 1), 100))\n",
    "# print(debt.Withdraw(datetime(2010, 1, 1), 100))\n",
    "# debt.Get(datetime(2020, 1, 1))\n",
    "\n",
    "\n",
    "# vanilla strategy with params\n",
    "\n",
    "# monthly_sip = 100\n",
    "# default_exposure = 0.5\n",
    "# green_pe = 15\n",
    "# red_pe = 28\n",
    "\n",
    "# Every month, invest monthly_sip * default_exposure in index and invest monthly_sip * (1 - default_exposure) in debt.\n",
    "# If nifty pe > red_pe, pull out all money from index to debt.\n",
    "# if nifty pe < green_pe, pull out all money from debt to index."
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 7,
   "metadata": {},
   "outputs": [
    {
     "data": {
      "text/plain": [
       "datetime.datetime(2019, 12, 31, 0, 0)"
      ]
     },
     "execution_count": 7,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "nifty.index[-1].to_pydatetime()"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 8,
   "metadata": {},
   "outputs": [],
   "source": [
    "def EvaluateStrategy(df, params):\n",
    "    print('params:', json.dumps(params.__dict__, indent=2))\n",
    "    push_num_installments = int(params.push_num_installments)\n",
    "    pull_num_installments = int(params.pull_num_installments)\n",
    "    # strategy\n",
    "    curr_month = -1\n",
    "    e = drivers.EquityCorpus(df)\n",
    "    d = drivers.DebtCorpus()\n",
    "    total_invested = 0\n",
    "    num_installments = 0\n",
    "    size_installment = 0;\n",
    "    for ind in df.index:\n",
    "        if ind.month != curr_month:\n",
    "            curr_month = ind.month\n",
    "            index_sip = params.monthly_sip * params.default_exposure\n",
    "            debt_sip = params.monthly_sip * (1 - params.default_exposure)\n",
    "            current_pe = df['P/E'][ind]\n",
    "            if (current_pe < params.green_pe):\n",
    "                # we are in bear market.\n",
    "                if (0 == num_installments):\n",
    "                    debt_funds = d.Get(ind)\n",
    "                    # print('debt_funds', debt_funds, ind)\n",
    "                    size_installment = debt_funds / params.push_num_installments\n",
    "                to_invest = size_installment if num_installments < params.push_num_installments else d.Get(ind)\n",
    "                # print('to_invest', to_invest, size_installment, d.Get(ind))\n",
    "                debt_sip -= to_invest\n",
    "                index_sip += to_invest\n",
    "                num_installments+=1\n",
    "            elif (current_pe > params.red_pe):\n",
    "                # we are in bull market\n",
    "                equity_funds = e.Get(ind)\n",
    "                if (0 == num_installments):\n",
    "                    # print('equity_funds', equity_funds, ind)\n",
    "                    size_installment = equity_funds / params.pull_num_installments\n",
    "                to_redeem = min(size_installment, equity_funds)\n",
    "                # print('to_redeem', to_redeem, size_installment, e.Get(ind), ind)\n",
    "                index_sip -= to_redeem\n",
    "                debt_sip += to_redeem\n",
    "                num_installments+=1\n",
    "            else:\n",
    "                num_installments = 0\n",
    "            assert abs(index_sip + debt_sip - params.monthly_sip) < 0.01,\\\n",
    "                'index_sip:' + str(index_sip) + ', debt_sip: ' + str(debt_sip) + ', monthly_sip:' + str(params.monthly_sip) \n",
    "            if (index_sip > 0):\n",
    "                # print('deposit in equity', index_sip, ind)\n",
    "                e.Deposit(ind, index_sip)\n",
    "            elif (index_sip < 0):\n",
    "                # print('withdraw from equity', index_sip, ind)\n",
    "                e.Withdraw(ind, - index_sip)\n",
    "            if (debt_sip > 0):\n",
    "                d.Deposit(ind, debt_sip)\n",
    "            elif (debt_sip < 0):\n",
    "                d.Withdraw(ind, - debt_sip)\n",
    "            total_invested += params.monthly_sip\n",
    "    start_date = df.index[0].to_pydatetime()\n",
    "    end_date = df.index[-1].to_pydatetime()\n",
    "    # print('start-end', start_date, end_date)\n",
    "    # print('total_invested', total_invested)\n",
    "    # print('e.Get()', e.Get(end_date))\n",
    "    # print('d.Get()', d.Get(end_date))\n",
    "    returns = (e.Get(end_date) + d.Get(end_date)) / total_invested\n",
    "    print('returns', returns)\n",
    "    return returns"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 9,
   "metadata": {},
   "outputs": [],
   "source": [
    "# # Debug Strategy\n",
    "# params = drivers.Parameters(monthly_sip=100,\n",
    "#                             default_exposure=0.0,\n",
    "#                             green_pe=12,\n",
    "#                             red_pe=22,\n",
    "#                             pull_num_installments=2.0,\n",
    "#                             push_num_installments=2.0)\n",
    "# EvaluateStrategy(nifty, params)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 10,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "params: {\n",
      "  \"monthly_sip\": 100,\n",
      "  \"default_exposure\": 1,\n",
      "  \"green_pe\": -1,\n",
      "  \"red_pe\": 100,\n",
      "  \"pull_num_installments\": 12,\n",
      "  \"push_num_installments\": 12\n",
      "}\n",
      "returns 4.4901087169736345\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "4.4901087169736345"
      ]
     },
     "execution_count": 10,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Always and only equity investor\n",
    "params = drivers.Parameters(monthly_sip=100,\n",
    "                            default_exposure=1,\n",
    "                            green_pe=-1,\n",
    "                            red_pe=100,\n",
    "                            pull_num_installments=12,\n",
    "                            push_num_installments=12)\n",
    "EvaluateStrategy(nifty, params)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 11,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "params: {\n",
      "  \"monthly_sip\": 100,\n",
      "  \"default_exposure\": 0,\n",
      "  \"green_pe\": -1,\n",
      "  \"red_pe\": 100,\n",
      "  \"pull_num_installments\": 12,\n",
      "  \"push_num_installments\": 12\n",
      "}\n",
      "returns 2.326306709798203\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "2.326306709798203"
      ]
     },
     "execution_count": 11,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Always and only debt investor\n",
    "params = drivers.Parameters(monthly_sip=100,\n",
    "                            default_exposure=0,\n",
    "                            green_pe=-1,\n",
    "                            red_pe=100,\n",
    "                            pull_num_installments=12,\n",
    "                            push_num_installments=12)\n",
    "EvaluateStrategy(nifty, params)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 12,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "params: {\n",
      "  \"monthly_sip\": 100,\n",
      "  \"default_exposure\": 0.5,\n",
      "  \"green_pe\": -1,\n",
      "  \"red_pe\": 100,\n",
      "  \"pull_num_installments\": 12,\n",
      "  \"push_num_installments\": 12\n",
      "}\n",
      "returns 3.4082077133859188\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "3.4082077133859188"
      ]
     },
     "execution_count": 12,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Mixed investor, no rebalancing.\n",
    "params = drivers.Parameters(monthly_sip=100,\n",
    "                            default_exposure=0.5,\n",
    "                            green_pe=-1,\n",
    "                            red_pe=100,\n",
    "                            pull_num_installments=12,\n",
    "                            push_num_installments=12)\n",
    "EvaluateStrategy(nifty, params)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 13,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "params: {\n",
      "  \"monthly_sip\": 100,\n",
      "  \"default_exposure\": 0.5,\n",
      "  \"green_pe\": 18,\n",
      "  \"red_pe\": 28,\n",
      "  \"pull_num_installments\": 12,\n",
      "  \"push_num_installments\": 12\n",
      "}\n",
      "returns 4.791885658856569\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "4.791885658856569"
      ]
     },
     "execution_count": 13,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# Mixed investor, with rebalancing.\n",
    "params = drivers.Parameters(monthly_sip=100,\n",
    "                            default_exposure=0.5,\n",
    "                            green_pe=18,\n",
    "                            red_pe=28,\n",
    "                            pull_num_installments=12,\n",
    "                            push_num_installments=12)\n",
    "EvaluateStrategy(nifty, params)"
   ]
  },
  {
   "cell_type": "code",
   "execution_count": 14,
   "metadata": {},
   "outputs": [
    {
     "name": "stdout",
     "output_type": "stream",
     "text": [
      "params: {\n",
      "  \"monthly_sip\": 100,\n",
      "  \"default_exposure\": 0.15789473684210525,\n",
      "  \"green_pe\": 16.63157894736842,\n",
      "  \"red_pe\": 24.526315789473685,\n",
      "  \"pull_num_installments\": 2.0,\n",
      "  \"push_num_installments\": 2.0\n",
      "}\n",
      "returns 11.413761644520301\n"
     ]
    },
    {
     "data": {
      "text/plain": [
       "11.413761644520301"
      ]
     },
     "execution_count": 14,
     "metadata": {},
     "output_type": "execute_result"
    }
   ],
   "source": [
    "# A good strategy\n",
    "params = drivers.Parameters(monthly_sip=100,\n",
    "                            default_exposure=0.15789473684210525,\n",
    "                            green_pe=16.63157894736842,\n",
    "                            red_pe=24.526315789473685,\n",
    "                            pull_num_installments=2.0,\n",
    "                            push_num_installments=2.0)\n",
    "EvaluateStrategy(nifty, params)"
   ]
  },
  {
   "cell_type": "code",
   "metadata": {},
   "source": [
    "from scipy import optimize\n",
    "\n",
    "def f(z, *params):\n",
    "    de, gpe, rpe, pli, psi = z\n",
    "    p = drivers.Parameters(monthly_sip=100,\n",
    "                           default_exposure=de,\n",
    "                           green_pe=gpe,\n",
    "                           red_pe=rpe,\n",
    "                           pull_num_installments=pli,\n",
    "                           push_num_installments=psi)\n",
    "    return - 1 * EvaluateStrategy(nifty, p)\n",
    "\n",
    "rranges = ((0.0, 1.0), (12, 20), (22, 30), slice(2, 12), slice(2, 12))\n",
    "resbrute = optimize.brute(f, rranges, args=None, full_output=True,\n",
    "                              finish=optimize.fmin)"
   ]
  }
 ],
 "metadata": {
  "kernelspec": {
   "display_name": "Python 3",
   "language": "python",
   "name": "python3"
  },
  "language_info": {
   "codemirror_mode": {
    "name": "ipython",
    "version": 3
   },
   "file_extension": ".py",
   "mimetype": "text/x-python",
   "name": "python",
   "nbconvert_exporter": "python",
   "pygments_lexer": "ipython3",
   "version": "3.7.5"
  }
 },
 "nbformat": 4,
 "nbformat_minor": 2
}
