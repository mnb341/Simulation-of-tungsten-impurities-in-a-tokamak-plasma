# Simulation-of-tungsten-impurities-in-a-tokamak-plasma
This is a program that simulates tungsten impurities at a tokamak plasma edge. It can be modified to simulate other elements and particles, as well as any 2D electric field with a constant magnetic field in the z direction. Further modifications would allow for full 3D electromagnetic fields and particle movements.

This program is derived from the work of Mikael K. T. Jensen & Nicolai L. Jensen. If you use the program please cite "Impact of Heavy Ions on Edge Energy Loss in Fusion Plasmas" by Mikael Kim Tvermark Jensen and Nicolai Lomstein Jensen, 2025, DTU Department of Physics. You can find the entry in the DTU database for the bachelor theis at: https://findit.dtu.dk/en/catalog/68783f6865ea2c01029a58e1. 

The repository contains 3 programs. The field generator, that loads plasma field data and sets up interpolators; the animation function, that takes partciles data and electron density field as input to prduce an animation of the particles' motion; and the simulation program itself, that also contain a test run, that can simulate the particles' motion on field data of much smaller size. The test simulation uses much less memory during execution and serves to show how to program works. The simulation in the test itself is identical. It is only the background fields that have a much smaller resolution.

The initial conditions of the simulated particles are loaded from a file, created by the field generator. The file contains an array of shape [N,5], where N is the number of particles simulated, and 5 is number of variables needed to describe the particles motion, time, x position, y position, x velocity, and y velocity. The background field are provided as a meshgrid and interpolated for the program. This allows the simulation program to simply call the interpolator at some coordintaes to find the value of the relevant field. It is important that the interpolators are set up, such that query points take the form [t, x, y].

The ionisation and recombination rate coefficients, are specific to Tungsten. This means that several variables in the program, as well as the ionisation and recombination rate interpolators, would have to be changed in order to simulate other elements or particles. The ionisation rate and recombination rate coefficients are set up in dictionaries. The nth element in the dictionary is the interpolant for the nth ionisation level, meaning that if you need to calculate the ionisation and recombination rate coefficients for a tungsten ion in the +3 state, then nu_I[3] is the ionisation rate coefficient interpolator and nu_R[3] is the racombination rate coefficient interpolator. The interpolators are set up to take query points of the form [T_e,n_e], where T_e is the temperature of the electrons in the background plasma and n_e is the electron density of the background plasma.

The output data comes in two forms. The particle class itself, and the "collect" data function. The particle save a wide variety of simulation parameters, as well as the raw data from the nummerical integration. However it is not saved to a file in the program. If you want, you will have to pickle it yourselves. The "collect" function interpolates the particle data on a evenly space time array, simplifying the data analysis, and ensuring the data is uniform across the timespan of the simulation. The function itself output a dictionary containing several fields. It saves the simulation data for the particles, namely the number of timesteps for each partcile simulation, the time and the variable values throughout the simulation. Each particles simulation is different however, and has a different number of timesteps. Therefore it also produces an array of the form [t,ij,n], for the variables position, velocity, acceleration, and charge. Here t is time, ij indicates the coordinate of the variable such as the x (ij=0) or y (ij=1) coordinate of velocity, and n is the particle number. These are interpolated on an evenly space time array, t_eval. This allows for matrix operations to be used during data analysis.

Python setup 
- ensure python is installed on your computer
- open the terminal
- with the command "pip install [name]" or "pip3 install [name]", install the following libraries: h5py; numpy; scipy; matplotlib; ffmpeg
- Download Simulation.zip from the repository
- unpack the .zip file

Run "Plasma_Simulation.py" on test field-data. Uses about .6 GB of memory.
- setup python as described above
- change current directory in terminal to the simulation-folder location
- in the "Plasma_Simulation.py" script line 11, set test=True
- run "Plasma_Simulation.py" in the terminal with the command "python Plasma_Simulation.py"

Run "Field_Generator.py".
- copy "nHESEL_data"-folder into the simulation-folder
- make sure files are called "phi__nHESEL_run.h5", "T_e__nHESEL_run.h5", & "n_e__nHESEL_run.h5"
- setup the variable for the initial conditions
- run "Field_Generator.py" as python program script

Run "Plasma_Simulation.py" with generated field-data.
- follow instructions for running "Field_Generator.py"
- in the "Plasma_Simulation.py" script line 11, set test=False
- run "Plasma_Simulation.py" in the terminal with the command "python Plasma_Simulation.py"
