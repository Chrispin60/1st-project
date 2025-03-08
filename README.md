# 1st-project
A Yahoo web scraper in Python. This is my first code and my first time using GitHub. I am learning how to use this website as you read this. Please review my web scraper for a few stocks I currently own below. As I learn more, I plan to add it to my portfolio.

import yfinance as yf
import time

# List of stocks I own and Track daliy
stock_symbols = ["NVDA", "TSLA", "BA", "CRWD", "BRK.B"]

# Dictionary to store the opening prices
opening_prices = {}


def get_stock_price(stock_symbol):
    try:
        stock = yf.Ticker(stock_symbol)
        history = stock.history(period="1d")  # Get daily data

        # Get the latest closing price
        latest_price = history["Close"][-1]
        # Get the opening price
        open_price = history["Open"][-1]

        # Store the opening price if not already stored
        if stock_symbol not in opening_prices:
            opening_prices[stock_symbol] = open_price

        # Calculate the price difference
        price_diff = latest_price - opening_prices[stock_symbol]

        # Alert if price drops $4 or more
        alert = ""
        if price_diff <= -4:
            alert = f"⚠️ ALERT: {stock_symbol} dropped ${abs(price_diff):.2f} from open!"

        return f"{stock_symbol}: ${latest_price:.2f} (Open: ${opening_prices[stock_symbol]:.2f}) {alert}"

    except Exception as e:
        return f"Error fetching {stock_symbol}: {e}"


# Run the tracker continuously
while True:
    print("\nFetching stock prices...\n")

    for symbol in stock_symbols:
        print(get_stock_price(symbol))

    print("\nWaiting 1 second for next update...\n")
    time.sleep(1)  # Update every second (adjust as needed)





The only "Error code" I do have is the following: FutureWarning: Series.__getitem__ treating keys as positions is deprecated. In a future version, integer keys will always be treated as labels (consistent with DataFrame behavior). To access a value by position, use `ser.iloc[pos]`
  open_price = history["Open"][-1]

I believe that if this were on a server instead of my personal laptop, it could lead to information leakage, but I'm not too sure.
