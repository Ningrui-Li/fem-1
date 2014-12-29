Non-plaid mesh
==============

This change was a little more involved (no MEX file unfortunately, just
straight Matlab). Basically, there are 2 kinds of interpolant classes in
matlab, griddedInterpolant and scatteredInterpolant. The gridded version is the
one that interpn() uses and requires the matrix to be plaid, etc. The scattered
version is the one that griddata() uses, which I think is confusingly named
since you don’t need a grid. What’s nice with the scatteredInterpolant is that
you just pass in XYZ coordinates and the value at each coordinate, and it
figures out the interpolant from there. So basically, in the read_dot_dyn
function, you can remove all that meshgrid stuff that happens after the
textscan, and just pipe the 4 vectors (X,Y,Z, Val) directly into the
interpolation function. I chose to implement this approach using the
scatteredInterpolant() rather than griddata(), but either could work.

  

Outside for-loop
================
```
%Predefine interpolants for X Y and Z displacement data

Fx = scatteredInterpolant(nodes(:,2),nodes(:,3),nodes(:,4),zeros(size(nodes,1),1));
Fy = scatteredInterpolant(nodes(:,2),nodes(:,3),nodes(:,4),zeros(size(nodes,1),1));
Fz = scatteredInterpolant(nodes(:,2),nodes(:,3),nodes(:,4),zeros(size(nodes,1),1));
```


Inside for-loop
===============
```
%Update the X Y and Z displacement values in the interpolants from disp.dat slice

Fx.Values = zdisp_slice(:,2);
Fy.Values = zdisp_slice(:,3); 
Fz.Values = zdisp_slice(:,4);
                                                                                               %Perform interpolation of arbitrary scatterer point 
                                                                                               sdX = Fx(scatterers(:,1), scatterers(:,2), scatterers(:,3)); 
sdY = Fy(scatterers(:,1), scatterers(:,2), scatterers(:,3));
sdZ = Fz(scatterers(:,1), scatterers(:,2), scatterers(:,3));
                                                                                               % Add displacements to initial scatterer positions
% and insert the values in the phantom structure

phantom.position=(scatterers+[sdX sdY sdZ])*FD_RATIO;
```
