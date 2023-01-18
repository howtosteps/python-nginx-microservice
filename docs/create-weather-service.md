# Create Weather Service
Now we will create the weather service app. The news service app is also based of Flask API and simply calls the  [WeatherAPI](https://openweathermap.org/api). The weather service app will be configured on port:3002 

## Sample request/response
A request involves 2 attributes :
* appid : This is the 2 letter code for the country
* q : Represents the city name

Sample request/response will look like this:
* Sample request for the weather in New York city
```
PS C:\Users\aniru\workspace\github\nginx-docker> curl https://api.openweathermap.org/data/2.5/weather?appid=deb96cff96df7c74e93e62661b91c3c2'&'q=new%20york
```
* Sample response for the above request :  
```
{"coord":{"lon":-74.006,"lat":40.7143},"weather":[{"id":804,"main":"Clouds","description":"overcast clouds","icon":"04n"}],"base":"stations","main":{"temp":279.94,"feels_like":276.89,"temp_min":277.88,"temp_max":281.12,"pressure":1005,"humidity":53},"visibility":10000,"wind":{"speed":4.63,"deg":230},"clouds":{"all":100},"dt":1674012646,"sys":{"type":2,"id":2008101,"country":"US","sunrise":1673957823,"sunset":1673992477},"timezone":-18000,"id":5128581,"name":"New York","cod":200}
```
## Create weather service
Create file `weather.py` and copy the following contents :
```
from flask import Flask, request
from flask_restful import Api
import json
import requests

app = Flask(__name__)
api = Api(app)
@app.route('/weather')
def weather():
    # sample request: http://api.openweathermap.org/data/2.5/weather?appid=deb96cff96df7c74e93e62661b91c3c2&q=new%20york
    city_name = request.args.get('city')
    api_key = "deb96cff96df7c74e93e62661b91c3c2"
    base_url = "http://api.openweathermap.org/data/2.5/weather?"
    complete_url = base_url + "appid=" + api_key + "&q=" + city_name
    response = requests.get(complete_url)
    return response.json()

if __name__ == '__main__':
    app.run(host="0.0.0.0",port='3002',threaded=True,debug=True)
```

This tells weather app to parse any incoming request and pass on the request to the OpenWeather API. The app returns the response returned by the weather API as is. 

## Create Dockerfile
We will create a new dockerfile called `Dockerfile.weather`. This will be referenced in the `docker-compose.yml` file. Copy the following contents into `Dockerfile.weather`.
```
# use Python 3.11 image
FROM python:3.11-alpine

# install dependencies:
COPY requirements.txt .
RUN pip install -r requirements.txt
EXPOSE 3002
COPY weather.py .

# run the application:
CMD ["python", "weather.py"]
```

This will tell docker to:

* Use python base image 3.11-alpine 
* Reference `requirements.txt` in the image to install required software 
* Expose port 3002
* Copy `news.py` to the image
* Start the weather service