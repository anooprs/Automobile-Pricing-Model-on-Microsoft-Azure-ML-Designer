```python
import urllib.request
import json
import os
import ssl

def allowSelfSignedHttps(allowed):
    # bypass the server certificate verification on client side
    if allowed and not os.environ.get('PYTHONHTTPSVERIFY', '') and getattr(ssl, '_create_unverified_context', None):
        ssl._create_default_https_context = ssl._create_unverified_context

allowSelfSignedHttps(True) # this line is needed if you use self-signed certificate in your scoring service.

data = {
    "Inputs": {
          "WebServiceInput0":
          [
              {
                    'CulmenLength': "39.1",
                    'CulmenDepth': "18.7",
                    'FlipperLength': "181",
                    'BodyMass': "3750",
              },
          ],
    },
    "GlobalParameters":  {
    }
}

body = str.encode(json.dumps(data))

url = 'http://13.90.45.204:80/api/v1/service/predict-penguin-clusters/score'
api_key = 'm0ncmRI8u6kvd97q4slgQTsfKIk5eSpx' # Replace this with the API key for the web service
headers = {'Content-Type':'application/json', 'Authorization':('Bearer '+ api_key)}

req = urllib.request.Request(url, body, headers)

try:
    response = urllib.request.urlopen(req)

    result = response.read()
    print(result)
except urllib.error.HTTPError as error:
    print("The request failed with status code: " + str(error.code))

    # Print the headers - they include the requert ID and the timestamp, which are useful for debugging the failure
    print(error.info())
    print(json.loads(error.read().decode("utf8", 'ignore')))
```

    b'{"Results": {"WebServiceOutput0": [{"CulmenLength": 0.25454545454545463, "CulmenDepth": 0.6666666666666665, "FlipperLength": 0.15254237288135597, "BodyMass": 0.29166666666666674, "Assignments": 1, "DistancesToClusterCenter no.0": 0.40912920968252825, "DistancesToClusterCenter no.1": 0.16110565519474315, "DistancesToClusterCenter no.2": 0.9455736711433625}]}}'



```python

```
