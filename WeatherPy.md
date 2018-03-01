
# WeatherPy

#### Analysis
* The temperature increases as the latitude approaches the equator and decreases further away from the equator (latitude of equator is 0).

* As the latitude approaches the equatore, Humidity increases.

* As the latitude goes away from the equator, wind speed increases.


```python
# Dependencies
import json
import requests
import random
import pandas as pd
import numpy as np
from citipy import citipy
import matplotlib.pyplot as plt
import seaborn as sns

# Import OpenWeatherMap API key.
from config import api_key
```

### Generate Cities List


```python
# Build a list of cities
cities = []

# Create a list of random latitude, longitude values
lats = np.random.uniform(low=-90.000, high=90.000, size=1500)
lngs = np.random.uniform(low=-180.000, high=180.000, size=1500)

lats_lngs = zip(lats, lngs)

# Create a for loop to get cities by latitudes, longitudes
for lat, lon in lats_lngs:
    city_data = citipy.nearest_city(lat, lon)
    city = city_data.city_name
        
    if city not in cities:
        cities.append(city)
        
print(len(cities))
```

    616


### Perform API Calls


```python
# Weather check on each of the cities using a series of successive API calls.
print("Beginning Data Retrieval")
print("-" * 30)

target_url = 'http://api.openweathermap.org/data/2.5/weather?'
units = 'imperial'

record = 1
cities_data = []
city_char = []

for city in cities:
    # To get city names without space
    for ch in city:
        city_nospace = city.replace(" ", "%20")
    city_char.append(city_nospace)
    
    url = target_url + 'units=' + units + '&APPID=' + api_key + '&q=' + city_nospace
    weather_response = requests.get(url)
    weather_json = weather_response.json()
    if (weather_json["cod"] == 200):
        city_cloudiness = weather_json["clouds"]["all"]
        country = weather_json["sys"]["country"]
        city_date = weather_json["dt"]
        city_humidity = weather_json["main"]["humidity"]
        city_lat = weather_json["coord"]["lat"]
        city_lng = weather_json["coord"]["lon"]
        city_max_temp = weather_json["main"]["temp_max"]
        city_wind_speed = weather_json["wind"]["speed"]
        
    # Append the list to a single dictionary for each parameter 
    cities_data.append({"City": city, 
                        "Cloudiness": city_cloudiness, 
                        "Country": country, 
                        "Date": city_date, 
                        "Humidity": city_humidity, 
                        "Lat": city_lat, 
                        "Lng": city_lng, 
                        "Max Temp": city_max_temp, 
                        "Wind Speed": city_wind_speed})
    print("Processing Record {} | {}".format(record,city))
    print(url)
    record = record + 1 
    
print("-" * 30)
print("Data Retrieval Complete")
print("-" * 30)      
    

```

    Beginning Data Retrieval
    ------------------------------
    Processing Record 1 | farsund
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=farsund
    Processing Record 2 | upernavik
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=upernavik
    Processing Record 3 | kahului
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kahului
    Processing Record 4 | amderma
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=amderma
    Processing Record 5 | okha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=okha
    Processing Record 6 | attawapiskat
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=attawapiskat
    Processing Record 7 | kapaa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kapaa
    Processing Record 8 | curaca
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=curaca
    Processing Record 9 | dakoro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=dakoro
    Processing Record 10 | faanui
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=faanui
    Processing Record 11 | kyrksaeterora
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kyrksaeterora
    Processing Record 12 | katsuura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=katsuura
    Processing Record 13 | labuhan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=labuhan
    Processing Record 14 | qaanaaq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=qaanaaq
    Processing Record 15 | busselton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=busselton
    Processing Record 16 | taolanaro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=taolanaro
    Processing Record 17 | rikitea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=rikitea
    Processing Record 18 | mahroni
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mahroni
    Processing Record 19 | inta
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=inta
    Processing Record 20 | vaini
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=vaini
    Processing Record 21 | iqaluit
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=iqaluit
    Processing Record 22 | atuona
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=atuona
    Processing Record 23 | hobart
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=hobart
    Processing Record 24 | santa marta
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=santa%20marta
    Processing Record 25 | ngukurr
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ngukurr
    Processing Record 26 | punta arenas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=punta%20arenas
    Processing Record 27 | sandpoint
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sandpoint
    Processing Record 28 | panguna
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=panguna
    Processing Record 29 | severomuysk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=severomuysk
    Processing Record 30 | new norfolk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=new%20norfolk
    Processing Record 31 | lebu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=lebu
    Processing Record 32 | ust-ishim
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ust-ishim
    Processing Record 33 | chibuto
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=chibuto
    Processing Record 34 | carnarvon
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=carnarvon
    Processing Record 35 | hermanus
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=hermanus
    Processing Record 36 | umm lajj
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=umm%20lajj
    Processing Record 37 | esil
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=esil
    Processing Record 38 | provideniya
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=provideniya
    Processing Record 39 | vista hermosa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=vista%20hermosa
    Processing Record 40 | tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=tuktoyaktuk
    Processing Record 41 | mataura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mataura
    Processing Record 42 | saint-augustin
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=saint-augustin
    Processing Record 43 | kloulklubed
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kloulklubed
    Processing Record 44 | port macquarie
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=port%20macquarie
    Processing Record 45 | port alfred
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=port%20alfred
    Processing Record 46 | salinopolis
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=salinopolis
    Processing Record 47 | albany
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=albany
    Processing Record 48 | te anau
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=te%20anau
    Processing Record 49 | sohag
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sohag
    Processing Record 50 | wulanhaote
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=wulanhaote
    Processing Record 51 | saldanha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=saldanha
    Processing Record 52 | georgetown
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=georgetown
    Processing Record 53 | kurchum
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kurchum
    Processing Record 54 | bluff
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bluff
    Processing Record 55 | bredasdorp
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bredasdorp
    Processing Record 56 | cape town
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=cape%20town
    Processing Record 57 | los llanos de aridane
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=los%20llanos%20de%20aridane
    Processing Record 58 | geraldton
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=geraldton
    Processing Record 59 | chapais
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=chapais
    Processing Record 60 | rio grande
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=rio%20grande
    Processing Record 61 | tasiilaq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=tasiilaq
    Processing Record 62 | ushuaia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ushuaia
    Processing Record 63 | vardo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=vardo
    Processing Record 64 | craig
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=craig
    Processing Record 65 | hilo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=hilo
    Processing Record 66 | port lincoln
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=port%20lincoln
    Processing Record 67 | fontenay-le-comte
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=fontenay-le-comte
    Processing Record 68 | clyde river
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=clyde%20river
    Processing Record 69 | mehamn
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mehamn
    Processing Record 70 | estreito
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=estreito
    Processing Record 71 | mitsamiouli
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mitsamiouli
    Processing Record 72 | kaitangata
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kaitangata
    Processing Record 73 | ruteng
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ruteng
    Processing Record 74 | norman wells
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=norman%20wells
    Processing Record 75 | viedma
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=viedma
    Processing Record 76 | fort nelson
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=fort%20nelson
    Processing Record 77 | whitianga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=whitianga
    Processing Record 78 | belushya guba
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=belushya%20guba
    Processing Record 79 | castro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=castro
    Processing Record 80 | illoqqortoormiut
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=illoqqortoormiut
    Processing Record 81 | khuzdar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=khuzdar
    Processing Record 82 | ramanathapuram
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ramanathapuram
    Processing Record 83 | mys shmidta
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mys%20shmidta
    Processing Record 84 | maltahohe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=maltahohe
    Processing Record 85 | kruisfontein
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kruisfontein
    Processing Record 86 | erice
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=erice
    Processing Record 87 | uruzgan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=uruzgan
    Processing Record 88 | victoria
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=victoria
    Processing Record 89 | kodiak
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kodiak
    Processing Record 90 | douglas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=douglas
    Processing Record 91 | xuchang
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=xuchang
    Processing Record 92 | kirensk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kirensk
    Processing Record 93 | port elizabeth
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=port%20elizabeth
    Processing Record 94 | airai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=airai
    Processing Record 95 | springbok
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=springbok
    Processing Record 96 | adrar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=adrar
    Processing Record 97 | poole
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=poole
    Processing Record 98 | sentyabrskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sentyabrskiy
    Processing Record 99 | ko samui
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ko%20samui
    Processing Record 100 | yellowknife
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=yellowknife
    Processing Record 101 | calabozo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=calabozo
    Processing Record 102 | linxia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=linxia
    Processing Record 103 | qandahar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=qandahar
    Processing Record 104 | mar del plata
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mar%20del%20plata
    Processing Record 105 | narsaq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=narsaq
    Processing Record 106 | taft
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=taft
    Processing Record 107 | khatanga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=khatanga
    Processing Record 108 | bubaque
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bubaque
    Processing Record 109 | stephenville
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=stephenville
    Processing Record 110 | agua verde
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=agua%20verde
    Processing Record 111 | jamestown
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=jamestown
    Processing Record 112 | iquique
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=iquique
    Processing Record 113 | paris
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=paris
    Processing Record 114 | rosetta
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=rosetta
    Processing Record 115 | hit
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=hit
    Processing Record 116 | oranjemund
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=oranjemund
    Processing Record 117 | kutum
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kutum
    Processing Record 118 | andijon
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=andijon
    Processing Record 119 | quixada
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=quixada
    Processing Record 120 | dikson
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=dikson
    Processing Record 121 | sitka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sitka
    Processing Record 122 | longyearbyen
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=longyearbyen
    Processing Record 123 | gedo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=gedo
    Processing Record 124 | fairbanks
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=fairbanks
    Processing Record 125 | grand river south east
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=grand%20river%20south%20east
    Processing Record 126 | maumere
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=maumere
    Processing Record 127 | chokurdakh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=chokurdakh
    Processing Record 128 | poum
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=poum
    Processing Record 129 | petropavlovsk-kamchatskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=petropavlovsk-kamchatskiy
    Processing Record 130 | yulara
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=yulara
    Processing Record 131 | america dourada
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=america%20dourada
    Processing Record 132 | aflu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=aflu
    Processing Record 133 | kandrian
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kandrian
    Processing Record 134 | nome
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nome
    Processing Record 135 | vaitupu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=vaitupu
    Processing Record 136 | goundam
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=goundam
    Processing Record 137 | cockburn town
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=cockburn%20town
    Processing Record 138 | bogorodskoye
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bogorodskoye
    Processing Record 139 | avera
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=avera
    Processing Record 140 | chengmai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=chengmai
    Processing Record 141 | newport
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=newport
    Processing Record 142 | luderitz
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=luderitz
    Processing Record 143 | tumannyy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=tumannyy
    Processing Record 144 | stryn
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=stryn
    Processing Record 145 | abu dhabi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=abu%20dhabi
    Processing Record 146 | sept-iles
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sept-iles
    Processing Record 147 | saint-philippe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=saint-philippe
    Processing Record 148 | sfantu gheorghe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sfantu%20gheorghe
    Processing Record 149 | karaul
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=karaul
    Processing Record 150 | nikolskoye
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nikolskoye
    Processing Record 151 | buchanan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=buchanan
    Processing Record 152 | ishigaki
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ishigaki
    Processing Record 153 | road town
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=road%20town
    Processing Record 154 | las vegas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=las%20vegas
    Processing Record 155 | paamiut
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=paamiut
    Processing Record 156 | khed brahma
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=khed%20brahma
    Processing Record 157 | porto novo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=porto%20novo
    Processing Record 158 | bambous virieux
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bambous%20virieux
    Processing Record 159 | arraial do cabo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=arraial%20do%20cabo
    Processing Record 160 | buckeye
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=buckeye
    Processing Record 161 | diedorf
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=diedorf
    Processing Record 162 | ryotsu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ryotsu
    Processing Record 163 | nguiu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nguiu
    Processing Record 164 | camacupa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=camacupa
    Processing Record 165 | curup
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=curup
    Processing Record 166 | sayat
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sayat
    Processing Record 167 | bethel
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bethel
    Processing Record 168 | catuday
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=catuday
    Processing Record 169 | san patricio
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=san%20patricio
    Processing Record 170 | lillooet
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=lillooet
    Processing Record 171 | komsomolskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=komsomolskiy
    Processing Record 172 | nefteyugansk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nefteyugansk
    Processing Record 173 | umzimvubu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=umzimvubu
    Processing Record 174 | tessalit
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=tessalit
    Processing Record 175 | chuy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=chuy
    Processing Record 176 | torbay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=torbay
    Processing Record 177 | cidreira
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=cidreira
    Processing Record 178 | tchibanga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=tchibanga
    Processing Record 179 | pevek
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=pevek
    Processing Record 180 | sao filipe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sao%20filipe
    Processing Record 181 | east london
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=east%20london
    Processing Record 182 | kaeo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kaeo
    Processing Record 183 | kuito
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kuito
    Processing Record 184 | senanga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=senanga
    Processing Record 185 | bakchar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bakchar
    Processing Record 186 | zonguldak
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=zonguldak
    Processing Record 187 | fort saint john
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=fort%20saint%20john
    Processing Record 188 | vila
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=vila
    Processing Record 189 | rocha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=rocha
    Processing Record 190 | lorengau
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=lorengau
    Processing Record 191 | meulaboh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=meulaboh
    Processing Record 192 | chara
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=chara
    Processing Record 193 | maniwaki
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=maniwaki
    Processing Record 194 | genhe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=genhe
    Processing Record 195 | thunder bay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=thunder%20bay
    Processing Record 196 | coquimbo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=coquimbo
    Processing Record 197 | najran
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=najran
    Processing Record 198 | brigham city
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=brigham%20city
    Processing Record 199 | thompson
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=thompson
    Processing Record 200 | rapid valley
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=rapid%20valley
    Processing Record 201 | tiksi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=tiksi
    Processing Record 202 | lompoc
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=lompoc
    Processing Record 203 | aksu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=aksu
    Processing Record 204 | xingcheng
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=xingcheng
    Processing Record 205 | pitsunda
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=pitsunda
    Processing Record 206 | oakdale
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=oakdale
    Processing Record 207 | pekan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=pekan
    Processing Record 208 | nizhneyansk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nizhneyansk
    Processing Record 209 | kavaratti
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kavaratti
    Processing Record 210 | andros town
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=andros%20town
    Processing Record 211 | samarai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=samarai
    Processing Record 212 | znamenskoye
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=znamenskoye
    Processing Record 213 | biak
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=biak
    Processing Record 214 | tatarusi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=tatarusi
    Processing Record 215 | chiasso
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=chiasso
    Processing Record 216 | nipawin
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nipawin
    Processing Record 217 | mosquera
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mosquera
    Processing Record 218 | miyako
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=miyako
    Processing Record 219 | prado
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=prado
    Processing Record 220 | severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=severo-kurilsk
    Processing Record 221 | werda
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=werda
    Processing Record 222 | san quintin
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=san%20quintin
    Processing Record 223 | kununurra
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kununurra
    Processing Record 224 | puerto leguizamo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=puerto%20leguizamo
    Processing Record 225 | constitucion
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=constitucion
    Processing Record 226 | padang
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=padang
    Processing Record 227 | innisfail
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=innisfail
    Processing Record 228 | mount gambier
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mount%20gambier
    Processing Record 229 | ye
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ye
    Processing Record 230 | vagay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=vagay
    Processing Record 231 | saint anthony
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=saint%20anthony
    Processing Record 232 | codrington
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=codrington
    Processing Record 233 | sola
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sola
    Processing Record 234 | gwadar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=gwadar
    Processing Record 235 | lushunkou
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=lushunkou
    Processing Record 236 | alice springs
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=alice%20springs
    Processing Record 237 | kampot
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kampot
    Processing Record 238 | barentsburg
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=barentsburg
    Processing Record 239 | dukat
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=dukat
    Processing Record 240 | cayenne
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=cayenne
    Processing Record 241 | avarua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=avarua
    Processing Record 242 | laibin
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=laibin
    Processing Record 243 | prince rupert
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=prince%20rupert
    Processing Record 244 | ancud
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ancud
    Processing Record 245 | touros
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=touros
    Processing Record 246 | maine-soroa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=maine-soroa
    Processing Record 247 | salalah
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=salalah
    Processing Record 248 | mahebourg
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mahebourg
    Processing Record 249 | nhulunbuy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nhulunbuy
    Processing Record 250 | sainte-anne-des-monts
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sainte-anne-des-monts
    Processing Record 251 | srednekolymsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=srednekolymsk
    Processing Record 252 | minna
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=minna
    Processing Record 253 | ribeira grande
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ribeira%20grande
    Processing Record 254 | nuuk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nuuk
    Processing Record 255 | nishihara
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nishihara
    Processing Record 256 | iaciara
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=iaciara
    Processing Record 257 | eureka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=eureka
    Processing Record 258 | usinsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=usinsk
    Processing Record 259 | olafsvik
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=olafsvik
    Processing Record 260 | amur
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=amur
    Processing Record 261 | isangel
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=isangel
    Processing Record 262 | atasu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=atasu
    Processing Record 263 | monrovia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=monrovia
    Processing Record 264 | bairiki
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bairiki
    Processing Record 265 | hualmay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=hualmay
    Processing Record 266 | saint george
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=saint%20george
    Processing Record 267 | barrow
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=barrow
    Processing Record 268 | bac lieu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bac%20lieu
    Processing Record 269 | hithadhoo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=hithadhoo
    Processing Record 270 | breves
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=breves
    Processing Record 271 | binga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=binga
    Processing Record 272 | puerto ayora
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=puerto%20ayora
    Processing Record 273 | kotdwara
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kotdwara
    Processing Record 274 | vanimo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=vanimo
    Processing Record 275 | rovenki
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=rovenki
    Processing Record 276 | bengkulu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bengkulu
    Processing Record 277 | zharkent
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=zharkent
    Processing Record 278 | samusu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=samusu
    Processing Record 279 | rio rancho
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=rio%20rancho
    Processing Record 280 | oeiras do para
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=oeiras%20do%20para
    Processing Record 281 | butaritari
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=butaritari
    Processing Record 282 | yerbogachen
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=yerbogachen
    Processing Record 283 | sterling
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sterling
    Processing Record 284 | galbshtadt
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=galbshtadt
    Processing Record 285 | mabay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mabay
    Processing Record 286 | imeni tsyurupy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=imeni%20tsyurupy
    Processing Record 287 | almaznyy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=almaznyy
    Processing Record 288 | norden
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=norden
    Processing Record 289 | jumla
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=jumla
    Processing Record 290 | ijaki
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ijaki
    Processing Record 291 | hurghada
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=hurghada
    Processing Record 292 | beringovskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=beringovskiy
    Processing Record 293 | mazara del vallo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mazara%20del%20vallo
    Processing Record 294 | ilulissat
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ilulissat
    Processing Record 295 | ixtapa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ixtapa
    Processing Record 296 | taonan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=taonan
    Processing Record 297 | xuddur
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=xuddur
    Processing Record 298 | broome
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=broome
    Processing Record 299 | macaboboni
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=macaboboni
    Processing Record 300 | souillac
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=souillac
    Processing Record 301 | kangaatsiaq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kangaatsiaq
    Processing Record 302 | arawa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=arawa
    Processing Record 303 | svetlaya
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=svetlaya
    Processing Record 304 | belaya gora
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=belaya%20gora
    Processing Record 305 | merauke
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=merauke
    Processing Record 306 | binucayan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=binucayan
    Processing Record 307 | villa carlos paz
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=villa%20carlos%20paz
    Processing Record 308 | cherskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=cherskiy
    Processing Record 309 | ponta delgada
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ponta%20delgada
    Processing Record 310 | lamu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=lamu
    Processing Record 311 | guerrero negro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=guerrero%20negro
    Processing Record 312 | qui nhon
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=qui%20nhon
    Processing Record 313 | rockport
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=rockport
    Processing Record 314 | yar-sale
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=yar-sale
    Processing Record 315 | lima
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=lima
    Processing Record 316 | torbat-e jam
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=torbat-e%20jam
    Processing Record 317 | ulverstone
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ulverstone
    Processing Record 318 | haibowan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=haibowan
    Processing Record 319 | kysyl-syr
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kysyl-syr
    Processing Record 320 | luwuk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=luwuk
    Processing Record 321 | sinjah
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sinjah
    Processing Record 322 | yemelyanovo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=yemelyanovo
    Processing Record 323 | rawson
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=rawson
    Processing Record 324 | kirakira
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kirakira
    Processing Record 325 | palmer
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=palmer
    Processing Record 326 | walvis bay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=walvis%20bay
    Processing Record 327 | riyadh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=riyadh
    Processing Record 328 | karwar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=karwar
    Processing Record 329 | san cristobal
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=san%20cristobal
    Processing Record 330 | nowshera
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nowshera
    Processing Record 331 | kijang
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kijang
    Processing Record 332 | grand bend
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=grand%20bend
    Processing Record 333 | thouars
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=thouars
    Processing Record 334 | lagoa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=lagoa
    Processing Record 335 | pampierstad
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=pampierstad
    Processing Record 336 | lensk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=lensk
    Processing Record 337 | aswan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=aswan
    Processing Record 338 | saskylakh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=saskylakh
    Processing Record 339 | kiunga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kiunga
    Processing Record 340 | anloga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=anloga
    Processing Record 341 | verkhnyaya inta
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=verkhnyaya%20inta
    Processing Record 342 | rio blanco
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=rio%20blanco
    Processing Record 343 | vila franca do campo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=vila%20franca%20do%20campo
    Processing Record 344 | opuwo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=opuwo
    Processing Record 345 | tel aviv-yafo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=tel%20aviv-yafo
    Processing Record 346 | kropotkin
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kropotkin
    Processing Record 347 | neustrelitz
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=neustrelitz
    Processing Record 348 | bandarbeyla
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bandarbeyla
    Processing Record 349 | murdochville
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=murdochville
    Processing Record 350 | ballina
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ballina
    Processing Record 351 | cabo san lucas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=cabo%20san%20lucas
    Processing Record 352 | savyntsi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=savyntsi
    Processing Record 353 | orcopampa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=orcopampa
    Processing Record 354 | puerto baquerizo moreno
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=puerto%20baquerizo%20moreno
    Processing Record 355 | hvammstangi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=hvammstangi
    Processing Record 356 | palabuhanratu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=palabuhanratu
    Processing Record 357 | srednebelaya
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=srednebelaya
    Processing Record 358 | tual
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=tual
    Processing Record 359 | mayo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mayo
    Processing Record 360 | aquiraz
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=aquiraz
    Processing Record 361 | coihaique
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=coihaique
    Processing Record 362 | camana
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=camana
    Processing Record 363 | hasaki
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=hasaki
    Processing Record 364 | nizwa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nizwa
    Processing Record 365 | baghdad
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=baghdad
    Processing Record 366 | hofn
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=hofn
    Processing Record 367 | husavik
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=husavik
    Processing Record 368 | kuva
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kuva
    Processing Record 369 | la seyne-sur-mer
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=la%20seyne-sur-mer
    Processing Record 370 | ust-maya
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ust-maya
    Processing Record 371 | caravelas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=caravelas
    Processing Record 372 | azare
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=azare
    Processing Record 373 | sibolga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sibolga
    Processing Record 374 | jasdan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=jasdan
    Processing Record 375 | el alto
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=el%20alto
    Processing Record 376 | zhanaozen
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=zhanaozen
    Processing Record 377 | atambua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=atambua
    Processing Record 378 | mandali
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mandali
    Processing Record 379 | evanston
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=evanston
    Processing Record 380 | korla
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=korla
    Processing Record 381 | roma
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=roma
    Processing Record 382 | kavieng
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kavieng
    Processing Record 383 | hihifo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=hihifo
    Processing Record 384 | oistins
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=oistins
    Processing Record 385 | nizhnyaya poyma
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nizhnyaya%20poyma
    Processing Record 386 | kanniyakumari
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kanniyakumari
    Processing Record 387 | longjiang
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=longjiang
    Processing Record 388 | diamantino
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=diamantino
    Processing Record 389 | flin flon
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=flin%20flon
    Processing Record 390 | vila velha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=vila%20velha
    Processing Record 391 | akyab
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=akyab
    Processing Record 392 | maturin
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=maturin
    Processing Record 393 | mae sai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mae%20sai
    Processing Record 394 | tuni
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=tuni
    Processing Record 395 | subate
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=subate
    Processing Record 396 | hervey bay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=hervey%20bay
    Processing Record 397 | ponta do sol
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ponta%20do%20sol
    Processing Record 398 | de-kastri
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=de-kastri
    Processing Record 399 | nenjiang
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nenjiang
    Processing Record 400 | alta floresta
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=alta%20floresta
    Processing Record 401 | bilibino
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bilibino
    Processing Record 402 | sao joao da barra
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sao%20joao%20da%20barra
    Processing Record 403 | sondrio
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sondrio
    Processing Record 404 | ondorhaan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ondorhaan
    Processing Record 405 | general roca
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=general%20roca
    Processing Record 406 | nakur
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nakur
    Processing Record 407 | nahuala
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nahuala
    Processing Record 408 | ambilobe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ambilobe
    Processing Record 409 | benevento
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=benevento
    Processing Record 410 | santona
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=santona
    Processing Record 411 | wana
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=wana
    Processing Record 412 | warqla
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=warqla
    Processing Record 413 | dehloran
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=dehloran
    Processing Record 414 | sobolevo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sobolevo
    Processing Record 415 | dudinka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=dudinka
    Processing Record 416 | nushki
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nushki
    Processing Record 417 | richards bay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=richards%20bay
    Processing Record 418 | alofi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=alofi
    Processing Record 419 | san joaquin
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=san%20joaquin
    Processing Record 420 | kattivakkam
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kattivakkam
    Processing Record 421 | kommunisticheskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kommunisticheskiy
    Processing Record 422 | pangnirtung
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=pangnirtung
    Processing Record 423 | gainesville
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=gainesville
    Processing Record 424 | asau
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=asau
    Processing Record 425 | ascension
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ascension
    Processing Record 426 | eyl
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=eyl
    Processing Record 427 | cozumel
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=cozumel
    Processing Record 428 | savannah bight
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=savannah%20bight
    Processing Record 429 | baie-comeau
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=baie-comeau
    Processing Record 430 | madimba
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=madimba
    Processing Record 431 | abha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=abha
    Processing Record 432 | kholm-zhirkovskiy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kholm-zhirkovskiy
    Processing Record 433 | buraydah
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=buraydah
    Processing Record 434 | paralimni
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=paralimni
    Processing Record 435 | exokhi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=exokhi
    Processing Record 436 | mount isa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mount%20isa
    Processing Record 437 | axim
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=axim
    Processing Record 438 | ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ostrovnoy
    Processing Record 439 | tazmalt
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=tazmalt
    Processing Record 440 | kismayo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kismayo
    Processing Record 441 | broken hill
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=broken%20hill
    Processing Record 442 | camacha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=camacha
    Processing Record 443 | lusambo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=lusambo
    Processing Record 444 | kendari
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kendari
    Processing Record 445 | simao
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=simao
    Processing Record 446 | esperance
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=esperance
    Processing Record 447 | katherine
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=katherine
    Processing Record 448 | aurdal
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=aurdal
    Processing Record 449 | guelengdeng
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=guelengdeng
    Processing Record 450 | morehead
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=morehead
    Processing Record 451 | hirtshals
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=hirtshals
    Processing Record 452 | acarau
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=acarau
    Processing Record 453 | ocho rios
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ocho%20rios
    Processing Record 454 | laguna
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=laguna
    Processing Record 455 | cankuzo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=cankuzo
    Processing Record 456 | hami
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=hami
    Processing Record 457 | solaro
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=solaro
    Processing Record 458 | bontang
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bontang
    Processing Record 459 | panorama
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=panorama
    Processing Record 460 | chitungwiza
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=chitungwiza
    Processing Record 461 | sorland
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sorland
    Processing Record 462 | west odessa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=west%20odessa
    Processing Record 463 | kieta
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kieta
    Processing Record 464 | kupang
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kupang
    Processing Record 465 | kenai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kenai
    Processing Record 466 | balikpapan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=balikpapan
    Processing Record 467 | corsicana
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=corsicana
    Processing Record 468 | ternate
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ternate
    Processing Record 469 | pontianak
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=pontianak
    Processing Record 470 | kamaishi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kamaishi
    Processing Record 471 | kanungu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kanungu
    Processing Record 472 | great falls
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=great%20falls
    Processing Record 473 | san nicolas
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=san%20nicolas
    Processing Record 474 | gorontalo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=gorontalo
    Processing Record 475 | egvekinot
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=egvekinot
    Processing Record 476 | stanislav
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=stanislav
    Processing Record 477 | san jeronimo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=san%20jeronimo
    Processing Record 478 | lata
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=lata
    Processing Record 479 | northam
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=northam
    Processing Record 480 | uvinza
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=uvinza
    Processing Record 481 | karamea
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=karamea
    Processing Record 482 | kota kinabalu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kota%20kinabalu
    Processing Record 483 | tiarei
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=tiarei
    Processing Record 484 | hailar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=hailar
    Processing Record 485 | inhambane
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=inhambane
    Processing Record 486 | kindu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kindu
    Processing Record 487 | hambantota
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=hambantota
    Processing Record 488 | inuvik
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=inuvik
    Processing Record 489 | omboue
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=omboue
    Processing Record 490 | westport
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=westport
    Processing Record 491 | smithers
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=smithers
    Processing Record 492 | plouzane
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=plouzane
    Processing Record 493 | tynda
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=tynda
    Processing Record 494 | pacific grove
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=pacific%20grove
    Processing Record 495 | karratha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=karratha
    Processing Record 496 | marzuq
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=marzuq
    Processing Record 497 | timra
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=timra
    Processing Record 498 | north bend
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=north%20bend
    Processing Record 499 | manavgat
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=manavgat
    Processing Record 500 | krasnoyarsk-66
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=krasnoyarsk-66
    Processing Record 501 | springdale
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=springdale
    Processing Record 502 | redcliffe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=redcliffe
    Processing Record 503 | anjozorobe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=anjozorobe
    Processing Record 504 | half moon bay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=half%20moon%20bay
    Processing Record 505 | tuatapere
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=tuatapere
    Processing Record 506 | henties bay
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=henties%20bay
    Processing Record 507 | kibaha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kibaha
    Processing Record 508 | kerzhenets
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kerzhenets
    Processing Record 509 | saleaula
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=saleaula
    Processing Record 510 | margate
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=margate
    Processing Record 511 | pangai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=pangai
    Processing Record 512 | kousseri
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kousseri
    Processing Record 513 | chengde
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=chengde
    Processing Record 514 | shikarpur
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=shikarpur
    Processing Record 515 | mayna
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mayna
    Processing Record 516 | inirida
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=inirida
    Processing Record 517 | azle
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=azle
    Processing Record 518 | praia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=praia
    Processing Record 519 | miraflores
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=miraflores
    Processing Record 520 | taoudenni
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=taoudenni
    Processing Record 521 | port hardy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=port%20hardy
    Processing Record 522 | piacabucu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=piacabucu
    Processing Record 523 | bilma
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bilma
    Processing Record 524 | pulawy
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=pulawy
    Processing Record 525 | aksarka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=aksarka
    Processing Record 526 | kudahuvadhoo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kudahuvadhoo
    Processing Record 527 | chabahar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=chabahar
    Processing Record 528 | harwich
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=harwich
    Processing Record 529 | bajo baudo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bajo%20baudo
    Processing Record 530 | moroni
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=moroni
    Processing Record 531 | zhuhai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=zhuhai
    Processing Record 532 | shar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=shar
    Processing Record 533 | bara
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bara
    Processing Record 534 | lake havasu city
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=lake%20havasu%20city
    Processing Record 535 | akdepe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=akdepe
    Processing Record 536 | poshekhonye
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=poshekhonye
    Processing Record 537 | bagotville
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bagotville
    Processing Record 538 | huntingdon
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=huntingdon
    Processing Record 539 | yarmouth
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=yarmouth
    Processing Record 540 | marrakesh
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=marrakesh
    Processing Record 541 | salme
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=salme
    Processing Record 542 | sortland
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=sortland
    Processing Record 543 | tura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=tura
    Processing Record 544 | eslov
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=eslov
    Processing Record 545 | correntina
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=correntina
    Processing Record 546 | quatre cocos
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=quatre%20cocos
    Processing Record 547 | brindisi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=brindisi
    Processing Record 548 | maceio
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=maceio
    Processing Record 549 | henderson
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=henderson
    Processing Record 550 | aklavik
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=aklavik
    Processing Record 551 | oranzherei
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=oranzherei
    Processing Record 552 | reinheim
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=reinheim
    Processing Record 553 | allanmyo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=allanmyo
    Processing Record 554 | mendahara
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mendahara
    Processing Record 555 | seymchan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=seymchan
    Processing Record 556 | zhigansk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=zhigansk
    Processing Record 557 | tomatlan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=tomatlan
    Processing Record 558 | skagastrond
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=skagastrond
    Processing Record 559 | navalcarnero
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=navalcarnero
    Processing Record 560 | quelimane
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=quelimane
    Processing Record 561 | kalomo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kalomo
    Processing Record 562 | bathsheba
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bathsheba
    Processing Record 563 | dingle
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=dingle
    Processing Record 564 | akureyri
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=akureyri
    Processing Record 565 | chak azam sahu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=chak%20azam%20sahu
    Processing Record 566 | greenville
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=greenville
    Processing Record 567 | ambon
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ambon
    Processing Record 568 | ondjiva
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ondjiva
    Processing Record 569 | lodja
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=lodja
    Processing Record 570 | nakamura
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nakamura
    Processing Record 571 | rio bueno
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=rio%20bueno
    Processing Record 572 | fruitvale
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=fruitvale
    Processing Record 573 | bacolod
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bacolod
    Processing Record 574 | kamenskoye
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kamenskoye
    Processing Record 575 | vao
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=vao
    Processing Record 576 | upata
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=upata
    Processing Record 577 | ipua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ipua
    Processing Record 578 | puerto colombia
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=puerto%20colombia
    Processing Record 579 | kricim
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kricim
    Processing Record 580 | namatanai
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=namatanai
    Processing Record 581 | vidim
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=vidim
    Processing Record 582 | strelka
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=strelka
    Processing Record 583 | houston
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=houston
    Processing Record 584 | roald
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=roald
    Processing Record 585 | hirara
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=hirara
    Processing Record 586 | elko
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=elko
    Processing Record 587 | victor harbor
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=victor%20harbor
    Processing Record 588 | mangan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mangan
    Processing Record 589 | kandalaksha
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=kandalaksha
    Processing Record 590 | puerto ayacucho
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=puerto%20ayacucho
    Processing Record 591 | ondangwa
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ondangwa
    Processing Record 592 | aykhal
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=aykhal
    Processing Record 593 | barra do corda
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=barra%20do%20corda
    Processing Record 594 | poya
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=poya
    Processing Record 595 | flinders
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=flinders
    Processing Record 596 | mayumba
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=mayumba
    Processing Record 597 | pitimbu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=pitimbu
    Processing Record 598 | lolua
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=lolua
    Processing Record 599 | tsihombe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=tsihombe
    Processing Record 600 | klaksvik
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=klaksvik
    Processing Record 601 | bolshoy tsaryn
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=bolshoy%20tsaryn
    Processing Record 602 | douentza
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=douentza
    Processing Record 603 | naze
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=naze
    Processing Record 604 | copiapo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=copiapo
    Processing Record 605 | awjilah
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=awjilah
    Processing Record 606 | vallenar
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=vallenar
    Processing Record 607 | ambulu
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ambulu
    Processing Record 608 | yabelo
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=yabelo
    Processing Record 609 | ghanzi
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=ghanzi
    Processing Record 610 | jacareacanga
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=jacareacanga
    Processing Record 611 | gornozavodsk
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=gornozavodsk
    Processing Record 612 | nanortalik
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=nanortalik
    Processing Record 613 | great bend
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=great%20bend
    Processing Record 614 | chumikan
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=chumikan
    Processing Record 615 | port-gentil
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=port-gentil
    Processing Record 616 | santa fe
    http://api.openweathermap.org/data/2.5/weather?units=imperial&APPID=7b3dce35aaa126aed882dbff80b8c4dc&q=santa%20fe
    ------------------------------
    Data Retrieval Complete
    ------------------------------



```python
# Create a DataFrame with the information received 
cities_df = pd.DataFrame(cities_data)
cities_df.count()
```




    City          616
    Cloudiness    616
    Country       616
    Date          616
    Humidity      616
    Lat           616
    Lng           616
    Max Temp      616
    Wind Speed    616
    dtype: int64




```python
# Display the city dataframe
cities_df.head()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>Cloudiness</th>
      <th>Country</th>
      <th>Date</th>
      <th>Humidity</th>
      <th>Lat</th>
      <th>Lng</th>
      <th>Max Temp</th>
      <th>Wind Speed</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>farsund</td>
      <td>0</td>
      <td>NO</td>
      <td>1519941794</td>
      <td>100</td>
      <td>58.09</td>
      <td>6.80</td>
      <td>19.41</td>
      <td>19.19</td>
    </tr>
    <tr>
      <th>1</th>
      <td>upernavik</td>
      <td>88</td>
      <td>GL</td>
      <td>1519941854</td>
      <td>87</td>
      <td>72.79</td>
      <td>-56.15</td>
      <td>23.14</td>
      <td>9.13</td>
    </tr>
    <tr>
      <th>2</th>
      <td>kahului</td>
      <td>40</td>
      <td>US</td>
      <td>1519937760</td>
      <td>61</td>
      <td>20.89</td>
      <td>-156.47</td>
      <td>80.60</td>
      <td>14.99</td>
    </tr>
    <tr>
      <th>3</th>
      <td>amderma</td>
      <td>40</td>
      <td>US</td>
      <td>1519937760</td>
      <td>61</td>
      <td>20.89</td>
      <td>-156.47</td>
      <td>80.60</td>
      <td>14.99</td>
    </tr>
    <tr>
      <th>4</th>
      <td>okha</td>
      <td>64</td>
      <td>RU</td>
      <td>1519941855</td>
      <td>80</td>
      <td>53.59</td>
      <td>142.95</td>
      <td>3.97</td>
      <td>13.04</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Save the DataFrame as a csv
cities_df.to_csv("Output/weatherpy_data.csv", encoding="utf-8", index=False)
```

### Latitude vs. Temperature Plot


```python
# Scatter plot City Latitude vs. Temperature
sns.set_style("darkgrid")
plt.scatter(cities_df["Lat"],cities_df["Max Temp"],color="blue", s=20, marker="o", 
            alpha=0.75, linewidths=1, edgecolor='black')
plt.title("City Latitude vs. Max Temperature (03/01/2018)")
plt.xlabel("Latitude")
plt.ylabel("Max Temperature (F)")

# Save the figure
plt.savefig("Output/lat_temperature.png")

# Show plot
plt.show()
```


![png](output_11_0.png)


### Latitude vs. Humidity Plot


```python
# Scatter plot City Latitude vs. Humidity
plt.scatter(cities_df["Lat"],cities_df["Humidity"],color="blue", s=20, marker="o", 
            alpha=0.75, linewidths=1, edgecolor='black')
plt.title("City Latitude vs. Humidity (03/01/2018)")
plt.xlabel("Latitude")
plt.ylabel("Humidity (%)")

plt.xlim([-80, 100])
plt.ylim([-20,120])

# Save the figure
plt.savefig("Output/lat_humidity.png")

# Show plot
plt.show()
```


![png](output_13_0.png)


### Latitude vs. Cloudiness Plot


```python
# Scatter plot City Latitude vs. Cloudiness
plt.scatter(cities_df["Lat"],cities_df["Cloudiness"],color="blue", s=20, marker="o", 
            alpha=0.75, linewidths=1, edgecolor='black')
plt.title("City Latitude vs. Cloudiness (03/01/2018)")
plt.xlabel("Latitude")
plt.ylabel("Cloudiness (%)")

plt.xlim([-80, 100])
plt.ylim([-20,120])

# Save the figure
plt.savefig("Output/lat_cloud.png")

# Show plot
plt.show()
```


![png](output_15_0.png)


### Latitude vs. Wind Speed Plot


```python
# Scatter plot City Latitude vs. Wind Speed
plt.scatter(cities_df["Lat"],cities_df["Wind Speed"],color="blue", s=20, marker="o", 
            alpha=0.75, linewidths=1, edgecolor='black')
plt.title("City Latitude vs. Wind Speed (03/01/2018)")
plt.xlabel("Latitude")
plt.ylabel("Wind Speed (mph)")

plt.xlim([-80, 100])
plt.ylim([-5,40])

# Save the figure
plt.savefig("Output/lat_windspeed.png")

# Show plot
plt.show()
```


![png](output_17_0.png)

