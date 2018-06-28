
---
title: 'Load excel data to python, pandas vs. xlwings'
date: 2018-06-25 15:42:00
tags: [python,data science,data engineering,pandas,xlwings]
categories: Data Science
---
開始搞 data 之後就知道 ETL 是多重要的東西

但是 ETL 的工具大部分都綁手綁腳的，所以就開始嘗試自己動手用 python 做 ETL 

<!--More-->

## Intro

在沒用 python 之前，因為公司是用 SQL server  
所以想當然爾， ETL 都是用 [SSIS](https://docs.microsoft.com/zh-tw/sql/integration-services/sql-server-integration-services?view=sql-server-2017) 來做

但圖形化的工具有一個致命性的缺點，就是慢!  
剛好最近想學一點 python 就順水推舟用 python 來做 ETL

剛開始用 [openpyxl](https://openpyxl.readthedocs.io/) + [SQLAlchemy](https://www.sqlalchemy.org/) 來做讀 excel 在塞進資料庫  
但是這兩個套件的效率都很低

之後看了網路上其他人的效能測試，讀 excel 端就改用了 [xlwings](xlwings.org)  
而寫入資料庫是用[pyodbc](https://github.com/mkleehammer/pyodbc/wiki/Connecting-to-SQL-Server-from-Windows) ，比起 *SQLALchemy* 要每次把 model 建出來，速度提升不少

這套我也用了不少次，但周末聽朋友推薦說都用 [pandas](https://pandas.pydata.org/) 來讀 excel ，就想說那就來比比看吧  
況且在 *pandas* 底下，資料操作應該會比較方便  
*NOTE: pandas 讀 excel 的 module 是 [xlrd](https://xlrd.readthedocs.io/)*

## Let's Begin

這邊用的 data 是用比利時政府的資料: [Population by place of residence, nationality, marital status, age and sex](https://statbel.fgov.be/en/open-data/population-place-residence-nationality-marital-status-age-and-sex-8) 來做測試  
畢竟不能用公司的資料，但要找跟公司資料相近性質和大小的 data 來做測試


```python
import pandas as pd
import timeit
import xlwings as xw

path = r'C:\Users\10512157\Desktop\TF_SOC_POP_STRUCT_2018.xlsx'
```

## Loading data from pandas and measure the time consumption


```python
timer_start = timeit.default_timer()

t = pd.read_excel(path)

timer_end = timeit.default_timer()
print(timer_end - timer_start)
```

    75.93760436134232
    

## Loading data from xlwings and measure the time consumption


```python
timer_start = timeit.default_timer()

app = xw.App(visible=False)
wb = app.books.open(path)
sht = wb.sheets[0]

timer_end = timeit.default_timer()
print(timer_end - timer_start)
```

    17.660102651995146
    

我們可以看到 *xlwings* 讀資料的速度明顯比 *pandas* 快上不少  
資料的操作方法我這邊就不細比  

這邊在測試的時候倒是發現一件有趣的事

## Putting data from xlwings to pandas dataframe

我們嘗試把 *xlwings* 拉進來的資料塞到 *pandas* DataFrame  
*xlwings* 官方有提供 api 可以直接塞


```python
xw_to_pd = sht.range('A:V').options(pd.DataFrame, index=False).value
```

    .
    .
    .
    c:\users\10512157\appdata\local\continuum\anaconda3\lib\site-packages\win32com\client\dynamic.py in __getattr__(self, attr)
        514                         debug_attr_print("Getting property Id 0x%x from OLE object" % retEntry.dispid)
        515                         try:
    --> 516                                 ret = self._oleobj_.Invoke(retEntry.dispid,0,invoke_type,1)
        517                         except pythoncom.com_error as details:
        518                                 if details.hresult in ERRORS_BAD_CONTEXT:
    

    com_error: (-2147024882, '存放裝置空間不足，無法完成此操作。', None, None)


我們會發現他會出現 *存放裝置空間不足，無法完成此操作。* 的 error

照這樣說 *pandas* 可以全部塞進去，所以是在 memory 存放有做優化囉  
看起來是這樣沒錯，但是 *xlwings* 有一點小bug  

我們單載入一列，然後來看一下載進來的資料筆數


```python
print(f'pandas: {len(t)}')
xw_to_pd = sht.range('A:A').options(pd.DataFrame, index=False).value
print(f'xlwings to pandas: {len(xw_to_pd)}')
```

    pandas: 465418
    xlwings to pandas: 1048575
    

咦? 怎麼兩個筆數不一樣多  
看了資料名名才40萬筆， *xlwings* 怎麼會有100多萬筆

再用原生的 *xlwings* api 抓看看


```python
sht.range('A1').current_region.last_cell.row
```




    465419



真的40萬多筆阿!

所以看來是轉到 *pandas* 的時候出錯了

## Dig deeper in xlwings

這邊我們再用另一個方法來抓

看看只抓40萬筆的時候是不是可以塞進去的

這邊是 *xlwings* 抓資料更好的做法

可以抓到 excel 的 range 之後存成指定範圍


```python
## find range of rows
used_range_rows = (sht.api.UsedRange.Row, sht.api.UsedRange.Row + sht.api.UsedRange.Rows.Count) 
## find range of columns
used_range_cols = (sht.api.UsedRange.Column, sht.api.UsedRange.Column + sht.api.UsedRange.Columns.Count) 

## assign range
used_range = xw.Range(*zip(used_range_rows, used_range_cols))
used_range.select()
```


```python
xw_to_pd = sht.range(used_range).options(pd.DataFrame, index=False).value
```

    .
    .
    .
    c:\users\10512157\appdata\local\continuum\anaconda3\lib\site-packages\win32com\client\dynamic.py in __getattr__(self, attr)
        514                         debug_attr_print("Getting property Id 0x%x from OLE object" % retEntry.dispid)
        515                         try:
    --> 516                                 ret = self._oleobj_.Invoke(retEntry.dispid,0,invoke_type,1)
        517                         except pythoncom.com_error as details:
        518                                 if details.hresult in ERRORS_BAD_CONTEXT:
    

    com_error: (-2147024882, '存放裝置空間不足，無法完成此操作。', None, None)


又出現存放空間不足的問題了

看來只塞指定的資料也是進不去的...

## Summary

以上述的試驗來看， *pandas* 完勝 *xlwings* ，唯一輸的地方只有讀資料的時候比較慢  
但我認為應該是因為 *pandas* **可能**有先做資料壓縮，不過這也是要看 source code 才會知道

但是在資料操作上 *pandas* 一定也是完勝的，所以除非不透過 *pandas*  
不然 *xlwings* 也是沒有必要的

對了，順便再提個自己的 case study

如果要直接把資料丟進 SQL Server 裡面，比起用 *xlwings+pyodbc* 塞資料  
用 *SSIS* 或是 SQL Server 內建的匯入要快的多

所以除非是要自動化，不然塞資料最快的方法還是用 *pandas* 先把資料轉成 csv file  
再用 SQL Server 直接塞資料
就算是要自動化， *pandas* 多的那一分鐘也不會改變什麼

結論還是 **pandas 完勝**
