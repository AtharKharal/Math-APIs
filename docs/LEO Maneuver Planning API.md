# LEO Maneuver Planning API – Comprehensive Overview

## 1. Purpose and Scope
The **LEO Maneuver Planning API** provides computational services for low Earth orbit satellite trajectory optimization. It is designed to assist satellite engineers, mission planners, and researchers in simulating, calculating, and optimizing orbital maneuvers using mechanics and differential equations. The API enables users to perform calculations such as delta-v requirements, burn planning, and orbital transfers without needing to implement complex orbital mechanics algorithms locally.  

This API serves as a proof-of-concept for exposing **advanced mathematical models as ready-to-use web services**, bridging the gap between theoretical computations and operational use.

---

## 2. Core Features
- Compute **delta-v requirements** for orbital transfers, including Hohmann and bi-elliptic maneuvers.
- Estimate **propellant consumption** using the Tsiolkovsky rocket equation.
- Generate **burn schedules** for satellites, including time, duration, and direction vectors.
- Support input via standard **state vectors or classical orbital elements**.
- Output structured JSON suitable for integration with mission planning tools or other APIs.
- Provide extensibility for additional maneuver types, multi-satellite coordination, and time-varying orbit analysis.

---

## 3. Methodology
The API leverages fundamental orbital mechanics principles and numerical methods:

1. **Two-Body Dynamics** – Basic propagation of satellite position and velocity in Earth-centered inertial frames.
2. **Differential Equations** – Solving motion under central gravitational force; optional extensions to include perturbations.
3. **Delta-v Calculation** – Analytical formulas for circular-to-circular orbit transfers; includes plane-change components.
4. **Propellant Estimation** – Tsiolkovsky rocket equation:$$\Delta v = I_{sp} \cdot g_0 \cdot \ln \frac{m_0}{m_f}$$
5. **Burn Scheduling** – Single or multiple impulses timed according to orbital periods and mission constraints.
6. [Technical Documentation of LEO Maneuver API](https://documenter.getpostman.com/view/46925086/2sB3BHjoZo#e6cef870-ad2a-423d-938f-aeb007b4b626)

---

## 4. Input Parameters
The API accepts JSON payloads with the following fields:

````json
{
  "epoch": "2025-10-01T00:00:00Z",
  "frame": "ECI_J2000",
  "state": {
    "r_km": [6871.0, 0.0, 0.0],
    "v_km_s": [0.0, 7.61, 1.0]
  },
  "spacecraft": {
    "mass_kg": 220.0,
    "propellant_kg": 35.0,
    "isp_s": 320
  },
  "target_orbit": {
    "a_km": 7078.0,
    "e": 0.001,
    "i_deg": 98.0
  },
  "objective": "min_total_dv"
}
````


**Key fields:**
- `epoch`: reference time for the maneuver.    
- `state`: current satellite position and velocity vectors.    
- `spacecraft`: mass, propellant, and specific impulse.    
- `target_orbit`: desired orbit in terms of semi-major axis, eccentricity, and inclination.    
- `objective`: optimization criterion, e.g., minimizing total delta-v.    

---

## 5. Output Structure

The API returns a detailed JSON response:

````json
{
  "summary": {
    "initial_mass_kg": 220.0,
    "final_mass_kg": 219.15,
    "total_dv_m_s": 3.85,
    "burns_count": 1,
    "time_of_flight_hours": 2.14
  },
  "burns": [
    {
      "burn_number": 1,
      "start_time_utc": "2025-10-01T01:03:12Z",
      "duration_s": 9.3,
      "delta_v_m_s": 3.85,
      "direction_vector_rsw": [0.92, 0.38, 0.0],
      "propellant_used_kg": 0.85
    }
  ],
  "target_orbit_achieved": {
    "a_km": 7078.0,
    "e": 0.001,
    "i_deg": 98.0
  },
  "propagation_notes": {
    "gravity_model": "two-body",
    "drag_model": "off",
    "srp_model": "off"
  }
}


````

**Highlights:**
- `summary`: quick overview of mission feasibility and resources used.    
- `burns`: detailed information for each maneuver impulse.    
- `target_orbit_achieved`: final orbital parameters for verification.    
- `propagation_notes`: information about the simplifications and physics models applied.    

---

## 6. Architecture

- **Backend:** Python + FastAPI    
- **Hosting:** Vercel (serverless functions for instant scalability)    
- **Computation:** Numpy for vector math, custom orbital mechanics routines    
- **Extensibility:** Designed to integrate with other CASPAM APIs (e.g., Orbital Handover API) for combined multi-satellite simulations  

---

## 7. Example Usage

**POST request:**

````bash
curl -X POST http://your-vercel-app.vercel.app/api/leo-maneuver \
  -H "Content-Type: application/json" \
  -d @input.json
````

**Response:** JSON as shown in Section 5.

---

## 8. Potential Applications

- Mission planning for new LEO satellites.    
- Collision avoidance maneuver planning in dense constellations.    
- Educational tools demonstrating orbital mechanics.    
- Integration with scheduling APIs for satellite observation or data relay.    

---

## 9. Notes and Limitations

- Currently uses simplified two-body physics; atmospheric drag and perturbations are ignored in the default model.    
- Only impulsive maneuvers are supported; continuous-thrust maneuvers require extensions.    
- Designed as a **proof-of-concept**; further optimization can include multi-satellite coordination and time-varying constraints.