### Stocks trader:

This notebook aims to maximize the total returns from trading a financial security (e.g. stocks, options, bonds, etc.).

**Input:** *input_data.csv* 

This file should include the daily trading data of the financial instrument over some extended historical period.  
This file should include four features: 'date', 'high', 'low', 'close'.  
'date' format must be ="%m/%d/%Y"


The input data used in this notebook are those of the **OMXS30 (OMX Stockholm 30 Index)**

**Outputs:**
. *trading_decisions.csv*
. *trained_model.pkl* 

#### How it works?

The algorithm decides (based on the historical data of the last few days) whether to buy the instrument at the opening price of the current day. The algorithm also sets a "Target price" and a "Stop-loss price".

To start with, two technical indicators of the historical prices are constructed:  
    . Simple Moving average  
    . Bollinger Bands (https://en.wikipedia.org/wiki/Bollinger_Bands)
    
Buy decisions are based on those two technical indicators (note that the design here only makes one buy transaction followed by one sell transaction; short selling and multiple buys are not considered). 

Moreover, Bollinger Bands are used to set the "Target price" and a "Stop-loss price". Specifically, the upper bound of the Band is set as the "Target price"; while the lower bound of the Band is set as the "Stop-loss price". 

Buy decisions are limited to *one day*. This means that if a buy decision is made on a gven day, then the transaction must be closed in the following day. This can occur in one of two ways: (1) the following day's price reached the "Taregt price", or (2) the following day's price reached the "Stop-loss price", or (3) if neither of the previous two events occured, then the transaction is automatically closed at the following day's closing price.


**To maximize returns:**  A machine learing approach is taken. Specifically, a model is trained to predict whether a buy decision in a given day would be profitable (given the above-noted constraints). The decision threshold of this model is then adjusted to maximize the returns.
