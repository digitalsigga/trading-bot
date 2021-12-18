# trading-bot

This is a simple python trading algorithm built in the QuantConnect platform.

QuantConnect provides a free algorithm backtesting tool and financial data so engineers can design algorithmic trading strategies. 

In this algorithm we look at a past high, and generate a buy signal as soon as the price manages to break out from it's previous high. 
After that we do nothing while the price is ricing, but as soon as it drops more than a certain amount, we sell. 

We specify how a breakout is defined for this to work. We look at the closing process for the last three months and what ever price is the highest is out breakout level.


import numpy as np 

class CalmYellowGreenDogfish(QCAlgorithm):

    def Initialize(self):
        self.SetCash(100000) 
        
        self.SetStartDate(2017, 12, 1)  
        self.SetEndDate(2021, 12, 1) 
        
        self.symbol= self.AddEquity("SPY", Resolution.Daily).symbol
        
        self.lookback = 20
        
        self.ceiling = 30
        self.floor = 10
        
        self.initialStopRisk = 0.98
        self.trailingStopRisk = 0.9
        
        self.Schedule.On(self.DateRules.EveryDay(self.symbol),\
                        self.TimeRules.AfterMarketOpen(self.symbol, 20),\ 
                        Action(self.EveryMarketOpen))


    def OnData(self, data):
        self.Plot("Data Chart", self.symbol, self.Securities[self.symbol].Close)
        
    def EveryMarketOpen(self):    
        close = self.History(self.Symbol, 31, Resolution.Daily)["close"]
        todayvol = np.std(close[1:31])
        yesterdayvol = np.std(close[0:30])
        deltavol = (todayvol - yesterdayvol) / todayvol
        self.lookback = round(self.lookback * (1 + deltavol))
        
        if self.lookback > self.ceiling:
            self.lookback = self.ceiling
        elif self.lookback < self.floor:
            self.lookback = self.floor
            
        self.high = self.History(self.symbol, self.lookback, Resolution.Daily["high"])
        
        if not self.Securities[self.symbol].Invested and \
                self.Securities[self.symbol].Close >= max(self.high[:-1]):
            self.SetHoldings(self.symbol, 1)
            self.breakoutlvl = max(self.high[:-1])
            self.highestPrice = self.breakoutlvl
        
        if self.Securities[self.symbol].Invested:
            if not self.Transactions.GetOpenOrders(self.symbol):
                self.stopMarketTicket = self.StopMarketOrder(self.symbol, \
                                        -self.Portfolio[self.symbol].Quantity, \
                                        self.initialStopRisk * self.breakoutlvl)
            if self.Securities[self.symbol].Close > self.highestPrice and \
                    self.initialStopRisk * self.breakoutlvl < self.Securities[self.symbol].Close * self.trailingStopRisk:
                self.highestPrice = self.Securities[self.symbol].Close
                updateFields = UpdateOrderFields()
                updateFields.StopPrice = self.Securities[self.symbol].Close * self.trailingStopRisk 
                self.stopMarketTicket.Update(updateFields)
                
                self.Debug(updateFields.StopPrice)
                
            self.Plot("Data Chart", "Stop Price", self.stopMarketTicket.Get(OrderField.StopPrice)) 
            

<img width="944" alt="Screenshot 2021-12-16 at 16 36 49" src="https://user-images.githubusercontent.com/55410500/146645756-9c7f08b0-7d35-4b94-8369-0019eae40e99.png">
            
<img width="958" alt="Screenshot 2021-12-16 at 16 42 06" src="https://user-images.githubusercontent.com/55410500/146645759-0d38a8b0-5b09-485d-b17b-aa2d0e681747.png">



The project is a final project in the course Crypotcurrencies STÆ532M2019H. Work is done by sbm11
