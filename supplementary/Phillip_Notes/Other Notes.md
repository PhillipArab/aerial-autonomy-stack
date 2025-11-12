# Other Notes

## Issue with Conops Mission

- For running a specific mission try:
```
ros2 run mission mission --ros-args -r __ns:=/Drone$DRONE_ID -p use_sim_time:=true -p conops:=cat          
```

Need to reference uav global position to compare to repo_req so i can determine if its within tolerance and then end this phase of mission.

My mission code so far is lines 360. 
```
elif self.conop == 'AtoB':
            if self.mission_step == -1:
                self.get_logger().info("[Phillip] Mission failed")
                self.conops_timer.cancel() # Stop this timer
                return
            elif self.mission_step == 0:
                self.get_logger().info("[Phillip] Taking off")
                self.mission_step = 1 # Dummy step to wait for takeoff completion
                takeoff_goal = Takeoff.Goal()
                takeoff_goal.takeoff_altitude = 20.0
                #takeoff_goal.vtol_transition_heading = 300.0
                #takeoff_goal.vtol_loiter_nord = 100.0
                #takeoff_goal.vtol_loiter_east = 100.0
                #takeoff_goal.vtol_loiter_alt = 120.0
                self.send_goal(self._takeoff_client, takeoff_goal)
            elif self.mission_step == 2:
                self.get_logger().info("[Phil] Travelling")
                self.mission_step = 3 # Dummy step to wait for orbit completion
                repo_req = SetReposition.Request()
                repo_req.east = 50.0
                repo_req.north = 50.0
                repo_req.altitude = 35.0
                if os.getenv('AUTOPILOT', '') == 'px4':
                    time.sleep(1.5) # Quick and dirty way to make sure the autopilot is fully out of Takeoff mode 
                self.call_service(self._reposition_client, repo_req)
            elif self.mission_step == 3:
                #
                pos_goal = [repo_req.east, repo_req.north, repo_req.altitude]
                #distance = math.dist(self.something, pos_goal)
                #
            elif self.mission_step == 6:
                self.get_logger().info("[Phil] Mission complete")
                self.conops_timer.cancel() # Stop this timer
```

the subscriber initiation that i might need is at lines 138ish.
```
    def px4_global_position_callback(self, msg): # Mutally exclusive with mavros_global_position_callback
        with self.data_lock:
            self.lat = msg.lat
            self.lon = msg.lon
            self.alt_msl = msg.alt
```
