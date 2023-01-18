# Create News Service
In this section, we create our first news service app. The news service app is based of Flask API and calls the  [NewsAPI](https://newsapi.org/) to generate top-news by country. The news service app will be configured on port:3003 

## Sample request/response
A request involves 2 attributes:

* ***country*** : This is the 2 letter code for the country
* ***API key*** : This can be generated from the website. I am using the original author's 

A sample request/response will look like this : 

* Sample request for all news for the day in US 
```
https://newsapi.org/v2/top-headlines?country=US&apiKey=70dcd1c6a0d24ebdb57ff071ff9b8ddc
```
* Sample response for this request (JSON)
```
StatusCode        : 200
StatusDescription : OK
Content           : {
                      "articles": [
                        {
                          "author": "Amy Gardner, Dan Rosenzweig-Ziff", 
                          "content": "Comment on this story\r\nThe arrest of a defeated candidate for the New Mexico legislature on charges tha...
RawContent        : HTTP/1.1 200 OK
                    Connection: keep-alive
                    Content-Length: 18926
                    Content-Type: application/json
                    Date: Wed, 18 Jan 2023 03:16:42 GMT
                    Server: nginx/1.23.3
                    
                    {
                      "articles": [
                        {
                          "author": "Am...
```
## Create news service
Create the file `news.py` and copy the following contents

```
from flask import Flask, request
from flask_restful import Api
import json
import requests

app = Flask(__name__)
api = Api(app)
@app.route('/news')
def news():
    # sample request: https://newsapi.org/v2/top-headlines?country=US&apiKey=70dcd1c6a0d24ebdb57ff071ff9b8ddc
    country_name = request.args.get('country')
    api_key = "70dcd1c6a0d24ebdb57ff071ff9b8ddc"
    base_url = "http://newsapi.org/v2/top-headlines?"
    complete_url = base_url + "country=" + country_name + "&apiKey=" + api_key
    response = requests.get(complete_url)
    return response.json()

if __name__ == '__main__':
    app.run(host="0.0.0.0",port='3003',threaded=True,debug=True)
```
## Create Dockerfile
We will create a new dockerfile called `Dockerfile.news`. This will be referenced in the `docker-compose.yml` file. Copy the following contents into `Dockerfile.news`.

```
# use Python 3.11 image
FROM python:3.11-alpine

# install dependencies:
COPY requirements.txt .
RUN pip install -r requirements.txt
EXPOSE 3003
COPY news.py .

# run the application:
CMD ["python", "news.py"]
```
This will tell docker to:

* Use python base image 3.11-alpine 
* Reference `requirements.txt` in the image to install required software 
* Expose port 3003
* Copy `news.py` to the image
* Start the news service



