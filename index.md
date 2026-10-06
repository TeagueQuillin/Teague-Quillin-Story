# Teague Quillin

Masters student of Business Intelligence and Process Management

Berlin School of Economics and Law

https://www.linkedin.com/in/teaguequillin/

## Where I'm from

Born and raised in San Ramon, California, USA (45min outside San Francisco)

Lived in San Diego, California from 2016 to 2026

<img src="sf-photo.png" alt="sf-photo.png" width="40%"> <img src="sd-photo.png" alt="sd-photo.png" width="40%">

## My interests

Nature, Basketball, Comedy, Travel, Drawing weird pictures of my friend's dogs

<img src="nature-photo2.jpeg" alt="nature-photo2.jpeg" width="40%"> <img src="bball-photo1.JPG" alt="bball-photo1.JPG" width="40%">
<img src="comedy-photo1.jpeg" alt="comedy-photo1.jpeg" width="40%"> <img src="travel-photo2.jpg" alt="travel-photo2.jpg" width="40%">
<img src="drawing1-photo2.jpeg" alt="drawing1-photo2.jpeg" width="30%"> <img src="drawing2-photo2.jpeg" alt="drawing2-photo2.jpg" width="30%"> <img src="drawing3-photo1.jpeg" alt="drawing3-photo1.jpg" width="30%">

## Academic and Career Journey
| Period | Role | Organization |
|--------|------|--------------|
| 2016-2020 | Bachelors Accounting Student | San Diego State University |
| 2018-2021 | Part-time Accounting Assistant | Aztec Shops llc |
| 2020-2022 | Completed Certified Public Accountant (CPA) Exams | State of California |
| 2021-2023 | Senior Assurance Auditor | EY |
| 2024-2026 | Senior Accountant of SEC Reporting and Equity | NeoGenomics Laboratories |
| 2026-Present | BIPM Masters student | HWR Berlin |

import folium
from pyproj import Geod

geod = Geod(ellps="WGS84")

# Define stops
stops = [
    {"city": "San Ramon, CA", "lat": 37.7800, "lon": -121.9781, "popup": "Childhood"},
    {"city": "San Diego, CA", "lat": 32.7157, "lon": -117.1611, "popup": "Bachelors San Diego State University"},
    {"city": "Berlin, Germany", "lat": 52.5200, "lon": 13.4050, "popup": "Masters HWR Berlin"}
]

# Create map
m = folium.Map(
    location=[40, 0],
    zoom_start=2,
    #tiles="https://{s}.basemaps.cartocdn.com/light_all/{z}/{x}/{y}{r}.png",
    #tiles="OpenStreetMap",
    #attr="© OpenStreetMap contributors © CARTO"
    tiles="https://server.arcgisonline.com/ArcGIS/rest/services/Canvas/World_Light_Gray_Base/MapServer/tile/{z}/{y}/{x}",
    attr="Tiles &copy; Esri --- Esri, HERE, Garmin, and contributors"
)

# Add markers
for stop in stops:
    folium.Marker(
        [stop["lat"], stop["lon"]],
        popup=stop["popup"],
        tooltip=stop["city"],
        icon=folium.Icon(color="blue", icon="info-sign")
    ).add_to(m)

# Function to compute many intermediate great-circle points
def great_circle_points(lat1, lon1, lat2, lon2, npts=100):
    points = geod.npts(lon1, lat1, lon2, lat2, npts)
    coords = [(lat1, lon1)] + [(lat, lon) for lon, lat in points] + [(lat2, lon2)]
    return coords

# Draw arcs between consecutive stops
for i in range(len(stops) - 1):
    start, end = stops[i], stops[i + 1]
    arc = great_circle_points(start["lat"], start["lon"], end["lat"], end["lon"], npts=60)
    folium.PolyLine(
        arc,
        color="darkblue",
        weight=3,
        opacity=0.7
    ).add_to(m)

# Save and open
#m.save("academic_journey_map.html")
#import webbrowser; webbrowser.open("academic_journey_map.html")
m
