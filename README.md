# research_track2_first
# Research Track 2 - first assignment

Elham Mohammadi [5073904]

  launch->    launch files (
    sim.launch            . Gazebo simulation
    sim_bridge.launch     . launch python scripts
    sim_coppelia.launch   . launch nodes )  
    
  scripts->       - python scripts
(    go_to_point.py        . controlling the robot
    user_interface.py      . minimal command line UI )
    
  src->        - C++ source code
(    position_service.cpp  . returns random position
    state_machine.cpp    . manages the FSM logic )
    
  srv->    - custom services
(    Command.srv           . user UI input
    Position.srv           . goal position
    RandomPosition.srv     . bound random pose )
    
  urdf->  - robot description for Gazebo simulation
(    my_robot.urdf         . mobile robot description)

  cplScenes-> - CoppeliaSim scenes
 (   pioneer_scene.ttt     . Pioneer p3dx scene 
    robotnik_scene.ttt     . Robotnik Summit XL140701 scene )

## the decription of the packages

This package controls a mobile non-holonomic robot with a simple 'go_to_point' behaviour:
1. a random goal is issued (a _pose_, [x,y,theta]);
2. the robot orients itself towards the [x,y] destination;
3. it then drives straight to that position (adjusting the orientation if need be);
4. having reached the [x,y] goal position the robot turns in place in order to match the goal _theta_;
5. if the user does not stop the robot GOTO step 1, otherwise stay still until asked to start again, then GOTO step 1;

Since the user request is here implemented as a service it cannot be preempted, stopping the robot is possible only once a goal is reached, restarting it when issuing a new goal.

## contents

This package contains 4 nodes managing the aforementioned control of a mobile robot.
Two nodes are implemented as python scripts
- **go_to_point.py**: the service server managing the robot speed control depending on the goal received.
- **user_interface.py**:  the simple command line user interface, which sends the requests to start/stop the go_to_point behaviour.

Whilst the last two are C++ based nodes
- **position_service.cpp**: the server generating a random pose [x,y,theta] as a response to a request.
- **state_machine.cpp**:  the FSM managing the request of a new pose when needed, sending it as a goal request to 'go_to_point' service server.

Finally, the control can be applied to a robot simulated using Coppeliasim (see **Requirements**), for which two scenes are here presented
- **pioneer_scene.ttt**: a simple scene with a Pioneer p3dx non-holonomic mobile robot in an empty environment.
- **robotnik_scene.ttt**: a simple scene with a Robotnik Summit XL140701 non-holonomic mobile robot in an empty environment.

## Compiling and Running

Compilation can be carried out as always with

$catkin_make

" $ roslaunch rt2_assignment1 sim.launch "

In this case the Gazebo simulation will automatically start.
 " $roslaunch rt2_assignment1 sim_coppelia.launch "

In this case CoppeliaSim must be started separately (remember to have an instance of roscore running before launching the CoppeliaSim executable). The simulation can be either started before or after launching the nodes, but do not try to run a new simulation when the nodes have already been running on a previous instance or the system could find itself in an initial state different from the assumed one, never being able to reach the goal. In other terms, each time the simulation is restarted the nodes should be restarted to, and vice-versa (generally).

" $ roslaunch rt2_assignment1 sim_bridge.launch "


