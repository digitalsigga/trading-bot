# trading-bot

This is a simple python trading algorithm built in the QuantConnect platform.

QuantConnect provides a free algorithm backtesting tool and financial data so engineers can design algorithmic trading strategies. 

In this algorithm we look at a past high, and generate a buy signal as soon as the price manages to break out from it's previous high. 
After that we do nothing while the price is ricing, but as soon as it drops more than a certain amount, we sell. 

We specify how a breakout is defined for this to work. We look at the closing process for the last three months and what ever price is the highest is out breakout level.

            

<img width="944" alt="Screenshot 2021-12-16 at 16 36 49" src="https://user-images.githubusercontent.com/55410500/146645756-9c7f08b0-7d35-4b94-8369-0019eae40e99.png">
            
<img width="958" alt="Screenshot 2021-12-16 at 16 42 06" src="https://user-images.githubusercontent.com/55410500/146645759-0d38a8b0-5b09-485d-b17b-aa2d0e681747.png">



The project is a final project in the course Crypotcurrencies STÆ532M2019H. Work is done by sbm11
