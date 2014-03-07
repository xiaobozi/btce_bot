A brief description of each of the packages, for more information, please check the README.md in the package.

#### [macd_sell_signal](macd_sell_signal)
This package subscribes to a ticker topic and publishes a sell signal which is a boolean along with macd values at the current time(TODO). It utilizes a C++ object called [macd_indicator](macd_sell_signal/src/macd_indicator.cpp). This object is used to handle the MACD calculations. This package also provides a service that allows a node to ask for previous macd values. This is mostly used so the plotting node can ask for historical data and plot at it's leisure instead of publishing all of this data to a topic. It also is a client for a service that recalls historical data from a file. The idea is that the ticker/save nodes should always be running, and any MACD node should be able to be started at any point and have valid numbers for the filters, there won't be any settling time.
- Publishers  
  - [/macd_short_long_sig_xperiod/macd](macd_sell_signal/msg/macd.msg)   -   e.g. macd_12_26_9_x10/macd
  - [/macd_short_long_sig_xperiod/sell](macd_sell_signal/msg/sell.msg)   -   e.g. macd_12_26_9_x10/sell
- Subscribers
  - [/ticker_trade_pair](ticker_publisher/msg/ticker.msg)
- Services (Server)
  - [/macd_short_long_sig_xperiod/macd_array](macd_sell_signal/srv/macd_array.srv) -   e.g. macd_12_26_9_x10/macd_array
- Services (Client)
  - [/ticker_trade_pair_history](save_load_ticker/srv/history.srv)

#### [plot_macd](plot_macd)
This package makes a request via a service every 5 seconds to receive the latest MACD/ticker data from the macd_sell_signal package. It then uses matplotlib to provide a graphical display of EMAs, MACD lines and current price.
- Publishers - None
- Subscribers - None
- Services (Server) - None
- Services (Client)
  - [/macd_short_long_sig_x_period/macd_array](macd_sell_signal/srv/macd_array.srv) - e.g. macd_12_26_9_x10/macd_array

#### [save_load_ticker](save_load_ticker)
This package subscribes to all of the current ticker topics and saves their data to a file. It checks every second for new topics so if a new pair is added it will pick it up and start collecting data. It also provides a service to any node that would like historical data for a certain pair. This is mainly used when a MACD node is started.
- Publishers - None
- Subscribers - [/ticker_trade_pair](ticker_publisher/msg/ticker/msg)
- Services (Server)
  - [/ticker_trade_pair_history](save_load_ticker/srv/history.srv)
- Services (Client) - None

#### [sell_signal_filter](sell_signal_filter)
This package subscribes to all of the current MACD topics' sell signals. In the event of a sell signal from any of the nodes, it does a bunch of double checking to make sure it's really ok to go ahead with a sell.
- Publishers - None (yet)
- Subscribers - [/macd_short_long_sig_xperiod/sell](macd_sell_signal/msg/sell.msg)
- Services (Server) - None
- Services (Client) - None

#### [ticker_publisher](ticker_publisher) 
This package grabs ticker data from the btc-e site and publishes to a topic based on what the trade pair is. For example, the trade pair BTC/USD will publish on the topic /ticker_btc_usd.
- Publishers  
  - [/ticker_trade_pair](ticker_publisher/msg/ticker.msg) - e.g. /ticker_btc_usd
- Subscribers - None
- Services(Server/Client) - None

#### [user_interface](user_interface)
This package provides a convienient command line utility to start new MACD topics and show plots.