# trading-bot

This is a simple python trading algorithm built in the QuantConnect platform, which is algorithmic trading platform.

In this algorithm we look at a past high, and generate a buy signal as soon as the price manages to break out from it's previous high. 
After that we do nothing while the price is ricing, but as soon as it drops more than a certain amount, we sell. 

We specify how a breakout is defined for this to work. We look at the closing process for the last three months and what ever price is the highest is out breakout level.




The project is a final project in the course Crypotcurrencies STÆ532M2019H. Work is done by sbm11
