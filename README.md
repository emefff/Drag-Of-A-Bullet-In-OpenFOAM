# Drag-Of-A-Bullet-In-OpenFOAM
This is a very interesting example of a compressible flow simulation in OpenFOAM which we, sadly, might need more often in the future. The cylindrical object subjected to supersonic compressible flow is a 5.56mm bullet. The nominal caliber is 5.69mm, the total computational space is 10 calibers in every direction. The dimensions of the object are not super-accurate (there is a very shallow 'groove' which probably holds the very end of the shell, which is very likely not accurate), it's just about finding a workflow with which we can determine the drag coefficient of such a bullet easily from the data provided by OpenFOAM. 
The model for the simulation is derived from a sketch (some details are left to our imagination) found in "Aerodynamic and Flight Dynamic Characteristics of 5.56-mm Ammunition: M855" by Sidra I. Silton and Bradley E. Howell, ARL report nr. ARL-TR-5182 from May 2010. 


