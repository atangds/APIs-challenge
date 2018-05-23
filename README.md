
# WeatherPy Analysis

* In genreal, temperature rises as we approach the equator. Although we do see some even higher temperatures around 10-30N.

* Humidity is high around the euquator, and it reaches a low around 20N.

* Latitude does not seem to be a huge factor when it comes to cloudiness and wind speed. We can see varying levels of cloudiness and wind speed across all latitudes.


```python
import numpy as np
from citipy import citipy
from config import api_key
import openweathermapy.core as owm
import pandas as pd
from datetime import datetime
import matplotlib.pyplot as plt
```

## Generate Cities List


```python
# Randomly select 2000 pairs of latitude/longitude.
np.random.seed(1)
lats = np.random.uniform(-90,90,[2000])
lons = np.random.uniform(-180,180,[2000])
```


```python
# Get a list of nearest cities.
cities_ls = [citipy.nearest_city(i,j).city_name for i,j in zip(lats, lons)]
len(cities_ls)
```




    2000




```python
# Drop duplicates.
cities = set(cities_ls)
len(cities)
```




    773



## Perform API Calls


```python
settings = {"appid": api_key, "units": "imperial"}
responses = []
n = 0
for city in cities:
    try:
        responses.append(owm.get_current(city, **settings))
    except:
        n += 1
print(f"No results for {n} cities.")
```

    No results for 81 cities.
    


```python
summary = ["name", "sys.country", "coord.lat", "dt", "main.temp", "main.humidity", "clouds.all", "wind.speed"]
data = [responses[_](*summary) for _ in range(len(responses))]
```


```python
for _ in range(len(data)):
    print(f"http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q={data[_][0].lower().replace(' ', '+')}")
# See Bootcampspot for "appid".
```

    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mount+gambier
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nikolayevsk-na-amure
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ayorou
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mae+sai
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=naliya
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=haines+junction
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=charlestown
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=touros
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=san+quintin
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=namwala
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=muncar
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=khandbari
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tiarei
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=carnarvon
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nhulunbuy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bubaque
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=jizan
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tazovskiy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=havelock
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=hambantota
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=karlskrona
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mahina
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=salisbury
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=edinburg
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=listvyanka
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=san+patricio
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tiksi
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=provost
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=srednekolymsk
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=alta+floresta
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=baiyin
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=butaritari
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=torbay
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ponta+pora
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ushuaia
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=praxedis+guerrero
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kontagora
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=port+hedland
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ambilobe
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=shelburne
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=saint-francois
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ucluelet
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=shimoda
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=upata
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=yeppoon
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lasem
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=pekan
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=manokwari
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=barcelona
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=keti+bandar
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=simao
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nizwa
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=shajapur
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=muscat
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kabare
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=biak
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tel+aviv-yafo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=vaini
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=rapu-rapu
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=acari
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=deputatskiy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=boddam
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=orlik
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lamont
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=arlit
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=qaanaaq
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=martapura
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=moissala
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lavrentiya
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=luangwa
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=imbituba
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=adeje
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tahe
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=preobrazheniye
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ratnagiri
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=totness
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ilulissat
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=vao
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=vestmannaeyjar
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=pevek
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=acajutla
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bandar-e+lengeh
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=novyy+urengoy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bredasdorp
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=moa
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=muskegon
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=berezivka
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=rio+grande
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bay+roberts
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kathu
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nautla
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=yarada
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=karratha
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=grand+gaube
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=auch
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lodja
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=auki
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=rawson
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=porto+walter
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=baykit
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=noshiro
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kargopol
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=pechora
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=blenheim
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=leningradskiy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=igarka
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mount+isa
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=gravdal
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=inirida
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=clyde+river
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lerwick
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=gawler
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=shepetivka
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=shingu
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=beloha
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=robertsport
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=seymchan
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nome
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=taoudenni
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=cayenne
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=valparaiso
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=saint-augustin
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=hanmer+springs
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=karpogory
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=saskylakh
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=makakilo+city
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lexington+park
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=yumen
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=saint-pierre
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=xuddur
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kingaroy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=port+blair
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=navolato
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=san+cristobal
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=sawakin
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=san+vicente
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=saint+george
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=morant+bay
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=teacapan
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=buala
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tornio
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lobez
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=chernyshevskiy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=yuci
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kumluca
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=dingle
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kungurtug
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=zelenets
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ilam
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=beian
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=thompson
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nabire
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=hinton
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=airai
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ambovombe
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=zafra
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=oussouye
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=constancia
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=avera
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=beboto
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=puerto+ayora
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mendi
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=hatillo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=oro+valley
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=matara
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=catuday
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=huzhou
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nalut
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=chicama
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=fare
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=marzuq
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=port+alfred
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=pauini
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bathsheba
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=gamba
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tamiahua
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=astana
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=itarema
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=marawi
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=gat
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lata
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=poum
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=awbari
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kavaratti
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=westport
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=sola
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=amuntai
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=norman+wells
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=darnah
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=salinas
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=east+london
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=talnakh
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=vavoua
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=rikitea
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kruisfontein
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=heihe
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=luderitz
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=schrems
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kigoma
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=harper
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=petropavlovsk-kamchatskiy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=marsa+matruh
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bindura
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=muzhi
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=imeni+poliny+osipenko
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=hofn
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=placerville
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=goldap
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=gornopravdinsk
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lima
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=chifeng
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ayagoz
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=pindiga
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=rexburg
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=iqaluit
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=pehowa
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=cayeli
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=waingapu
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kikwit
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=eyl
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mogocha
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=jinka
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tuatapere
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=portland
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=chanute
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mangrol
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=san+lorenzo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=miragoane
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=price
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bad+windsheim
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=vikhorevka
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=puerto+berrio
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ust-donetskiy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=pacific+grove
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=hamilton
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nikolskoye
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nongstoin
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=longyearbyen
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kieta
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=rameswaram
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ishigaki
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mukhen
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tuktoyaktuk
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lasa
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=hervey+bay
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tevaitoa
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=port+hardy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ambulu
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kotido
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=castrillon
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mar+del+plata
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ambon
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=villa+carlos+paz
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=naantali
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kapaa
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=aksu
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=adrar
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=savelugu
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=houma
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tlaxco
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=dalbandin
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ushtobe
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bhalki
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=qitaihe
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bambous+virieux
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=carora
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=goderich
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=krasnyy+yar
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=sept-iles
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=alibunan
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=yellowknife
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=la+asuncion
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=pontivy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ekhabi
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=provideniya
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=banjar
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=khandyga
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=cidreira
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=salamanca
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=hailar
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kutum
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kiyasovo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=sotik
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=new+norfolk
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=barrow
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=qaqortoq
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=waddan
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=turbat
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=pisco
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=apango
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nouadhibou
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=skjervoy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=barahona
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=richards+bay
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=evensk
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=wismar
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=faanui
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=casas+grandes
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kyzyl
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=avarua
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=vostok
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mehamn
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=hilo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=panji
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=saint+anthony
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=puro
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=saint-georges
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=okha
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=zonguldak
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=moron
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=somondoco
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ballina
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kiruna
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=pyaozerskiy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mao
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kushiro
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=sinnamary
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kielce
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=floro
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=naze
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=guiratinga
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=necochea
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=north+bend
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=santiago+del+estero
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=rio+gallegos
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=conceicao+do+araguaia
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=sirur
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bull+savanna
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=vila+velha
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=busselton
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=prince+albert
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=alvaraes
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=vardo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=stykkisholmur
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=monte+patria
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=sabang
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=khorramshahr
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ngunguru
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kulhudhuffushi
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=corowa
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=jaisalmer
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=usinsk
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nanakuli
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=seoul
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kloulklubed
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=penzance
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=cape+town
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=geraldton
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bandraboua
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=espanola
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=davila
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=yaan
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kavieng
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=gao
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=souris
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=urusha
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=atar
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=peniche
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=esplanada
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=yining
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=jamkhed
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=zaraza
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ondjiva
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bilibino
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=daru
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bauchi
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=powell+river
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=batemans+bay
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=puerto+escondido
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bria
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=inhambane
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=fethiye
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nishihara
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=souillac
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=punta+arenas
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=wattegama
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tocopilla
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tebingtinggi
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bozeman
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=thunder+bay
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=skegness
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=albany
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lebu
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=conde
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=brae
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=baker+city
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=iquitos
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=saldanha
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=egvekinot
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=malanje
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=may+pen
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=puerto+narino
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=indiaroba
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=zabol
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=yulara
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ovsyanka
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=cherskiy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=gushikawa
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=caravelas
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tessalit
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lampazos+de+naranjo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lifford
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bondo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=constitucion
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nichinan
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=rocha
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lake+havasu+city
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=hailey
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=little+current
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ust-maya
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=west+fargo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=port+lincoln
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=aklavik
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=chokurdakh
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=batiano
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=komsomolskiy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=hithadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ancud
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=alofi
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lexington
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=timbiras
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=castro
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=chibombo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=butte
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bell+ville
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=yanam
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=antofagasta
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mocuba
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=gamboma
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=smithers
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=sungaipenuh
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=cabo+san+lucas
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=meulaboh
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=naranjito
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=chuy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=dubai
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=linjiang
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=jalu
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lahar
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bahadurgarh
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=adre
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tura
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=birobidzhan
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kushima
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mayo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=centralina
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=georgetown
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=college
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lagoa
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=zhigansk
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=havre-saint-pierre
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=sakaiminato
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=leme
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=salalah
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=orotukan
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lixourion
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=chapais
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=miandrivazo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=motygino
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=novobirilyussy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tucuman
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=arraial+do+cabo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=eureka
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=shaoguan
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=seminole
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=hualmay
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nuevo+progreso
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=iquique
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=yar-sale
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=grindavik
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=fairbanks
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=coroata
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=banjarmasin
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=hilton+head+island
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=russell
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=waipawa
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tautira
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=pangai
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tigil
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=guerrero+negro
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=araouane
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=iskateley
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=batouri
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=la+rioja
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=floresti
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=jorochito
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=meadville
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lufilufi
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=denpasar
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=chulym
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=klaksvik
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=hobyo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=la+ronge
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=beringovskiy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=wanning
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=espinosa
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kargasok
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=khatanga
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=vladyslavivka
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kudahuvadhoo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=samarai
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=quesada
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=staryy+nadym
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kangar
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kodiak
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tamale
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=texarkana
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=quatre+cocos
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=pangody
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=cravo+norte
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=roald
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ulaangom
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kaitangata
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ponta+delgada
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=atuona
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=biankouma
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=harpanahalli
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=soyo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ankang
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=jadu
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ridgecrest
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=teya
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=albemarle
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=luau
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=trairi
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=chimbote
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=hermanus
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=camacha
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mujiayingzi
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=gizo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=leshukonskoye
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=cap+malheureux
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=demidov
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=praia
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=knysna
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=sambava
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=fort+nelson
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bonoua
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=broome
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=yelizavetino
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=san+juan
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=almenara
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=margate
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kihei
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=riyadh
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=coahuayana
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=palana
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tilichiki
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=keuruu
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=port+shepstone
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tadine
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=los+llanos+de+aridane
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=aginskoye
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=goroka
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=dukat
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=yerofey+pavlovich
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=homer
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=caxito
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=monroe
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kenai
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=gaya
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=villamontes
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=esperance
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=acapulco
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=maldonado
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mecca
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tiznit
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=biltine
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=helena
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=pasir+gudang
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=state+college
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=besao
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=svetlaya
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=coquimbo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=belgrade
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kangaatsiaq
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=hasaki
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kendari
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=palimbang
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=muros
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=priiskovyy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=palmeira+das+missoes
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=milkovo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kahului
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=leh
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=coihaique
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=huangchuan
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=opuwo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=maniitsoq
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=charters+towers
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=west+wendover
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=dunedin
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=muhos
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=el+tigre
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=aykhal
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tateyama
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=port+elizabeth
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=dikson
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=roebourne
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=saint-philippe
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=flin+flon
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nurota
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=montepuez
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=camocim
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=teknaf
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=victoria
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=pringsewu
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=trogir
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bluff
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=yuanping
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=chumikan
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=severo-kurilsk
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=koupela
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=hobart
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=gatesville
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ahipara
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lorengau
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=macklin
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kirkland+lake
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tasiilaq
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bethel
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=gasa
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=narsaq
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=prado
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=moose+factory
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=kresttsy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=japura
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ostersund
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=aleppo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=namibe
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bereslavka
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ponta+do+sol
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=chama
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mayumba
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=otradnoye
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=severo-yeniseyskiy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=marystown
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mahebourg
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=merrill
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=antalaha
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=buin
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=watertown
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=rabat
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=dong+xoai
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=alyangula
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=codrington
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=isangel
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=stavrovo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=asyut
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=novopokrovka
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tarauaca
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=mataura
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=dabakala
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=farafangana
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=talaya
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=live+oak
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=greiz
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=olpad
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nuuk
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=maymyo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=brownsville
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=puerto+carreno
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nanortalik
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=thayetmyo
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=lillooet
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=santa+isabel+do+rio+negro
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ust-koksa
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=upernavik
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=gamboula
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=salamiyah
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=port+augusta
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=morlaix
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=paungde
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=tual
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=fort+madison
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=aktau
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=cabinda
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=bhera
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=wajima
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ribeira+grande
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=katsuura
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=nizhniy+ingash
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=turukhansk
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=maragogi
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ostrovnoy
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=francistown
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=miri
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=ugoofaaru
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=fortuna
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=sao+joao+da+barra
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=jamestown
    http://api.openweathermap.org/data/2.5/weather?appid=***&units=imperial&q=port-gentil
    


```python
df = pd.DataFrame(data, columns=["City", "Country", "Latitude", "Time", 
                                 "Temperature (F)", "Humidity (%)", "Cloudiness (%)", "Wind Speed (mph)"])
for _ in df.index.values:
    df.loc[_, "Time"] = datetime.fromtimestamp(df.loc[_, "Time"]).strftime('%Y-%m-%d %H:%M:%S')
df.to_csv("weather_data.csv", index=False)
df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>City</th>
      <th>Country</th>
      <th>Latitude</th>
      <th>Time</th>
      <th>Temperature (F)</th>
      <th>Humidity (%)</th>
      <th>Cloudiness (%)</th>
      <th>Wind Speed (mph)</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Mount Gambier</td>
      <td>AU</td>
      <td>-37.83</td>
      <td>2018-05-23 13:07:11</td>
      <td>51.03</td>
      <td>100</td>
      <td>80</td>
      <td>2.75</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Nikolayevsk-na-amure</td>
      <td>RU</td>
      <td>53.14</td>
      <td>2018-05-23 13:07:11</td>
      <td>40.14</td>
      <td>67</td>
      <td>76</td>
      <td>11.25</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Ayorou</td>
      <td>NE</td>
      <td>14.73</td>
      <td>2018-05-23 13:07:11</td>
      <td>104.04</td>
      <td>18</td>
      <td>8</td>
      <td>6.55</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Mae Sai</td>
      <td>MM</td>
      <td>20.44</td>
      <td>2018-05-23 12:00:00</td>
      <td>77.00</td>
      <td>94</td>
      <td>0</td>
      <td>1.63</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Naliya</td>
      <td>IN</td>
      <td>23.26</td>
      <td>2018-05-23 13:07:12</td>
      <td>80.01</td>
      <td>79</td>
      <td>0</td>
      <td>7.78</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.info()
# 692 = 773 - 81
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 692 entries, 0 to 691
    Data columns (total 8 columns):
    City                692 non-null object
    Country             692 non-null object
    Latitude            692 non-null float64
    Time                692 non-null object
    Temperature (F)     692 non-null float64
    Humidity (%)        692 non-null int64
    Cloudiness (%)      692 non-null int64
    Wind Speed (mph)    692 non-null float64
    dtypes: float64(3), int64(2), object(3)
    memory usage: 43.3+ KB
    

## City Latitude vs. Weather Plots


```python
plt.style.use("seaborn-darkgrid")
plt.figure(figsize=(10,7.5))
plt.scatter(df["Latitude"], df["Temperature (F)"], s=50, c="b", lw=1, edgecolors="k")
plt.title("City Latitude vs. Temperature (5/23/2018)", size=15)
plt.xlabel("Latitude", size=15)
plt.ylabel("Temperature (F)", size=15)
plt.savefig("plot1")
plt.show()
```


![png](output_13_0.png)



```python
plt.figure(figsize=(10,7.5))
plt.scatter(df["Latitude"], df["Humidity (%)"], s=50, c="b", lw=1, edgecolors="k")
plt.title("City Latitude vs. Humidity (5/23/2018)", size=15)
plt.xlabel("Latitude", size=15)
plt.ylabel("Humidity (%)", size=15)
plt.savefig("plot2")
plt.show()
```


![png](output_14_0.png)



```python
plt.figure(figsize=(10,7.5))
plt.scatter(df["Latitude"], df["Cloudiness (%)"], s=50, c="b", lw=1, edgecolors="k")
plt.title("City Latitude vs. Cloudiness (5/23/2018)", size=15)
plt.xlabel("Latitude", size=15)
plt.ylabel("Cloudiness (%)", size=15)
plt.savefig("plot3")
plt.show()
```


![png](output_15_0.png)



```python
plt.figure(figsize=(10,7.5))
plt.scatter(df["Latitude"], df["Wind Speed (mph)"], s=50, c="b", lw=1, edgecolors="k")
plt.title("City Latitude vs. Wind Speed (5/23/2018)", size=15)
plt.xlabel("Latitude", size=15)
plt.ylabel("Wind Speed (mph)", size=15)
plt.savefig("plot4")
plt.show()
```


![png](output_16_0.png)

