# Haversine Function (Distance Calculation)

The **Haversine function** calculates the **great-circle distance** between two points on a sphere using their latitudes and longitudes. It is widely used in geospatial applications, navigation, and astronomy to find the shortest distance over the Earth's surface.

---

## Why We Need It
- The Earth is roughly spherical, not flat.  
- Euclidean distance formulas (like the Pythagorean theorem) **do not work accurately** over large distances.  
- The Haversine formula accounts for the **curvature of the Earth**.

---

## Formula

Given two points:

- Point 1: `(lat1, lon1)`  
- Point 2: `(lat2, lon2)`  

Convert degrees to radians and compute:

```
distance = 2 * R * arcsin( sqrt( sin²(Δlat/2) + cos(lat1) * cos(lat2) * sin²(Δlon/2) ) )
```

Where:

- `R` = Earth's radius (≈ 6371 km)  
- `Δlat = lat2 - lat1`  
- `Δlon = lon2 - lon1`  

---

## Step-by-Step Calculation

1. Convert latitude and longitude from **degrees to radians**.  
2. Compute differences: `dlat = lat2 - lat1`, `dlon = lon2 - lon1`.  
3. Compute `a`:

```
a = sin²(dlat/2) + cos(lat1) * cos(lat2) * sin²(dlon/2)
```

4. Compute `c`:

```
c = 2 * atan2(sqrt(a), sqrt(1-a))
```

5. Compute distance:

```
distance = R * c
```

---

## Python Implementation

```python
import math

def haversine(lat1, lon1, lat2, lon2):
    R = 6371  # Earth's radius in km
    phi1, phi2 = math.radians(lat1), math.radians(lat2)
    dphi = math.radians(lat2 - lat1)
    dlambda = math.radians(lon2 - lon1)

    a = math.sin(dphi/2)**2 + math.cos(phi1) * math.cos(phi2) * math.sin(dlambda/2)**2
    c = 2 * math.atan2(math.sqrt(a), math.sqrt(1-a))
    return R * c
```

---

## Example Usage

Distance between **Islamabad (33.6844°N, 73.0479°E)** and **Lahore (31.5497°N, 74.3436°E)**:

```python
distance = haversine(33.6844, 73.0479, 31.5497, 74.3436)
print(distance)  # ≈ 291 km
```

---

## Key Points

- Very accurate for short and long distances on Earth.  
- Assumes Earth is a perfect sphere (small approximation error vs. ellipsoid).  
- Essential for applications like **satellite coverage, flight routing, and GPS systems**.

---

## Haversine Function Diagram

```mermaid
graph TD
    Earth[Earth Sphere]:::earth
    A[Point A (lat1, lon1)]:::point
    B[Point B (lat2, lon2)]:::point
    C[Great-circle distance d = R * c]:::distance

    Earth --> A
    Earth --> B
    A --> C
    B --> C

    classDef earth fill:#cce5ff,stroke:#3399ff,stroke-width:2px;
    classDef point fill:#ffd699,stroke:#ff9933,stroke-width:2px;
    classDef distance fill:#d1ffd6,stroke:#33cc33,stroke-width:2px;
```

### Diagram Explanation

- **Point A and Point B** represent two locations on the Earth's surface.  
- **Arc between A and B** represents the **great-circle distance**, the shortest path over the sphere.  
- **d = R × c** is computed using the Haversine formula.  
- This accounts for the **Earth’s curvature**, unlike straight-line 2D distance.

# Haversine Example: Islamabad to Lahore

```mermaid
graph LR
    %% Nodes for cities
    Islamabad[Islamabad (33.6844°N, 73.0479°E)]:::city
    Lahore[Lahore (31.5497°N, 74.3436°E)]:::city

    %% Node for distance
    Distance[Great-circle distance ≈ 291 km]:::distance

    %% Arrows representing distance
    Islamabad --> Distance
    Lahore --> Distance

    %% Styling
    classDef city fill:#ffd699,stroke:#ff9933,stroke-width:2px;
    classDef distance fill:#d1ffd6,stroke:#33cc33,stroke-width:2px;
```

### Diagram Explanation

- **Islamabad** and **Lahore** are represented as nodes.  
- **Arrow connections** point to the computed **great-circle distance**.  
- This visual represents how the Haversine formula calculates the shortest path between the two cities over the Earth’s surface.  
- While not geographically scaled, it clearly shows the relationship between points and distance.

