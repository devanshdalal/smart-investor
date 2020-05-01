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
