# Jack In a Box

[video1](https://github.com/JihaiZhao/Jack-In-a-Box/assets/99274626/9d1a8c2c-1278-4db5-882c-3a2ad86330e8)

## Overview
The goal of this project is to simulate a jack bouncing inside of a moving box. The drawing below shows the configuration and transformation of frames I used. For simulation, I simulate the jack starting at the center of the box with zero initial velocity and zero theta for 10 seconds with a time step of 0.01s.

## Drawing of the system and all necessary rigid body transformation 

![Alt text](https://github.com/JihaiZhao/Jack-In-a-Box/assets/99274626/585ea6ca-77ff-456c-9f81-eedfd24936d0)

Frame W is the world frame, frame A is the frame of the center of box, frame B is the frame of the center of the jack. 

g_B1, g_B2, g_B3, g_B4 are the transformations from frame B to the four edges of the jack. 


![Screenshot from 2023-12-07 20-32-20](https://github.com/JihaiZhao/Jack-In-a-Box/assets/99274626/c0665fb1-8ace-4a21-9af6-a775b4971a92)

## Calculation description
How calculated the Euler-Lagrange equations, the constrains, the external forces and impact update law. 

Lagrangian equation of the system: 

$\ L = KE - V$

For kinetic energy, first get the body velocity of the box by calculating their rigid body transformation to world frame. Next convert this 4 by 4 matrix to a 6 by 1 vector and last calculate KE. (I assume the center of the mass is the center of the geometry). Last, use the same method for the jack. 

$\ KE = \frac{1}{2}⋅ω^T⋅I⋅ω$

For potential energy, get the y value of the box relative to world frame and use the same method 	for the jack 

$\ V = mgh$  

Euler-Lagrange: 

$\ \frac{∂}{∂t}\frac{∂L}{∂\dot{q}}−\frac{∂L}{∂q}=F$

The constrains: 

There are a total of 16 constraints for this system. They are each edge of the jack reach the four sides of the box. 

The external force: 

I tried different magnitudes of force, and I found the two blow works best for me. 

$\ F_y = m_{box} * g *1.012$

$\ F_theta = m_{box} * g * 0.5$

The impact update law: 

![Screenshot from 2023-12-07 22-09-47](https://github.com/JihaiZhao/Jack-In-a-Box/assets/99274626/ffdf232c-ea46-4f9d-90b1-6e5ee8949cac)

The impact happened when one edge of the jack collided with one side of the box. P is the momentum; H is the Hamiltonian of the system. 

After defining the 16 constrains of the system, evaluate the related expressions at $\ τ^+$ and $\ τ^-$ and solve them for $\ \dot{q}(τ+)$, and the results will be the impact update rules. 

With the symbolic solutions for impact update, we can numerically evaluate them and define a 	function for the impact update in the simulation loop. 

## What happened in simulation 
When the simulation begins, the jack falls because of gravity. Since I give a force in the positive y direction and a rotation force to the box, the box will not fall, and the box will start rotating slowly. When the jack hits the sides of the box, it will bounce back, and the direction depends on the angle of the sides it collides. The simulation looks reasonable to me, the jack will bounce back, and the direction is related to the angle of the sides it collides. Also, since I chose the box mass to be way heavier than the jack, the collision will not affect the performance of the box. The blow picture shows the change of 6 related to time. It can further prove mt work is reasonable, from y2, we can clearly see where collision happened. Theta2 first increasing and then decreasing is also due to the collision.

![Screenshot from 2023-12-07 22-47-19](https://github.com/JihaiZhao/Jack-In-a-Box/assets/99274626/3658e8b5-d31d-4818-8c3e-43047d202fdd)
