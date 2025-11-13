# Shibuya Example

## Takeoff
```
python3 /aas/aircraft_resources/patches/cancellable_action.py "ros2 action send_goal /Drone${DRONE_ID}/takeoff_action autopilot_interface_msgs/action/Takeoff '{takeoff_altitude: 20.0}'"
```

## Travel
```
ros2 service call /Drone${DRONE_ID}/set_reposition autopilot_interface_msgs/srv/SetReposition '{north: 75.0, east: 90.0, altitude: -20.0}'
```

## Land on Building
```
ros2 service call /Drone${DRONE_ID}/set_reposition autopilot_interface_msgs/srv/SetReposition '{north: 75.0, east: 90.0, altitude: -50.0}'
```

## Land HOME
```
python3 /aas/aircraft_resources/patches/cancellable_action.py "ros2 action send_goal /Drone${DRONE_ID}/land_action autopilot_interface_msgs/action/Land '{landing_altitude: 20.0}'"
```