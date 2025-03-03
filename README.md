# Drag-Of-A-Bullet-In-OpenFOAM
This is a very interesting example of a compressible, inviscid flow simulation in OpenFOAM which we, sadly, might need more often in the future. The cylindrical object subjected to supersonic compressible flow is a 5.56mm (nominal caliber) bullet. The maximum diameter is is 5.69mm, the total computational domain is 10 calibers in every direction. The dimensions of the object are not super-accurate (there is a very shallow 'groove' which probably holds the top rim of the shell, which is very likely not accurate but might create substantial drag), it's just about finding a workflow with which we can determine the drag coefficient of such a bullet easily from the data provided by OpenFOAM. 
The model for the simulation is derived from a sketch (some details are left to our imagination) found in "Aerodynamic and Flight Dynamic Characteristics of 5.56-mm Ammunition: M855" by Sidra I. Silton and Bradley E. Howell, ARL report nr. ARL-TR-5182 from May 2010. 

The setup for the simulation is quite basic, for the single object we just need the bullet.stl, the 'box' sourrounding it is generated with blockMesh, the final mesh is generated with snappyHExMesh and features 3 inflation layers.
The simulation is pressure-driven, the air is modeled as ideal gas, temperature is 298K. As the inlet pressure drives our flow, we need to do some calibration to arrive at the velocity numbers we want. The initial velocity is set over the whole internal field, the inlet pressure is adapted in such a way it (approximately) keeps the set speed constant over the whole simulation time. As we use rhoCentralFoam as our solver, the simulation is transient and we can follow the flow buildup at the object. The duration of the sim is up to the user, basically we wait until our flow quantities don't change any longer with time. In OpenFOAM, we can apply functions to calculate quantities in our data. Here, the force function comes in handy, we use it on our patch named 'bullet'. This force, together with air density, bullet cross section area, and velocity (squared) can be used to calculate Cd of the bullet at the given Mach number. When launching a new simulation we start with a low maxCo = 0.011 and give it a few timeSteps. Later we steadily increase maxCo to 0.5, if possible. The script featured can do that in an automated manner to keep simulation runs consistent.

The report "Conceptual Design Approach for Small-Caliber Aeroballistics With Application to 5.56-mm Ammunition" by Paul Weinacht, James F. Newill, and Paul J. Conroy, ARL report nr. ARL-TR-3620 contains a drag coefficient vs Mach number diagram on p. 3, with which we can compare our findings. Generally, these numbers would be used for subsequent calculation of trajectories with additional input parameters like windage, elevation angle, altitude, temperature, air density, amount of shell powder, rifling, .....

At 298K the speed of sound is approx. 347.23 m/s ( = sqrt(k R T)). For the velocity we take the value of Uy in the free flow just before the bullet, the force is calculated by OpenFOAM. For the density we take the value of the free flow before the bullet. 
Let's take a look at a typical velocity result with developed flow at a low Ma of 1.2:
![Uy](https://github.com/user-attachments/assets/df6f4d12-1b84-42b0-8a2a-bfba786cfe36)

Changing the flow velocity to Ma = 1.5 obviously will also change the angle of the Mach cone:
![Uy](https://github.com/user-attachments/assets/b8662e0c-d98c-48d3-b897-f5464dbe0091)

And finally, Ma = 2.0:
![Uy](https://github.com/user-attachments/assets/13e950cb-3307-42a1-9fcd-f1cd2237e9be)



| velocity / m/s | Ma / 1 | force / N | drag coefficient / 1 |
| ------------- | ------------- | ------------- | ------------- |
| 416.68  | 1.2  | 1.614715 | 0.5681 |
| 520.84 | 1.5 | 2.215589  | 0.4990  | 
| 694.46 | 2.0 | 3.150877  | 0.3991  |

Both videos shared apply to the first row in the table (v = 416.7 m/s). 

How are the calculated values holding up against measured data? Let's take a look at the mentioned drag coefficient vs. Mach number diagram of the same caliber bullet:

![Fit-of-drag-coefficient-for-the-M855](https://github.com/user-attachments/assets/71c09887-cc43-49e9-a14a-e33cfa78ed7b)

Clearly, our values are too high, only somehow within range. This can have a bunch of reasons, among these a too coarse mesh (only ~1.6M cells) and/or a to small simulation domain and also not fully developed flow. 
However, carefully executed, OpenFOAM can also manage this engineering job with more precision.

emefff@gmx.at








