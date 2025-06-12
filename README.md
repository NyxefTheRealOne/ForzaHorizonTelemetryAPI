# Forza Horizon Telemetry API

This API listens to Forza Horizon UDP telemetry data and exposes it via a simple RESTful HTTP API on your local machine. It enables you to query real-time car telemetry like engine RPM, steering input, tire slip, and more.

---

## Table of Contents

- [Overview](#overview)  
- [Setup](#setup)  
- [Running the API](#running-the-api)  
- [API Endpoints](#api-endpoints)  
- [Telemetry Parameters](#telemetry-parameters)  
- [Example Usage](#example-usage)  
- [Troubleshooting](#troubleshooting)  
- [License](#license)

---

## Overview

Forza Horizon 5 can broadcast live telemetry data over UDP. This Python-based API listens for those UDP packets on your local machine and provides an HTTP interface to access the latest telemetry values in JSON format.

---

## Setup

1. **Forza Horizon 5 Settings**  
   - Go to **Settings → HUD and Gameplay → Telemetry**  
   - Enable **Data Out**  
   - Set **IP Address** to `127.0.0.1` (or your PC’s LAN IP if accessing remotely)  
   - Set **Port** to `5300`  

2. **Python Requirements**  
   - Python 3.7+  
   - Install dependencies:  
     ```bash
     pip install flask
     ```

3. **Download the API Script**  
   Save the provided Python script (e.g., `fh5_telemetry_api.py`).

---

## Running the API

Run the script with:

```bash
python fh5_telemetry_api.py
```
This will:

Start listening for UDP telemetry packets on 127.0.0.1:5300

Launch a Flask HTTP server on 127.0.0.1:5000

---

##API Endpoints

###GET `/telemetry`
Returns the full latest telemetry data as JSON.

Example response:

```json
{
  "is_race_on": 1,
  "timestamp_ms": 123456789,
  "engine_max_rpm": 9000,
  "steering": -0.123,
}
```
###GET `/telemetry/<parameter>`
Returns the current value of a single telemetry parameter.

Replace <parameter> with any supported telemetry field (see below).

Example:
GET /telemetry/steering

Example response:

```json
{
  "steering": -0.123
}
```
If the parameter does not exist:

```json
{
  "error": "Parameter 'unknown_param' not found."
}
```
###GET `/telemetry/keys`
Returns a JSON array of all available telemetry parameter names.

Example response:

```json
[
  "is_race_on",
  "timestamp_ms",
  "engine_max_rpm",
  "steering",
]
```

---

##Telemetry Parameters
The API supports the following parameters (with their typical meaning):

-Parameter	Description
-is_race_on	Whether race is active (1 = yes)
-timestamp_ms	Timestamp in milliseconds
-engine_max_rpm	Maximum RPM of the engine
-engine_idle_rpm	Idle RPM of the engine
-current_engine_rpm	Current engine RPM
-acceleration_x	Acceleration along X axis (m/s²)
-acceleration_y	Acceleration along Y axis (m/s²)
-acceleration_z	Acceleration along Z axis (m/s²)
-velocity_x	Velocity X component (m/s)
-velocity_y	Velocity Y component (m/s)
-velocity_z	Velocity Z component (m/s)
-angular_velocity_x	Angular velocity around X axis
-angular_velocity_y	Angular velocity around Y axis
-angular_velocity_z	Angular velocity around Z axis
-yaw	Yaw angle (radians)
-pitch	Pitch angle (radians)
-roll	Roll angle (radians)
-suspension_travel_fl	Suspension travel front-left (meters)
-suspension_travel_fr	Suspension travel front-right (meters)
-suspension_travel_rl	Suspension travel rear-left (meters)
-suspension_travel_rr	Suspension travel rear-right (meters)
-tire_slip_ratio_fl	Tire slip ratio front-left
-tire_slip_ratio_fr	Tire slip ratio front-right
-tire_slip_ratio_rl	Tire slip ratio rear-left
-tire_slip_ratio_rr	Tire slip ratio rear-right
-wheel_rotation_speed_fl	Wheel rotation speed front-left
-wheel_rotation_speed_fr	Wheel rotation speed front-right
-wheel_rotation_speed_rl	Wheel rotation speed rear-left
-wheel_rotation_speed_rr	Wheel rotation speed rear-right
-wheel_on_ground_fl	Wheel contact with ground (bool) front-left
-wheel_on_ground_fr	Wheel contact with ground (bool) front-right
-wheel_on_ground_rl	Wheel contact with ground (bool) rear-left
-wheel_on_ground_rr	Wheel contact with ground (bool) rear-right
-suspension_travel_raw_fl	Raw suspension travel front-left
-suspension_travel_raw_fr	Raw suspension travel front-right
-suspension_travel_raw_rl	Raw suspension travel rear-left
-suspension_travel_raw_rr	Raw suspension travel rear-right
-tire_slip_angle_fl	Tire slip angle front-left
-tire_slip_angle_fr	Tire slip angle front-right
-tire_slip_angle_rl	Tire slip angle rear-left
-tire_slip_angle_rr	Tire slip angle rear-right
-tire_combined_slip_fl	Combined tire slip front-left
-tire_combined_slip_fr	Combined tire slip front-right
-tire_combined_slip_rl	Combined tire slip rear-left
-tire_combined_slip_rr	Combined tire slip rear-right
-suspension_pos_fl	Suspension position front-left
-suspension_pos_fr	Suspension position front-right
-suspension_pos_rl	Suspension position rear-left
-suspension_pos_rr	Suspension position rear-right
-suspension_velocity_fl	Suspension velocity front-left
-suspension_velocity_fr	Suspension velocity front-right
-suspension_velocity_rl	Suspension velocity rear-left
-suspension_velocity_rr	Suspension velocity rear-right
-wheel_rotation_angle_fl	Wheel rotation angle front-left
-wheel_rotation_angle_fr	Wheel rotation angle front-right
-wheel_rotation_angle_rl	Wheel rotation angle rear-left
-wheel_rotation_angle_rr	Wheel rotation angle rear-right
-throttle	Throttle pedal input (0.0 - 1.0)
-brake	Brake pedal input (0.0 - 1.0)
-clutch	Clutch pedal input (0.0 - 1.0)
-handbrake	Handbrake input (0.0 - 1.0)
-steering	Steering input (-1.0 full left, 1.0 full right)
-normalized_driving_line	Normalized driving line position
-normalized_ai_brake_diff	Normalized AI brake difference

Example Usage
Using curl:
```bash
curl http://127.0.0.1:5000/telemetry/steering
```
Using Python requests:
```python

import requests

resp = requests.get("http://127.0.0.1:5000/telemetry")
data = resp.json()

print("Current RPM:", data['current_engine_rpm'])
print("Steering Input:", data['steering'])
```

---

##Troubleshooting
**No data received?**

Confirm FH5 telemetry output IP and port exactly match your script's UDP bind address and port.

Check your firewall settings to allow UDP port 5300 inbound.

**Empty JSON {}?**

Try driving in-game to generate telemetry packets.

Check console logs for UDP packet reception confirmation.

---

##License
MIT License — Feel free to use and modify!
