# LEO Satellite Handover Model (Pakistan)

This document demonstrates a **graph-theory-based handover** computation for Low Earth Orbit (LEO) satellites relative to ground stations in Pakistan.

[Technical Documentation of Handingover API](https://documenter.getpostman.com/view/46925086/2sB3BHjoZo#3f9cfe05-8410-4826-85b0-725d363c07f5)

## 1. Ground Stations and Satellites

**Satellites:**
```json
[
    {"id": "sat1", "latitude": 33.5, "longitude": 71.5, "altitude": 550},
    {"id": "sat2", "latitude": 32.0, "longitude": 72.0, "altitude": 560},
    {"id": "sat3", "latitude": 34.0, "longitude": 73.5, "altitude": 540}
]
```

**Ground Stations:**
```json
[
    {"id": "gs1", "latitude": 33.6844, "longitude": 73.0479},  # Islamabad
    {"id": "gs2", "latitude": 31.5497, "longitude": 74.3436},  # Lahore
    {"id": "gs3", "latitude": 24.8607, "longitude": 67.0011}   # Karachi
]
```

---

## 2. Haversine Function (Distance Calculation)
```python
import math

def haversine(lat1, lon1, lat2, lon2):
    R = 6371  # Earth radius in km
    phi1 = math.radians(lat1)
    phi2 = math.radians(lat2)
    dphi = math.radians(lat2 - lat1)
    dlambda = math.radians(lon2 - lon1)
    a = math.sin(dphi/2)**2 + math.cos(phi1)*math.cos(phi2)*math.sin(dlambda/2)**2
    c = 2 * math.atan2(math.sqrt(a), math.sqrt(1-a))
    return R * c
```

---

## 3. Graph Construction

Each satellite can connect to ground stations within **1500 km**. Distances are computed using the Haversine formula.

```python
satellites = [
    {"id": "sat1", "latitude": 33.5, "longitude": 71.5},
    {"id": "sat2", "latitude": 32.0, "longitude": 72.0},
    {"id": "sat3", "latitude": 34.0, "longitude": 73.5}
]

ground_stations = [
    {"id": "gs1", "latitude": 33.6844, "longitude": 73.0479},  # Islamabad
    {"id": "gs2", "latitude": 31.5497, "longitude": 74.3436},  # Lahore
    {"id": "gs3", "latitude": 24.8607, "longitude": 67.0011}   # Karachi
]

graph = {}
for sat in satellites:
    graph[sat["id"]] = {}
    for gs in ground_stations:
        distance = haversine(sat["latitude"], sat["longitude"], gs["latitude"], gs["longitude"])
        if distance < 1500:
            graph[sat["id"]][gs["id"]] = distance

print(graph)
```

---

## 4. Handover Computation

For each satellite, select the **nearest ground station** within range.

```python
handover = {}
for sat_id, links in graph.items():
    if links:
        nearest_gs = min(links, key=links.get)
        handover[sat_id] = nearest_gs

print(handover)
```

**Example Output:**
```json
{
  "sat1": "gs1",
  "sat2": "gs2",
  "sat3": "gs1"
}
```

- `sat1` and `sat3` are closest to Islamabad  
- `sat2` is closest to Lahore  
- Karachi (`gs3`) may only connect to satellites passing over southern Pakistan

---

This simple graph-theory model can be extended into a **FastAPI API** to dynamically compute satellite handovers based on live positions.
