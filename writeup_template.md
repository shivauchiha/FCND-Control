## Project: 3D Control


---


# Required Steps for a Passing Submission:
1. Implemented body rate control in C++. 
2. Implement roll pitch control in C++.
3. Implement altitude controller in C++.
4. Implement lateral position control in C++.
5. Implement yaw control in C++.
6. Implement calculating the motor commands given commanded thrust and moments in C++.
7. Tune the parameters to pass all five scenarios.
8. Write up


## [Rubric](https://review.udacity.com/#!/rubrics/1643/view) Points
### Here I will consider the rubric points individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one.  You can submit your writeup as markdown or pdf.  

You're reading it! Below I describe how I addressed each rubric point and where in my code each point is handled.


### Implementing Your Path Planning Algorithm

#### 1. Implemented body rate control in C++.
The job of this function is to provide appropriate momentum cmds for the drone to perform to achieve control objective.These momentum cmd follow the control law


	(pqrCmd-pqr)*Inertia*kpPQR;



As you can see the cmds are a function of desired pitch , roll and yaw rates.The moments of inertia are calculated based on mass and form factor of the drone.[List of moments of inertia](https://en.wikipedia.org/wiki/List_of_moments_of_inertia)
 

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





