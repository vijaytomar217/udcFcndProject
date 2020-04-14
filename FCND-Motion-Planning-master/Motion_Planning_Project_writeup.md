## Project: 3D Motion Planning
![Quad Image](./misc/enroute.png)

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
[VT] This document provide the writeup on how i implemented this project.

### Explain the Starter Code

#### 1. Explain the functionality of what's provided in `motion_planning.py` and `planning_utils.py`
[VT] . motion_planning.py script contains additional state called Planning, function plan_path is used to calculate new path 
		between the buildings, this function also define target altitude and the safety distance to keep between drone and 
		the obstacles send_waypoints function is used to visualize the planned waypoints.
	 . planning_utils.py contains create_grid function, which creates a 2D grid representation of the obstacles, grid contains o's 
	 	where there is feasible space and 1's for the obstaces.
	 	A* is used to generate the path from from grid_sart to grid_goal.
	 	Action class is used to return a list of valid actions


### Implementing Your Path Planning Algorithm

#### 1. Set your global home position
Here students should read the first line of the csv file, extract lat0 and lon0 as floating point values and use the self.set_home_position() method to set global home. Explain briefly how you accomplished this in your code.
[VT] read and get the first line of csv file from csv.reader function, with this i extracted lat0 and lon0 and set global home

#### 2. Set your current local position
Here as long as you successfully determine your local position relative to global home you'll be all set. Explain briefly how you accomplished this in your code.
[VT] self.global_position is used to get drone's current location, then this is converted into local position with global_to_local function


#### 3. Set grid start position from local position
This is another step in adding flexibility to the start location. As long as it works you're good to go!
[VT] self.local_position is used to set the grid_start position, this is subtracted from north and east offsets, then i rounded the
	 float numbesrs to integers

#### 4. Set grid goal position from geodetic coords
This step is to add flexibility to the desired goal location. Should be able to choose any (lat, lon) within the map and have it rendered to a goal location on the grid.
[VT] grid_start position is used set the goal position and added random values to limit the north and east offsets.

#### 5. Modify A* to include diagonal motion (or replace A* altogether)
Minimal requirement here is to modify the code in planning_utils() to update the A* implementation to include diagonal motions on the grid that have a cost of sqrt(2), but more creative solutions are welcome. Explain the code you used to accomplish this step.
[VT] A* is modified to include diagonal motions on the grid to have a cost of sqrt(2). 4 new actions North-East, North-West,
	 South-East, South-West have been added in the action class to delta each action. Also, for valid actions function i added
	 if statements in order to check diagonal actions that do not make drone fly into obstacles.

#### 6. Cull waypoints 
For this step you can use a collinearity test or ray tracing method like Bresenham. The idea is simply to prune your path of unnecessary waypoints. Explain the code you used to accomplish this step.
[VT] I used collinearity test with an epsilon value of 4 to shorten full path and eliminate the collinear waypoints that are not 
	  necessary. Once full path is calculated, it goes to the shorten_path function which return a shorter list of waypoints.


### Execute the flight
#### 1. Does it work?
[VT] Yes, The code works and drone is able to fly in between waypoints start from grid start to goes to grid goal without hitting any
	 obstacles. Please find the the screenshot in Drone_Images folder describing way points and drone reaching destination and landed. Also,
	 find below log describing the actions taken by drone. 
	 I did find some issues with connection in my local when simulator started and the starting position of drone is close to building,
	 i have to restart multiple times in order to get to a different location.


### Double check that you've met specifications for each of the [rubric](https://review.udacity.com/#!/rubrics/1534/view) points.
  
# Extra Challenges: Real World Planning

For an extra challenge, consider implementing some of the techniques described in the "Real World Planning" lesson. You could try implementing a vehicle model to take dynamic constraints into account, or implement a replanning method to invoke if you get off course or encounter unexpected obstacles.


