# AAS Quadcopter Mission Cheatsheet

- Drone_ID is env variable in Xterminal. can see it with: `echo $DRONE_ID` . can see action list with: `ros2 action list | grep /Drone`

- Can only execute certain commands from certain states

## ~~Cancel Action (Work In Progress)~~
~~Must do between each action, can mannually CTRL + C , or use `ros2 action cancel /Drone$DRONE_ID/<action_name>`~~
```
# ros2 action cancel /Drone$DRONE_ID/takeoff_action
```

## Takeoff 
Environement Variable: `takeoff_altitude`
```
python3 /aas/aircraft_resources/patches/cancellable_action.py "ros2 action send_goal /Drone${DRONE_ID}/takeoff_action autopilot_interface_msgs/action/Takeoff '{takeoff_altitude: 40.0}'"
```

## Land
Environment Variable: `landing_altitude` does not matter for quadcopter
```
python3 /aas/aircraft_resources/patches/cancellable_action.py "ros2 action send_goal /Drone${DRONE_ID}/land_action autopilot_interface_msgs/action/Land '{landing_altitude: 60.0}'"
```

## Orbit
Environment Variables: `north`, `east`, `altitude`, `radius`
```
python3 /aas/aircraft_resources/patches/cancellable_action.py "ros2 action send_goal /Drone${DRONE_ID}/orbit_action autopilot_interface_msgs/action/Orbit '{north: 0.0, east: 50.0, altitude: 100.0, radius: 20.0}'"
```

## ~~Offboard~~ Still not sure what this does
Environment Variables: `offboard_setpoint_type`, `max_duration_sec` 
```
python3 /aas/aircraft_resources/patches/cancellable_action.py "ros2 action send_goal /Drone${DRONE_ID}/offboard_action autopilot_interface_msgs/action/Offboard '{offboard_setpoint_type: 1, max_duration_sec: 2.0}'"
```

## ~~Set Speed~~ Didn't get to work
Environment Variable: `speed`
```
ros2 service call /Drone${DRONE_ID}/set_speed autopilot_interface_msgs/srv/SetSpeed '{speed: 15.0}'
```

## Set Location
Environment Variables: `north`, `east`, `altitude`
```
ros2 service call /Drone${DRONE_ID}/set_reposition autopilot_interface_msgs/srv/SetReposition '{north: 200.0, east: 100.0, altitude: 60.0}' 
```

## Wind (in host terminal)
Environment Variables: `x`, `y`, `enable_wind`
```
docker exec simulation-container bash -c " \
  gz topic -t /world/\$WORLD/wind/ -m gz.msgs.Wind \
  -p 'linear_velocity: {x: 0.0 y: 3.0}, enable_wind: true'"
```