# Financial-DataAnalysis
The financial sector plays a crucial role in the stability and growth of the economy. Analyzing the historical performance of major banks can provide valuable insights 
into market trends, risk management, and investment opportunities. This project focuses on a comprehensive analysis of the stock data of six major US banks: Bank of America (BAC),
Citigroup (C), Goldman Sachs (GS), JPMorgan Chase (JPM), Morgan Stanley (MS) and Wells Fargo (WFC) over a period of ten years, from 2006 to 2016.

# objective
     The primary objective of this project is to:

  -   Analyze the historical stock data of the selected banks.
  -   Visualize the trends and patterns in stock prices and returns.
  -  Assess the volatility and risk associated with each bank's stock.
  -  Examine the correlations between the stock performances of different banks.
  -  Identify significant events and trends that impacted the financial sector during the specified period.

# Tools and libraries used :
- pandas_datareader: For retrieving stock data.
- pandas: For data manipulation and analysis.
- numpy: For numerical operations.
- matplotlib: For static plotting.
- seaborn: For enhanced statistical plotting.
- plotly: For interactive visualizations.
- cufflinks: For integrating Plotly with pandas DataFrames.
- datetime: For handling date and time operations.

  # Discription

-  Data Loading

       # Install required packages
   
       !pip install pandas-datareader pandas numpy matplotlib seaborn plotly cufflinks
    
       # Import necessary libraries
   
         from pandas_datareader import data
         import pandas as pd
         import numpy as np
         import matplotlib.pyplot as plt
         import datetime
         import seaborn as sns
         import plotly
         import cufflinks as cf
    
       # Set up plotting and seaborn style
   
         sns.set_style('whitegrid')
         %matplotlib inline
         cf.go_offline()
    
       # Define date range for the data
   
         start = datetime.datetime(2006, 1, 1)
         end = datetime.datetime(2016, 1, 1)
    
       # Load data for each bank
   
         BAC = data.DataReader("BAC", 'stooq', start, end)
         C = data.DataReader("C", 'stooq', start, end)
         GS = data.DataReader("GS", 'stooq', start, end)
         JPM = data.DataReader("JPM", 'stooq', start, end)
         MS = data.DataReader("MS", 'stooq', start, end)
         WFC = data.DataReader("WFC", 'stooq', start, end)
    
       # Combine the data into a single DataFrame
   
         tickers = ['BAC', 'C', 'GS', 'JPM', 'MS', 'WFC']
         bank_stocks = pd.concat([BAC, C, GS, JPM, MS, WFC], axis=1, keys=tickers)
         bank_stocks.columns.names = ['Bank Ticker', 'Stock Info']
    
       # View the first few rows of the DataFrame
   
         bank_stocks.head()

  -  Pair Plot of Returns
    
         sns.pairplot(returns[1:])
     
    
     [image](https://github.com/Ishikamate/Financial-DataAnalysis/assets/85469773/9c8d9a00-3729-4372-a57d-466a6200bced)

 -   Minimum Return Date : This analysis identifies the dates on which each bank had its minimum daily return, indicating the worst performing days for each bank during the
   specified period.

         returns.idxmin()
   ![image](https://github.com/Ishikamate/Financial-DataAnalysis/assets/85469773/2b3ce475-fded-4c98-a9fc-02eeadf8dd9e)

   - Maximum Return Date : This analysis identifies the dates on which each bank had its maximum daily return, highlighting the best performing days for each bank.

         returns.idxmax()

  ![image](https://github.com/Ishikamate/Financial-DataAnalysis/assets/85469773/012de3ea-b12c-486e-8791-fb387ebf815d)

  - Standard Deviation of Returns : This calculation provides the standard deviation of daily returns for each bank, offering insights into the volatility of each bank's stock.

        returns.std()
  ![image](https://github.com/Ishikamate/Financial-DataAnalysis/assets/85469773/a8e0afb6-fb33-41f3-9f11-21c81f573236)


-   Standard Deviation of Returns in 2015 : This calculation focuses on the standard deviation of returns specifically for the year 2015, allowing for a more granular
  analysis of volatility during that year.

        returns['2015-01-01':'2015-12-31'].std()
  ![image](https://github.com/Ishikamate/Financial-DataAnalysis/assets/85469773/a6e76d84-0cdb-4c55-a39f-615732b0f349)

-   Distribution Plot of Morgan Stanley Returns in 2015 : This histogram shows the distribution of daily returns for Morgan Stanley in 2015, with the frequency of returns
  plotted against the return values.

        sns.distplot(returns['2015-01-01':'2015-12-31']['MSReturn'], color='Green', bins=100)
  ![image](https://github.com/Ishikamate/Financial-DataAnalysis/assets/85469773/9e5218d3-7e2e-49f0-bf5f-5b6b7d200531)

-   Distribution Plot of Citigroup Returns in 2008 : This histogram illustrates the distribution of daily returns for Citigroup in 2008, highlighting the impact of the
  financial crisis on the bank's performance.


        sns.distplot(returns['2008-01-01':'2008-12-31']['CReturn'], color='blue', bins=100)
  ![image](https://github.com/Ishikamate/Financial-DataAnalysis/assets/85469773/6a970606-2c50-494b-a96a-4ccbe05466ba)


  - Closing Prices of All Banks : This line plot shows the closing prices of all the banks from 2006 to 2016, allowing for a visual comparison of their performance over time.

        for tick in tickers:
           bank_stocks[tick]['Close'].plot(figsize=(12,4), label=tick)
        plt.legend()
  ![image](https://github.com/Ishikamate/Financial-DataAnalysis/assets/85469773/85f8a84e-49c3-4ddc-a75e-8e4c05490486)


-   Combined Closing Prices of All Banks : This aggregated line plot presents the closing prices of all banks in a single graph for a unified view of their trends over the decade.

        bank_stocks.xs(key='Close', axis=1, level='Stock Info').plot(figsize=(12,4))
  ![image](https://github.com/Ishikamate/Financial-DataAnalysis/assets/85469773/c8f391a2-1dd8-438b-9646-ed60fc741c6f)

-   Interactive Plot of Closing Prices : An interactive plotly graph of the closing prices, allowing for dynamic interaction such as zooming and hovering for detailed information.

        bank_stocks.xs(key='Close', axis=1, level='Stock Info').iplot()
  ![image](https://github.com/Ishikamate/Financial-DataAnalysis/assets/85469773/281096f4-bebb-4543-8df9-522e9688b9fd)

-    Rolling Mean and Closing Price of Bank of America (2008-2009) : This graph shows the 30-day rolling average of Bank of America's closing prices from 2008 to 2009, compared with the actual closing prices during the same period.


         BAC['Close'].loc['2008-01-01':'2009-01-01'].rolling(window=30).mean().plot(label='30 days AVG')
         BAC['Close'].loc['2008-01-01':'2009-01-01'].plot(label='BAC Close')
  ![image](https://github.com/Ishikamate/Financial-DataAnalysis/assets/85469773/2a6610ca-a267-4d48-8061-5804802abf74)

-    Correlation Heatmap of Closing Prices : A heatmap visualizing the correlation coefficients between the closing prices of the banks, indicating the strength and direction of their relationships.

         sns.heatmap(bank_stocks.xs(key='Close', axis=1, level='Stock Info').corr(), annot=True)
  ![image](https://github.com/Ishikamate/Financial-DataAnalysis/assets/85469773/42517b59-a788-4d15-a3ae-58d779b48ff3)

-    Cluster Map of Closing Prices Correlation : A cluster map that hierarchically clusters banks based on the correlations of their closing prices, providing a visual representation of similar and dissimilar banks.

         sns.clustermap(bank_stocks.xs(key='Close', axis=1, level='Stock Info').corr(), annot=True)
  ![image](https://github.com/Ishikamate/Financial-DataAnalysis/assets/85469773/e45fde49-d72f-4c33-93de-79f93e2e1d18)

-    Interactive Correlation Heatmap : An interactive plotly heatmap of the correlation matrix for the closing prices, offering an intuitive way to explore correlations.

         close_corr.iplot(kind='heatmap', colorscale='rdylbu')
  ![image](https://github.com/Ishikamate/Financial-DataAnalysis/assets/85469773/4c5792fb-2278-4310-8015-db5559d7a2f4)

-    Candlestick Chart of Bank of America (2015) : An interactive candlestick chart showing the open, high, low, and close prices of Bank of America for the year 2015, used for technical analysis.

         BAC[['Open', 'High', 'Low', 'Close']]['2015-01-01':'2016-01-01'].iplot(kind='candle', legend=True)
   ![image](https://github.com/Ishikamate/Financial-DataAnalysis/assets/85469773/2714ee92-8d63-450c-8575-646854f7139c)

-   Simple Moving Averages of Morgan Stanley (2015) : This graph presents the simple moving averages (SMA) of Morgan Stanley for different periods (13, 21, and 55 days) in 2015, used to identify trends.

        MS['Close'].loc['2015-01-01':'2016-01-01'].ta_plot(study='sma', periods=[13, 21, 55], title='Simple Moving Averages')
   ![image](https://github.com/Ishikamate/Financial-DataAnalysis/assets/85469773/c3444208-2d73-4eee-87b9-55f668fd197a)

-   Bollinger Bands of Bank of America (2015) : A plot showing the Bollinger Bands for Bank of America's closing prices in 2015, used to measure market volatility and identify overbought or oversold conditions.

        BAC['Close'].loc['2015-01-01':'2016-01-01'].ta_plot(study='boll')
   ![image](https://github.com/Ishikamate/Financial-DataAnalysis/assets/85469773/151d8af8-1f2f-4809-ad79-0d72546334b7)




