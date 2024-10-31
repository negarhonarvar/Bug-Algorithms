# Bug Algorithms in a Maze
<p align = "center">
 <img src="https://github.com/user-attachments/assets/fdbef732-7f0d-437a-9ad8-a44e5469da70" width = "400" >
</p>
We shall implement Bug1 , Bug2 and wall following algorithm for guiding an GCTronic' e-puck rebot through a maze to a target position.
<p align = "center">
   <img src="https://github.com/user-attachments/assets/ed615e48-3640-433d-b337-4eb0f6675363" width = "400" >
</p>
## Bug 1 and Map Gathering
Bug 1 is implemented mostly similar to bug2 but with a few number of differences  :
1. **Exploration Mechanism:**
   - **Bug1:** The robot uses a comprehensive exploration strategy, meaning it will extensively search and explore the environment to find its way.
   - **Bug2:** The robot makes decisions based on proximity to the target, which can limit unnecessary exploration.

2. **Chase Goal:**
   - **Bug1:** The robot doesn't necessarily chase the goal directly but instead focuses on a strategy to avoid obstacles and explore the environment.
   - **Bug2:** The robot continuously checks if it can move directly towards the goal along the M-line (a direct line from start to goal). If it can, it will resume moving towards the goal.

3. **Path Comparison:**
   - **Bug1:** The robot may take a more complex and longer path due to its exploration-focused approach.
   - **Bug2:** The robot tends to follow a more direct path, with less deviation, as it tries to stick to the M-line whenever possible.

In summary, **Bug2** is generally more efficient than **Bug1** in environments with obstacles because it tries to minimize unnecessary exploration and follows a more direct path towards the goal.
<p align = "center">
  <img src="https://github.com/user-attachments/assets/cb769ad6-d03f-43ce-9302-a3e9c2afa854" width = "400" >
</p>

Using Distance Sensor with Threshold 0.05 for Gathering a Map of Maze:
The distance sensor is used to detect obstacles. The threshold of 0.05 implies that when the sensor detects an object closer than this distance, it triggers a response (e.g., the robot considers this a boundary or obstacle).
In essence, the robot uses the Bug1 algorithm for navigation while applying the Split and Merge method to process sensor data, particularly from a distance sensor, to identify and manage obstacles effectively. The threshold helps in distinguishing when an obstacle is close enough to require the robot to adjust its path.
<p align = "center">
  <img src="https://github.com/user-attachments/assets/085b3d90-7ef0-452e-849c-00daab557eb3" width = "400" >
</p>
## Bug 2
This code's algorithm is designed to guide a robot using the "wall-following" approach. This method is commonly used in environments with obstacles and mazes to prevent the robot from colliding with obstacles and to help it find a path around them. The algorithm operates in several main states:

1. Initial State:
   The robot starts at an initial point with coordinates `Init_x` and `Init_y` and aims to reach a target point with coordinates `Goal_x` and `Goal_y`. To achieve this goal, a line called the "M-line" is drawn from the initial point to the target point. The robot's goal is to follow this line as long as it doesn't encounter any obstacles.

2. Calculating the M-Line:
   At the start of the code, the slope and bias of this line are calculated using the equation \( y = ax + b \). These values are then used to determine whether the robot is on this line or not. Due to sensor errors, it is nearly impossible for the robot to be exactly on this line, so a threshold (`thresh`) is defined. If the computed value for the robot's position falls within this threshold, the robot is considered to be on the line.

   The function used for this calculation is:
   ```python
   def M_Line(point, thresh=10**(-3)):
       global slope, bias
       result = abs(slope * point[0] - point[1] + bias) / sqrt(slope**2 + 1)
       return result <= thresh
   ```

3.Reading Sensor Values:
   This problem uses a set of sensors, including GPS, Compass, and 5 Distance Sensors. The 5 sensors provide distance values such that:
   - If there is no obstacle in the sensor's direction, the sensor value is 1.0.
   - As the robot gets closer to an obstacle, the sensor's value decreases, reaching near zero upon collision. To prevent the robot from hitting an obstacle, sensor values are continuously monitored, and if one of them falls below a threshold (set to 0.035), the robot begins to turn.

4. Determining the Wall State:
   While moving, several states related to the walls may occur:
   - **Losing_wall:** If the robot is moving away from the walls, this is detected through sensor values, and the robot attempts to reorient towards the walls.
   - **Boundary_wall:** If there are walls around the robot, it continues moving along them and may transition to other states.
   - **On_Mline:** If it is detected that the robot is on the M-line and there are no walls within the threshold distance ahead, it continues moving in that direction until it gets too close to a wall.
   - In the final state, the robot detects a wall closer than the threshold and starts turning to avoid the obstacle.

Considering the overall BUG2 algorithm, the robot may traverse around the maze multiple times, eventually reaching its goal by following the M-line. The implemented algorithm includes a mechanism for making turning decisions that reduce exploration but increase exploitation. This mechanism records the robot's last turn direction and chooses to turn in the opposite direction the next time it needs to avoid an obstacle.
<p align = "center">
  <img src="https://github.com/user-attachments/assets/32a42d7c-6fa6-4bec-9ef9-947e7ab051c9" width = "400" >
</p>

## Wall Following
This aproach is mostly similar to bug2 but the only priority for our robot is to stay close to wall. The path taken with this algorithm is shown below :
<p align = "center">
  <img src="https://github.com/user-attachments/assets/4d53db20-f006-43e0-a37d-2098bf923b08" width = "400" >
</p>
