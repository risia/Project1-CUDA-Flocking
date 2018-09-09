**University of Pennsylvania, CIS 565: GPU Programming and Architecture,
Project 1 - Flocking**

* Angelina Risi
  * [LinkedIn](www.linkedin.com/in/angelina-risi)
* Tested on: Windows 10, i7-6700HQ @ 2.60GHz 8GB, GTX 960M 4096MB (Personal Laptop)

### (TODO: Your README)

**Features:**
* Naive flocking iterating over all other boids to adjust velocity
* Grid-based search for influencing boids
  - Region is divided into cells and before search the boids are sorted by containing cell using Thrust
  - Modified search to dynamically change cell search bounds based on search radius
    * Search not limited to fixed number of cells when rule distances, grid resolution, etc modified
* Coherent grid-based search
  - In addition to sorting boids by cell, their position and velocity data are sorted into this order as well
    * Allows more direct access to data, potentially improving performance

**Known Issues:**
* Boid counts in the range of N = 5153 through N = 10112 cause CUDA launch failure or Thrust bad_alloc errors
  - Cause unknown, as boid counts above and below this range behave correctly
  - May be some sort of setup issue, needs testing on another machine but code appears correct
    * Tried changing architecture to sm_50 (which should be supported by graphics card) and running CMake, but CUDA launch failure
  - Does NOT occur in Naive simulation, probably since it doesn't use Thrust
    
**Images:**
  Uniform Coherent Grid Under Default Conditions:
  ![Early in Simulation](/images/CoherentGridSim1.PNG)
  ![Late in Simulation](/images/CoherentGridSim3.PNG)
  
  Animation at 15000 Boids (for increased FPS)
  ![Animated Coherent Grid](/images/uniformcoherentgrid.gif)
  
Include screenshots, analysis, etc. (Remember, this is public, so don't put
anything here that you don't want to share with the world.)
