# Drag-Of-A-Bullet-In-OpenFOAM
This is a very interesting example of a compressible flow simulation in OpenFOAM which we, sadly, might need more often in the future. The cylindrical object subjected to supersonic compressible flow is a 5.56mm bullet. The nominal caliber is 5.69mm, the total computational space is 10 calibers in every direction. The dimensions of the object are not super-accurate (there is a very shallow 'groove' which probably holds the very end of the shell, which is very likely not accurate), it's just about finding a workflow with which we can determine the drag coefficient of such a bullet easily from the data provided by OpenFOAM. 
The model for the simulation is derived from a sketch (some details are left to our imagination) found in "Aerodynamic and Flight Dynamic Characteristics of 5.56-mm Ammunition: M855" by Sidra I. Silton and Bradley E. Howell, ARL report nr. ARL-TR-5182 from May 2010. 

The setup for the simulation is quite basic, for the single object we just need the bullet.stl, the 'box' sourrounding it is generated with blockMesh, the final mesh is generated with snappyHExMesh.
The simulation is pressure-driven, the air is modeled as ideal gas, temperature is 298K. As the inlet pressure drives our flow, we need to do some calibration to arrive at the velocity numbers we want. The initial velocity is set over the whole internal field, the inlet pressure is adapted in such a way it (approximately) keeps the set speed constant over the whole simulation time. As we use rhoCentralFoam as our solver, the simulation is transient and we can follow the flow buildup at the object. The duration of the sim is up to the user, basically we wait until our flow quantities don't change any longer with time. 



