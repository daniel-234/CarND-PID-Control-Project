# **PID Controller** 


## Reflection

---

**Describe the effect each of the PID Components had in your implementation**

The Proportional term in the PID controller is proportional to the current value of the CTE. 
If we set the center line of the road as our reference trajectory, the CTE is the lateral distance between the vehicle and that reference trajectory. 

To control the car direction, we have to set the proportional term in such a way that if the car deviates from the optimal trajectory, the controller should make it steer and correct that deviation. 
So if the car is moving to the right of the road, the CTE increases and the proportional term should be set to a negative value so that the steering angle could be negative. If the CTE is negative, the proportional term should be positive. 
In order to do that, the proportional term is negative and equal to -Kp * p_error. The first value that was chosen for its constant Kp is 0.5, to avoid the effect of overshooting that was observed when the value was set to 1. 

To help reduce the overshoot from the proportional term, we can tune the D (derivative) parameter of the PID controller. The D parameter gives us the temporal derivative of the cross track error and lets the controller know that it has already reduced the error. As the error becomes smaller over time, the car reduces the angle of counter steering and this will allow it to approach the target trajetory more gracefully. 
This term is represented in the total error formula as -Kd * d_error. 

The integral cross track error is the sum of all the cross track errors observed thus far. When the steering mode of a car leads it to a trajectory that is far away from its optimal trajectory, the car needs to steer more and more as time goes by to compensate the bias that has been created. This bias is calculated as the sum of the cross track errors over time. The integral cross track error is measured as the integral of the sum of the cross track errors over time. This integral term becomes larger as time goes by and will eventually correct the robot's motion. 

The parameters were chosen initially to set a starting value and observe the car behavior. 

I recorded short videos of the car behavior while setting the above parameters. The [first video](./output_images/no_component.mp4) shows the car with no parameters set and it doesn't steer. 
In the [second video](./output_images/proportional_component.mp4) I've only set the proportional parameter to a value of 0.5 and the car steers heavily in the opposite direction. 
In the [third video](./output_images/proportional_component.mp4) the derivative parameter was set to 0.5; it smooths the steering angle a bit, although it isn't still able to keep the car on track. 
In the [fourth video](./output_images/proportional_component.mp4) the integral parameter has been added. Although it has been set to an arbitrary value of 0.005, the car starts to steer heavily again, due to the nature of the integral error being a sum af all the previous errors and therefor increasing in value as time passes. 

After some time spent tuning the parameter values, I found that the best ones to keep the car on track were suggested in this thread: https://knowledge.udacity.com/questions/703789. 
The final result is shown [here](./output_images/final_video.mp4). 