# Perpetual Market Making

```
Abu Toha Md Faisal  
May 23, 2022 
```

## Intro  

This strategy allows Hummingbot users to run a market making strategy on a single trading pair on a perpetuals swap (`perp`) order book exchange.

Similar to the `pure_market_making` strategy, the `perpetual_market_making` strategy keeps placing limit buy and sell orders on the order book and waits for other participants (takers) to fill its orders. But unlike market making on spot markets, where assets are being exchanged, market making on perpetual markets creates and closes positions. Since outstanding perpetual swap positions are created after fills, the strategy has a number of parameters to determine when positions are closed to take profits and prevent losses.


## Strategy Configs

There are total 32 parameters in `perpetual_market_making` strategy:


(1)  
```
Parameter:  strategy  
Prompt:     What is your market making strategy?
>>>         perpetual_market_making
```


(2)  
```
Parameter:  derivative
Prompt:     Enter your maker derivative connector
>>>         binance_perpetual_testnet
```


(3)  
```
Parameter:  market
Prompt:     Enter the token trading pair you would like to trade on [exchange]
>>>         BTC-USDT
```


(4)  
```
Parameter:  leverage 
Prompt:     How much leverage do you want to use?               
>>>         (Binance perpetuals supports up to 75X for most pairs)
```


In crypto trading, leverage refers to using borrowed capital to make trades. Leverage trading can amplify your buying or selling power, allowing you to trade larger amounts. So even if your initial capital is small, you can use it as collateral to make leveraged trades. While leveraged trading can multiply your potential profits, it is also subject to high risk - especially in the volatile crypto market. Be careful when using leverage to trade crypto. It may lead to substantial losses if the market moves against your position.

For example, imagine that you have $100 in your exchange account but want to open a position worth $1,000 in bitcoin (BTC). With a 10x leverage, your $100 will have the same buying power as $1,000.


Leverage can be applied to both long and short positions. Opening a long position means that you expect the price of an asset to go up. In contrast, opening a short position means that you believe the price of the asset will fall. While this may sound like regular spot trading, using leverage allows you to buy or sell assets based on your collateral only and not on your holdings. So, even if you don’t have an asset, you can still borrow it and sell (open a short position) if you think the market will go lower.

Example of a leveraged long position:  
Imagine you want to open a long position of $10,000 worth of BTC with 10x leverage. This means that you will use $1,000 as collateral. If the price of BTC goes up 20%, you will earn a net profit of $2,000 (minus fees), which is much higher than the $200 you would have made if you traded your $1,000 capital without using leverage.

However, if the BTC price drops 20%, your position would be down $2,000. Since your initial capital (collateral) is only $1,000, a 20% drop would cause a liquidation (your balance goes to zero). In fact, you could get liquidated even if the market only drops 10%. The exact liquidation value will depend on the exchange you are using.

To avoid being liquidated, you need to add more funds to your wallet to increase your collateral. In most cases, the exchange will send you a margin call before the liquidation happens (e.g., an email telling you to add more funds).

Example of a leveraged short position:  
Now, imagine that you want to open a $10,000 short position on BTC with 10x leverage. In this case, you will borrow BTC from someone else and sell it at the current market price. Your collateral is $1,000, but since you are trading on 10x leverage, you are able to sell $10,000 worth of BTC.

Assuming the current BTC price is $40,000, you borrowed 0.25 BTC and sold it. If the BTC price drops 20% (down to $32,000), you can buy back 0.25 BTC with just $8,000. This would give you a net profit of $2,000 (minus fees).

However, if BTC rises 20% to $48,000, you would need an extra $2,000 to buy back the 0.25 BTC. Your position will be liquidated as your account balance only has $1,000. Again, to avoid being liquidated, you need to add more funds to your wallet to increase your collateral before the liquidation price is reached.


Link: https://academy.binance.com/en/articles/what-is-leverage-in-crypto-trading



(5)  
```
Parameter:  position_mode   
Prompt:     Which position mode do you want to use? (One-way/Hedge) 
>>>         One-way
```

One-Way Mode  
In one-way mode, you can only hold positions in one direction under one contract.
For example, if you open a short position and anticipate that the price will go down in the longer timeframe, but in the meanwhile, you also want to open a long position for a shorter time frame, you won’t be able to open positions in both directions at the same time. Opening positions in both directions would result in canceling one another out.

Hedge Mode  
In hedge mode, you can hold positions in both long and short directions simultaneously under the same contract.
For example, you can hold both long and short positions of the BTC-USDT contract at the same time.

(6)  
```
Parameter:    bid_spread
Prompt:        How far away from the mid price do you want to place the first bid order?
>>>        
```

(7)  
```
Parameter:  ask_spread
Prompt:     How far away from the mid price do you want to place the first ask order?
>>>
```

Tips for choosing the right spread:

It is important to choose the right spread that balances liquidity mining rewards and having fewer filled trades (hence reducing transaction fees). Too small spread will increase the risk of fill orders, and as a result, the rewards can not cover the loss due to the transaction fees. Excessive spreads will lead to a drop in yield or even inability to get rewards (each token has an agreed-upon maximum spread for mining rewards).

Market is uptrend:  
Set tighter bid spreads and wider ask spreads so that you are holding more of the base asset when the market is going uptrend. For example, set inventory skew to hold more of the base asset

Market is downtrend:  
Set tighter ask spreads and wider bid spreads so that you are holding less of the base asset when the market is going downtrend. For example, you can set inventory skew to hold less of the base asset.

https://hummingbot.io/en/academy/basic-concepts-of-crypto-trading/

(8)
```
Parameter:    minimum_spread
Prompt:        At what minimum spread should the bot automatically cancel orders?
>>>        -100
```

If the spread of any active order fall below this param value, it will be automatically cancelled.


(9)
```
Prompt:    How often do you want to cancel and replace bids and asks (in seconds)?
Parameter:    order_refresh_time
>>>        120
```

(10)
```
Parameter:    order_refresh_tolerance_pct
Prompt:        Enter the percent change in price needed to refresh orders at each cycle
>>>        
```

The spread (from mid price) to defer order refresh process to the next cycle.

(11)
Parameter:    order_amount
Prompt:        What is the amount of [base_asset] per order?
>>>        


(12)
```
Parameter:    long_profit_taking_spread
Prompt:        At what spread from the entry price do you want to place a short order to reduce position?
>>>        
```

Short Position:
Shorting (or short selling) means selling an asset in the hopes of rebuying it later at a lower price. A trader who enters a short position expects the asset’s price to decrease, meaning that they are “bearish” on that asset.



(13)
```
Parameter:    short_profit_taking_spread
Prompt:        At what spread from the position entry price do you want to place a long order to reduce position?
>>>        
```

Long Position:
The opposite of a short position is a long position, where a trader buys an asset in the hopes of selling it later at a higher price.


(14)
```
Parameter:    stop_loss_spread
Prompt:        At what spread from position entry price do you want to place stop_loss order?
>>>        
```

The Stop Order on Binance Futures is a combination of stop-loss and take-profit orders. The system will decide if an order is a stop-loss order or a take-profit order based on the price level of trigger price against the last price or mark price when the order is placed.



Important Notes:
Binance uses Mark Price as a trigger for liquidation and to measure unrealized profit and loss.


(15)
```
Parameter:    time_between_stop_loss_orders
Prompt:        How much time should pass before refreshing a stop loss order that has not been executed? (in seconds)
>>>        
```

(16)
```
Parameter:    stop_loss_slippage_buffer
Prompt:    How much buffer should be added in stop loss orders' price to account for slippage (Enter 1 for 1%)?
>>>        
```

(17)
```
Parameter:    price_ceiling
Prompt:        Enter the price point above which only sell orders will be placed
>>>        
```

Place only sell orders when mid price goes above this price.


(18)
```
Parameter:    price_floor
Prompt:        Enter the price below which only buy orders will be placed
>>>        
```

Place only buy orders when mid price falls below this price.

(19)
```
Parameter:    order_levels
Promt:        How many orders do you want to place on both sides?
>>>        
```

The number of order levels to place for each side of the order book.


(20)
```
Parameter:    order_level_amount
Promt:        How much do you want to increase or decrease the order size for each additional order?
>>>        
```

The size can either increase(if set to a value greater than zero) or decrease (if set to a value less than zero) for subsequent order levels after the first level.


(21)
```
Parameter:    order_level_spread
Promt:        Enter the price increments (as percentage) for subsequent orders?
>>>        
```

The incremental spread increases for subsequent order levels after the first level.

(22)
```
Parameter:    filled_order_delay
Promt:        How long do you want to wait before placing the next order if your order gets filled (in seconds)?
>>>        
```

How long to wait before placing the next set of orders in case at least one of your orders gets filled.

(23)
```
Parameter:    order_optimization_enabled
Promt:        Do you want to enable best bid ask jumping? (Yes/No)
>>>        
```

Allows your bid and ask order prices to be adjusted based on the current top bid and ask prices in the market.

(24)
Promt:        How deep do you want to go into the order book for calculating the top ask, ignoring dust orders on the top (expressed in base asset amount)?
Parameter:    ask_order_optimization_depth
>>>        

The depth in base asset amount to be used for finding top bid ask.
        

Promt:        How deep do you want to go into the order book for calculating the top bid, ignoring dust orders on the top (expressed in base asset amount)?
Parameter:    bid_order_optimization_depth
>>>        

The depth in base asset amount to be used for finding top bid.

 Promt:        Which price source to use? (current_market/external_market/custom_api)
Parameter:    price_source
>>>        

Promt:        Which price type to use? (mid_price/last_price/last_own_trade_price/best_bid/best_ask)
Parameter:    price_type
>>>        

https://hummingbot.io/en/blog/2020-11-commands-and-config-price-source



Promt:        Enter external price source connector name or derivative name
Parameter:    price_source_derivative
>>>        

           
   
Promt:        Enter the token trading pair on [external_market]
Parameter:    price_source_market
>>>        

Promt:        Enter pricing API URL
Parameter:    price_source_custom_api
>>>        

Promt:        Enter custom API update interval in second (default: 5.0, min: 0.5)
Parameter:    custom_api_update_interval
>>>        


            

   
Promt:        
Parameter:    order_override
>>>                
                 
                
          
          
          
  


                
                  
     
         
     
  
              




Concepts:
Inventory risk is the probability a market maker can't find buyers for his inventory, resulting in the risk of holding more of an asset at exactly the wrong time, e.g. accumulating assets when prices are falling or selling too early when prices are rising.




Link:
A Complete Guide to Cryptocurrency Trading for Beginners

