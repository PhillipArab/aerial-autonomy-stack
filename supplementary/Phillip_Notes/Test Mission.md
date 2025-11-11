# Phillip's First AAS Mission

## Takeoff. Drone_ID is env variable in Xterminal. can see it with: ``echo $DRONE_ID`` . can see action list with: ``ros2 action list | grep /Drone``
```
python3 /aas/aircraft_resources/patches/cancellable_action.py "ros2 action send_goal /Drone${DRONE_ID}/takeoff_action autopilot_interface_msgs/action/Takeoff '{takeoff_altitude: 40.0}'"
```

## Cancel Action. must do between each action, can mannually CTRL + C , or use `ros2 action cancel /Drone$DRONE_ID/<action_name>`
```
ros2 action cancel /Drone$DRONE_ID/takeoff_action
```
