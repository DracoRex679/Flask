from flask import Flask, render_template, request
import requests
from bs4 import BeautifulSoup
import html5lib
import webbrowser
import time
import datetime
import json

app = Flask(__name__)


@app.route("/")
def home():
    return render_template("homepage.html")


@app.route("/stocks")
def stock_item():
    query = True
    item = request.args.get('q')
    if item:
        stock_name = str(item)
        date = (datetime.date.today())
        day = date.weekday()
        print(day)
        if day == 0:
            yesterday = date - datetime.timedelta(days=3)
        elif day == 6:
            yesterday = date - datetime.timedelta(days=2)
        else:
            yesterday = date - datetime.timedelta(days=1)
        yesterday = yesterday.strftime("%Y-%m-%d")
        #Add your Apikey for Alphavantage
        response = requests.get(f"https://www.alphavantage.co/query?function=TIME_SERIES_DAILY&symbol={item}&apikey=#Your API Key")
        response.raise_for_status()
        econ_info = response.json()
        stock_info = econ_info["Time Series (Daily)"]
        open = stock_info[yesterday]['1. open']
        high = stock_info[yesterday]['2. high']
        low = stock_info[yesterday]['3. low']
        close = stock_info[yesterday]['4. close']
        volume = stock_info[yesterday]['5. volume']
        return render_template("homepage.html", open=open, high=high, low=low, close=close, volume=volume,
                               show_data=True, query=True, stock=stock_name)
    return render_template("homepage.html", query=True)


@app.route("/weather")
def weather():
    OWM_Endpoint = "https://api.openweathermap.org/data/2.5/onecall"
    #Put your api_key for openweathermap.
    api_key = "Your API Key"

    weather_params = {
        "lat": 39.72021366541054,
        "lon": -104.87234606087752,
        "appid": api_key,
        "exclude": "current,minutely,daily"
    }

    response = requests.get(OWM_Endpoint, params=weather_params)
    response.raise_for_status()
    weather_data = response.json()
    weather_info = weather_data["hourly"][:12]

    return render_template("homepage.html", weather_info=weather_info)


@app.route("/twittertrends")
def twitter_trends():
    table_rows = []
    titles = []
    r = requests.get('https://twitter-trends.iamrohit.in/united-states')
    soup = BeautifulSoup(r.text, 'html5lib')

    for tr in soup.find_all("tr"):
        table_rows.append(tr)

    # print(table_rows)
    for title in table_rows:
        column = (title.find_all("td"))
        if "[]" not in str(column) and "colspan" not in str(column):
            column = (title.find_all("td"))
            titles.append(column[1])
    return render_template("homepage.html", contents=titles)


if __name__ == "__main__":
    app.run(debug=True)
