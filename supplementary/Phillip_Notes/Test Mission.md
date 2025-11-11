# Phillip's First AAS Quadcopter Mission

## Takeoff 
Drone_ID is env variable in Xterminal. can see it with: `echo $DRONE_ID` . can see action list with: `ros2 action list | grep /Drone`
Environement Variable: `takeoff_altitude`
```
python3 /aas/aircraft_resources/patches/cancellable_action.py "ros2 action send_goal /Drone${DRONE_ID}/takeoff_action autopilot_interface_msgs/action/Takeoff '{takeoff_altitude: 40.0}'"
```

## Cancel Action
Must do between each action, can mannually CTRL + C , or use `ros2 action cancel /Drone$DRONE_ID/<action_name>`
```
ros2 action cancel /Drone$DRONE_ID/takeoff_action
```

## Land
Environment Variable: `landing_altitude` does not matter for quadcopter
```
python3 /aas/aircraft_resources/patches/cancellable_action.py "ros2 action send_goal /Drone${DRONE_ID}/land_action autopilot_interface_msgs/action/Land '{landing_altitude: 60.0}'"
```
