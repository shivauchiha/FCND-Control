## Project: 3D Control


---


# Required Steps for a Passing Submission:
1. Load the 2.5D map in the colliders.csv file describing the environment.
2. Discretize the environment into a grid or graph representation.
3. Define the start and goal locations.
4. Perform a search using A* or other search algorithm.
5. Use a collinearity test or ray tracing method (like Bresenham) to remove unnecessary waypoints.
6. Return waypoints in local ECEF coordinates (format for `self.all_waypoints` is [N, E, altitude, heading], where the drone’s start location corresponds to [0, 0, 0, 0].
7. Write it up.
8. Congratulations!  Your Done!

## [Rubric](https://review.udacity.com/#!/rubrics/1534/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it! Below I describe how I addressed each rubric point and where in my code each point is handled.

### Explain the Starter Code

#### 1. Explain the functionality of what's provided in `motion_planning.py` and `planning_utils.py`
The one main difference between the backyard_flyer and Motion Planning code is use of an auxilary support file planning_utils .This Planning_util offers tools for use in the Motion Planning code.Lets dive into the tools that planning util offers. 

1. create_grid()-This function generates a grid map based on the information extracted from real world regarding free space and obstacle dimension including its height.The returned grid map is a 2D configuration space.  

2. Action()-This is a class that define all possible motions(North,South,East,West) and their respective costs.It also contains property definitions which makes it easy to access this attributes.

3. valid_actions()-This function given a grid and position within the grid checks feasbility of taking a particular action.

4. a_star()-This function implements a* algorithm for finding minimum path to reach from path to goal.

5. heuristics()-This function defines a heuristic which is the measured used by A* to find an optimal path.

Apart from usage of above mentioned tools we also have a new state (Planning) added to our state machine.This state is set once global path planning is complete and takeoff transition is set and code proceeds as normal as seen in backyard flyer code. 

For visualisation there is a send_waypoints function that sends waypoint to simulator.

<!--
![Top Down View](./misc/high_up.png)

Here's | A | Snappy | Table
--- | --- | --- | ---
1 | `highlight` | **bold** | 7.41
2 | a | b | c
3 | *italic* | text | 403
4 | 2 | 3 | abcd
-->
### Implementing Your Path Planning Algorithm

#### 1. Set your global home position
For the first task, I extracted lat lon from first line of collider.csv file.This is basically the centre point of the map. I used this co-ordinates to be assigned as global home using the udacidrone api set_home_position() 

#### 2. Set your current local position
Next we want the path planning to start from your current position. This position should preferably be in NED co-ordinates.This current local position is acquired by passing self.global_position attribute to the gloabl_to_local() api of udacidrone.This gives the drones current location in local co-ordinates with respect to global home.

#### 3. Set grid start position from local position
Here we use the local position acquired in previous step and adjust it using offset so it is sort of transformed to the grid reference frame.Then this is passed as start position to A* algorithms. 

#### 4. Set grid goal position from geodetic coords
Here the hardcoded goal position is replaced by global_to_local() api which is fed with destination lat,lon acquired from user . check line 152.

Note:- Not all lat , lon values results in successful path find . I would like your advice in properly selecting lat , lon as goal position. Despite experimentation have still not figured out . But for one goal , I found by chance which validates all of the code logic.

#### 5. Modify A* to include diagonal motion (or replace A* altogether)
Here changes were made to Action enum class to add diagonal motion and the necessary checks were added to valid_actions. check line 54 and line 91.

#### 6. Cull waypoints 
Here I used collinearity test to prune paths . But in future I hope to add voxcel based planning and bresenham based pruning algorithm.





