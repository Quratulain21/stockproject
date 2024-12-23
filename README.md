import yfinance as yf

class SimpleStockPortfolio:
    def __init__(self):
        self.portfolio = {}

    def add_stock(self, symbol, shares):
        if symbol in self.portfolio:
            self.portfolio[symbol] += shares
        else:
            self.portfolio[symbol] = shares
        print(f"Added {shares} shares of {symbol}.")

    def remove_stock(self, symbol, shares):
        if symbol in self.portfolio:
            if self.portfolio[symbol] >= shares:
                self.portfolio[symbol] -= shares
                print(f"Removed {shares} shares of {symbol}.")
                if self.portfolio[symbol] == 0:
                    del self.portfolio[symbol]
            else:
                print(f"Not enough shares to remove. You have {self.portfolio[symbol]} shares.")
        else:
            print(f"{symbol} is not in your portfolio.")

    def view_portfolio(self):
        if not self.portfolio:
            print("Your portfolio is empty.")
            return
        total_value = 0.0
        print("\nYour Portfolio:")
        for symbol, shares in self.portfolio.items():
            stock = yf.Ticker(symbol)
            current_price = stock.history(period="1d")['Close'].iloc[-1]
            total_value += current_price * shares
            print(f"{symbol}: {shares} shares at ${current_price:.2f} each, Total: ${current_price * shares:.2f}")
        print(f"Total portfolio value: ${total_value:.2f}")

def main():
    portfolio = SimpleStockPortfolio()
    while True:
        print("\n1. Add Stock")
        print("2. Remove Stock")
        print("3. View Portfolio Value")
        print("4. Exit")
        choice = input("Choose an option: ")

        if choice == '1':
            symbol = input("Enter stock symbol: ").upper()
            shares = int(input("Enter number of shares: "))
            portfolio.add_stock(symbol, shares)
        elif choice == '2':
            symbol = input("Enter stock symbol: ").upper()
            shares = int(input("Enter number of shares to remove: "))
            portfolio.remove_stock(symbol, shares)
        elif choice == '3':
            portfolio.view_portfolio()
        elif choice == '4':
            print("Exiting the program.")
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
