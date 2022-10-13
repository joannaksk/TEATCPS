# Test Effort Alleviation Through Test Case Prioritization in Simulink

This webpage provides extended information about the method utilised and results observed when conducting the feasibility study about how test effort may be alleviated through test case prioritisation using a mechanical plant model of a cyber physical system.


## 1. Method
To answer the research questions, we identified the following high level requirements to develop research artifacts:

Figure 1 shows a summary of the method used.
<figure>
<p align="center"><img src="METHOD.JPG" width="400"/></p>
<figcaption align="center">Figure 1: Method in a nutshell.</figcaption>
</figure>

## 1.1. Robot Assembly and Control Model Implementation

The robot was built using [LEGO MINDSTORMS EV3](https://education.LEGO.com/en-us/products/LEGO-mindstorms-education-ev3-core-set/5003400#
The [MATLAB Support Package for Raspberry Pi](https://nl.mathworks.com/hardware-support/raspberry-pi-MATLAB.html) was used to create the control model(controller) that is run on the Raspberry PI hardware and controls the movement of the robot.
<figure>
<p align="center"><img src="robot.png" width="250"/></p>
<figcaption align="center">Figure 2: Robot.</figcaption>
</figure>
The control model of the robot was altered so that the LineFollowControl model component was it's own model that could be referenced by other models. This was done to allow it to be used to control the plant model of the robot during simulation.
<figure>
<p align="center">[<img src="control model altered.png" width="600"/>] (control model altered.png)</p>
<figcaption align="center">Figure 3: The figure shows the control model altered by extracting the LineFollowControl sub-model and re-importing it as a Referenced model.</figcaption>
</figure>
<figure>
<p align="center"><img src="LineFollowControl.png" width="600"/></p>
<figcaption align="center">Figure 4: The LineFollowControl sub-model that contained the PID control logic.</figcaption>
</figure>

## 1.2. Plant Model Implementation
A simplified model of the line follower robot was created in within the Simscape Multibody™ environment. Most of the work effort was placed on the components of the robot that interacted with the control model such as the actuators, sensors, ground and lane. Less emphasis was placed on modelling components that did not considerably affect the simulation results such as the body component.

### 1.2.1. Ground and Lane Implementation
The ground was modelled as an [Infinite plane](https://in.mathworks.com/help/physmod/sm/ref/infiniteplane.html) block connected to the World Frame block. The lane was implemented as a pair of nonintersecting [splines](https://in.mathworks.com/help/physmod/sm/ref/spline.html). This way, the track could be defined using mathematical functions.
<figure>
<p align="center"><img src="Ground Model.png" width="200"/></p>
<figcaption align="center">Figure 5: The figure shows the ground and track implemented as an infinite plane block and a pair of spline blocks
</figure>

### 1.2.2. Sensor Subsystem Implementation
The sensor subsystem comprised 2 [brick solid](https://in.mathworks.com/help/physmod/sm/ref/bricksolid.html) blocks that were rigidly connected to the front of the body. A [Transform Sensor](https://in.mathworks.com/help/physmod/sm/ref/transformsensor.html) block was attached to each sensor
The scalar values we obtained were then passed into 2 independent functions that helped track the position of the model robot within the model track.
<figure>
<p align="center"><img src="Sensor Subsystem.png" width="600"/></p>
<figcaption align="center">Figure 6: The figures shows a close up of the sensor subsystem that models the sensors of the physical robot.</figcaption>
</figure>

### 1.2.3. Controller Implementation
The *LineFollowControl* block, Figure 4, of the control model of the actual robot was extracted into it’s own

### 1.2.4. Motor Subsystem Implementation
The Motor Subsystem was modelled using a constant block that has a configurable ideal torque input of [0.2Nm](https://www.LEGO.com/cdn/cs/set/assets/bltbef4d6ce0f40363c/LMSUser_Guide_LEGO_MINDSTORMS_EV3_
Within the plant model, since the wheels were represented by [Revolute joints](https://www.mathworks.com/help/physmod/sm/ref/revolutejoint.html), the actuation force
<figure>
<p align="center"><img src="Motor Subsystem.png" width="600"/></p>
<figcaption align="center">Figure 7: The Motor Subsystem.</figcaption>
</figure>

### 1.2.5. Wheel Subsystem Implementation
Each wheel, Figure 8, was modelled using 3 pairs of [Spatial Contact Force](https://in.mathworks.com/help/physmod/sm/ref/spatialcontactforce.html) blocks and [Spherical

<figure>
<p align="center"><img src="Wheel Subsystem.png" width="600"/></p>
<figcaption align="center">Figure 8: The Wheel Subsystem.</figcaption>
</figure>

The Spatial Contact Force blocks

### 1.2.6. Body Implementation
The rest of the body, Figure 9, was implemented as a single Brick solid block. There was a single
<figure>
<center><img src="Body.png" width="600"/></center>
<figcaption align="center">Figure 8: The Body.</figcaption>
</figure>

## 1.3. Shared Configuration Creation
The plant model was designed so that it had a high degree of parameterization that would help create variants of the base model later on.
Since the LineFollowControl sub model needed to be shared by 2 independent models, it was necessary to create a shared configuration data store.

## 1.4. Template Definition
The parameterization of the plant model enabled the creation of variants of the base model with the same

## 1.5. Test Suite Creation
The test suite was created according to the 4 key principles of chaos

### 1.5.1. Principle 1: Hypothesis around Steady State Behaviour 
> The line follower robot always follows the line.

To prove that this hypothesis holds, two test cases were added that would demonstrate this steady state

### 1.5.2. Principle 2: Vary Real World Events
To vary real world events, the faults that were chosen were faults that could occur during the execution


The table below shows a mapping of the faults that were introduced to their corresponding test cases.
<center>

| Fault                     | Fault Code | Test Case Name      |
|---------------------------|------------|---------------------|
| F1 = Wheel Size Fault     | F1 : 1     | Wheel Fault 1.0     |
|                           | F1 : 2     | Wheel Fault 0.8     |
|                           | F1 : 3     | Wheel Fault 0.6     |
| F2 = Gyro Sensor Fault    | F2 : 1     | Obstacle Fault 1000 |
|                           | F2 : 2     | Obstacle Fault 0    |
| F3 = Light Sensor Fault   | F3         | Right Sensor Fault  |
| F4 = Motor Fault          | F4         | Right Motor Fault   |
| F5 = PID Controller Fault | F5         | PID Fault P 0       |

</center>

These five faults were selected because they are representative of real-world faults that could occur during

The test suite comprised of 10 test scenarios portraying different types of track arrangements. The main

For the physical implementation for each test scenario, this came down to the number of tiles used.

The model alterations included:

### 1.5.3. Principle 3: Run Experiments in Production

### 1.5.4. Principle 4: Automate Experiments to run Continuously

### 1.5.5. Test Scenarios

<figure>
<center><img src="Test Scenarios 1.png" width="600"/></center>
<figcaption align="center">Figure 9: The Physical Test Scenarios alongside their Virtual Counterparts.</figcaption>
</figure>

<figure>
<center><img src="Test Scenarios 2.png" width="600"/></center>
<figcaption align="center">Figure 10: The Physical Test Scenarios alongside their Virtual Counterparts.</figcaption>
</figure>

### 1.5.6. Faults

<figure>
<center><img src="HiL Wheel Size Fault.png" width="600"/></center>
<figcaption align="center">Figure 11: The HiL Wheel Size Fault.</figcaption>
</figure>
<figure>
<center><img src="HiL Obstacle Fault.jpg" width="600"/></center>
<figcaption align="center">Figure 12: The HiL Obstacle Fault.</figcaption>
</figure>
<figure>
<center><img src="HiL Sensor Fault.jpg" width="600"/></center>
<figcaption align="center">Figure 13: The HiL Sensor Fault.</figcaption>
</figure>
<figure>
<center><img src="HiL Motor Fault.jpg" width="600"/></center>
<figcaption align="center">Figure 14: The HiL Motor Fault.</figcaption>
</figure>

At the MiL test configuration level, the faults were implemented as changes to the model or to the model configuration.


## 2. Experimentation
To conduct the experiments and come up with answers to the research questions the following steps

1. Within the plant model, one of the faults was implemented. The fault implementation went in

### 2.2. RQ2. To what extent can this technique estimate the effectiveness of the test scenarios in case of real faults?

After all the test scenarios had been simulated, they were first ranked according to the product of the

### 3.1. Results

The table below shows the results of the test cases exercised first at the MiL level, (MiL) and then at the

<figure>
<center><img src="Results Table.png" width="600"/></center>
<figcaption align="center">Table 2: The Results.</figcaption>
</figure>
Thus they are stand-in values

### 3.2. Analysis
The number of faults detected by each test case was the main metric that was used to rank them. This

For each modelled test scenario the predicted route of the robot was plotted and this route was compared

Table 3 shows the accuracies of the route the robot was predicted to take according to the plant model

<figure>
<center><img src="Accuracies Table.png" width="600"/></center>
<figcaption align="center">Table 3: Test Scenario Predicted Accuracies. <br>
Each cell represents the percentage of x, y coordinates of the plant model’s translation that lay within
</figure>

Table 4 shows how well the plant model was able to predict the behavioural response of the robot to

<figure>
<center><img src="Average Prediction Accuracy Table.png" width="600"/></center>
<figcaption align="center">Table 4: Table showing the prediction accuracy of the plant model. <br>
The average prediction accuracy of the plant model with regard to the expected behaviour
</figure>

#### 3.2.1. Ranking and Answers to Research Questions
<figure>
<center><img src="Ranking Table 1.png" width="800"/></center>
<figcaption align="center">Table 5: Ranking of Test Scenarios at the MiL and HiL levels.</figcaption>
</figure>




<figure>
<center><img src="Ranking Table 2.png" width="800"/></center>
<figcaption align="center">Table 6: Ranking of Test Scenarios at the MiL and HiL levels according to the four correlation combinations.</figcaption>
</figure>


**RQ1. Is it feasible to prioritize Hardware-in-the-Loop (HiL) test scenarios at the Model-in-the-




# Contact: 
Please do not hesitate to contact [Joanna Kisaakye](joannaksk85@gmail.com) for any questions and comments.