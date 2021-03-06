Read me for
RTA Transfer Matrix Calculator v1
for MatLab

Version: 1.0
Updated: November 6, 2018

By: Aaron H. Rose
Physics PhD candidate, Boston College, Chestnut Hill, MA
Grad Intern, Chemistry and Nanoscience Center, National Renewable Energy Laboratory, Golden, CO
rose3fa@gmail.com

Written in MatLab R2018b for Mac, OSX 10.11.5
Verified working on Windows 7 PC with no modification


Disclaimer
I am not a programmer and not a MatLab expert, so please forgive me if my code is hard to look at. So far, it does look like it correctly does what it is intended to do. 


Reference
The scripts here are based upon the script jreftran_rt.m, written by: 
Shawn Divitt, ETH Zurich, Photonics Group, which in turn is based upon the following paper written by K. Pascoe:
"Reflectivity and Transmissivity through Layered, Lossy Media: A User-Friendly Approach," 2001. 
See the following link: http://www.dtic.mil/dtic/tr/fulltext/u2/a389099.pdf
The paper is also included in this directory

The rest of the scripts were written by me. 


Directory contents
The directory is called ‘RTA Transfer Matrix Calculator v1’
The scripts are in the top level of this directory
The ‘Output’ folder is where all output files are directed, within their own subfolders
The ‘Refractive Indices’ folder is where the refractive indices are stored, to be called upon by thinfilmRTA.m
	Please see the Read Me within this folder to learn about how to format and add new indices


Overview of scripts
The jreftran_rt script is the work horse that actually does the transfer matrix calculation, for single wavelength and angle of incidence.
thinfilmRTA extends jreftran_rt to a create a wavelength dependent spectrum.
The other scripts compute various quantities or plot those various quantities. E.g. R, T, or A as a function of input angle or dispersion plots based on A.
The documentation is best for thinfilmRTA and bilayerDispersionStudy, so if you have questions about what variables refer to or what units are used, see those functions. 


Description of Scripts (bolded script is best place to start)
I. jreftran_rt.m
[r,t,R,T,A]=jreftran_rt(l,d,n,t0,polarization) 
The script jreftran_rt.m output transmission and reflection coefficients, as well reflectance, transmittance, and absorptivity for a given thin film stack, at a single wavelength and polarization. 
For scripts that have ‘function […] = name.m(…), these are functions that can’t be run alone. See thinfilmRTA.m for instructions on how to call function.

II. thinfilmRTA.m
[wl, n_substrate, R, T, A, nData, nDataInterp, kData, kDataInterp, N] = thinfilmRTA(lam0, lam1, dlam, layers, thicknesses, angle, polarization)
This script uses jreftran_rt.m to calculate R, T, and A as a function of wavelength. As such, it takes a range of wavelengths rather than one value. Also, the layers are called by name, not by refractive index, and the function makes a call to a database of wavelength-dependent real and imaginary refractive index files. Thus, any material needed for the calculation needs to have separate n and k files in the Refractive Indices folder in this directory. See the ‘read me’ within that folder for more information. The refractive index data can be at arbitrary wavelength, as it’s interpolated/extrapolated for use in the script. 
     One other note: I intentionally made the commenting in this script very thorough (I hope) to help people who are new to MatLab. 

III. bilayerDispersionStudy
[] =  bilayerDispersionStudy(lam0, lam1, dlam, date, suffix, layers, film1thicknesses, film2thicknesses, angle, polarization, single, control)
This script works for thin film stacks with 2 films sandwiched between the outer 2 semi-infinite media. Dispersion plots can be created for each stack. Option to also plot a control stack on the same figure. The control stack is just the average of each of the separate stacks only containing one of the two films. Plot dispersion with kx or angle on the x-axis. Sweep over arbitrary thicknesses of each film. Should be possible to sweep over materials too. 

IV. aveOfFilms
[Ac, Aavg, n_substrate]=aveOfFilms(lam0, lam1, dlam, layers, thicknesses, angle, polarization, wl)
Calculates absorptivity for arbitrary thin film stack as well as the “control” film absorptivity (see description in bilayerDispersionStudy). 

V. plotDispersion
[] = plotDispersion(angle, wl, A, n_substrate, layers, thicknesses, polarization, directory, dispersionX)
Plots the dispersion when passed absorptivity data

VI. plot2dispersions
[] = plot2dispersions(angle, wl, A, Aavg, n_substrate, layers, thicknesses, polarization, directory, dispersionX)
Plots the dispersion and control when passed absorptivity data for both

VII. example
Contains examples of calls to functions. Probably the most useful script. Just copy and paste into new script and delete sections not needed to build your own. 

VIII. AvsAngle/AbsVsAngle/RvsAngle/TvsAngle
[A, n_substrate]=AvsAngle(lam0, lam1, dlam, layers, thicknesses, angle, polarization, wl)
[Abs, n_substrate]=AbsVsAngle(lam0, lam1, dlam, layers, thicknesses, angle, polarization, wl)
[R]=RvsAngle(lam0, lam1, dlam, layers, thicknesses, angle, polarization, wl)
[T, n_substrate]=TvsAngle(lam0, lam1, dlam, layers, thicknesses, angle, polarization, wl)
Calculates and outputs plots of absorptivity/absorbance/reflectivity/transmissivity vs angle for arbitrary stacks

IX. plotAvsAngle/plotAbsVsAngle/plotRvsAngle/plotTvsAngle
[] = plotAvsAngle(wl, layers, lam0, lam1, angle,thicknesses, A, polarization, directory), same format for others
Called by AvsAngle/AbsVsAngle… to create the plot

X. plotFittedIndices
[] = plotFittedIndices(wl, nData, nDataInterp, kData, kDataInterp, N, layers, lam0, lam1, directory)
Plots the interpolated/extrapolated dielectric functions that are used to calculate R, T, A along with data from dielectric input file. Useful if you need to check the fitting. 

XI. plotRTA
[] = plotRTA(wl, layers, lam0, lam1, angle, thicknesses, R, T, A, polarization, directory)
Plots R, T, A on the same plot for a given input angle. 


To Do
jreftran and thinfilmRTA calculate R, T, and A. To speed things up, each value could be separated into its own script and called upon when needed. 	Could also keep them as one script, but add conditional statements to control what gets calculated. 
Make bilayerDispersionStudy work for arbitrary number of layers. 
In bilayerDispersionStudy, add in sweeping over material types 
Add in scripts to import and plot dispersion from external data
Add in code to output text file of computed data—this is very easy, one line of code. 
Add in ability to sweep R, T, A, Abs as function of thickness. I have a messy version of this not included. 
The R,T,A vs angle scripts could be consolidated into one script. Would want it to calculate each value only when asked for speed. The plot script would have a bunch of conditionals to set the labelling. Would also be nice if there was a conditional that allowed a single angle to be plotted without displaying the legend and instead putting the angle in the title.






