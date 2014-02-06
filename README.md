Current version development by pulsecat, javdejong, shadww, D-Nice

Rudimentary backtesting by javdejong

Limit orders by shadww ==> UPDATE: found a bug which will not update the price, a fix is proposed but needs more testing

Simpler and streamlined backtesting by D-Nice 


If you found this tool helpful, consider donating:

D-Nice: 1DNiceqwCXFS9rxVddVGR2f6qtbwZiwLNV

pulsecat: cryptotrader.org (consider subscribing there :D)

javdejong: 

shadww: 149UTnSMy8nUc4qhsBsAiUXPE26XFQEhB8

__________________________________________________
## Install

    git clone https://github.com/shadww/cryptrade.git
    cd cryptrade
    npm install (or sudo npm install if you are having trouble with npm update later on)

## Usage

For backtesting type

    ./backtest.sh examples/tradingscript.coffee

By default, it will use all the settings in your config.cson, and that is all you need for the backtest, however, it will also accept command-line arguments

    Usage: backtrade.coffee [options] <filename>

    Options:

      -h, --help               output usage information
      -c,--config [value]      Load configuration file
      -p,--platform [value]    Trade at specified platform
      -i,--instrument [value]  Trade instrument (ex. btc_usd)
      -s,--initial [value]     Number of trades that are used for initialization (ex. 248)
      -b,--balance <asset,curr>Initial balances of trade instrument (ex. 0,5000)
      -f,--fee [value]         Fee on every trade in percent (ex. 0.2)
      -a,--add_length [value]  Additional initial periods to include (default: 100)
      

Backtesting on this version works correctly.
Limit orders have been tested (by shadww) to sell on live data and works fine (sell instrument,amount,price,timeout).
If you face any issue in a specific script please share it with us :)

Also, if somebody is using the live version of cryptotrader.org, it would be very helpful if you could capture the request when turning offset on to a certain value. If you do that, I could add offsets to cryptrade, assuming they've implemented it within their API. I do have an alternative to offseting as well though, although it would be better to come straight off their API.

## FAQ

Q: The backtest is buying and selling even when my portfolio is empty

A: You must include the following code or some other variation, within your trading script, that ensures it does not buy or sell if needed balance isn't available. Example in EMA_10_21
Beginning of my EMA Script:

    class Functions
       @can_buy: (ins, min_btc, fee_percent) ->
       portfolio.positions[ins.curr()].amount >= ((ins.price * min_btc) * (1 + fee_percent / 100))
       @can_sell: (ins, min_btc) ->
       portfolio.positions[ins.asset()].amount >= min_btc
       
End where functions are called:

    if diff > context.buy_treshold and Functions.can_buy(instrument, .01, .2)         
        buy instrument # Spend all amount of cash for BTC
    else
        if diff < -context.sell_treshold and Functions.can_sell(instrument, .01) 
            sell instrument # Sell BTC position
      

Q: How do I change the dates for the backtest?

A: Change the 'init_data_length' in config.cson to the desired length or change the value of -s when you run the backtest (example: ./backtest.sh -s 500 examples/tradingscript.coffee).

Original Readme can be found here: https://github.com/pulsecat/cryptrade
