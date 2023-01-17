```
PS C:\Users\aniru\workspace\github\nginx-docker> curl https://api.openweathermap.org/data/2.5/weather?appid=deb96cff96df7c74e93e62661b91c3c2'&'q=new%20york


StatusCode        : 200
StatusDescription : OK
Content           : {"coord":{"lon":-74.006,"lat":40.7143},"weather":[{"id":800,"main":"Clear","descri
                    ption":"clear sky","icon":"01n"}],"base":"stations","main":{"temp":276.47,"feels_l
                    ike":271.84,"temp_min":273.13,"temp_...
RawContent        : HTTP/1.1 200 OK
                    Connection: keep-alive
                    X-Cache-Key: /data/2.5/weather?q=new%20york
                    Access-Control-Allow-Origin: *
                    Access-Control-Allow-Credentials: true
                    Access-Control-Allow-Methods: GET, POST
                    C...
Forms             : {}
Headers           : {[Connection, keep-alive], [X-Cache-Key, /data/2.5/weather?q=new%20york],
                    [Access-Control-Allow-Origin, *], [Access-Control-Allow-Credentials, true]...}
Images            : {}
InputFields       : {}
Links             : {}
ParsedHtml        : mshtml.HTMLDocumentClass
RawContentLength  : 474
```