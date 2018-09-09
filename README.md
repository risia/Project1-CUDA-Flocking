**University of Pennsylvania, CIS 565: GPU Programming and Architecture,
Project 1 - Flocking**

* Angelina Risi
  * [LinkedIn](www.linkedin.com/in/angelina-risi)
* Tested on: Windows 10, i7-6700HQ @ 2.60GHz 8GB, GTX 960M 4096MB (Personal Laptop)

### (TODO: Your README)

**Features**
* Naive flocking iterating over all other boids to adjust velocity
* Grid-based search for influencing boids
  - Region is divided into cells and before search the boids are sorted by containing cell using Thrust
  - Modified search to dynamically change cell search bounds based on search radius
    * Search not limited to fixed number of cells when rule distances, grid resolution, etc modified
* Coherent grid-based search
  - In addition to sorting boids by cell, their position and velocity data are sorted into this order as well
    * Allows more direct access to data, potentially improving performance

Include screenshots, analysis, etc. (Remember, this is public, so don't put
anything here that you don't want to share with the world.)
