**University of Pennsylvania, CIS 565: GPU Programming and Architecture,
Project 1 - Flocking**

* Angelina Risi
  * [LinkedIn](www.linkedin.com/in/angelina-risi)
* Tested on: Windows 10, i7-6700HQ @ 2.60GHz 8GB, GTX 960M 4096MB (Personal Laptop)

**Images:**  
Uniform Coherent Grid Under Default Conditions:
  ![Early in Simulation](/images/CoherentGridSim1.PNG)
  ![Late in Simulation](/images/CoherentGridSim3.PNG)
  
Animation at 15000 Boids (for increased FPS)
  ![Animated Coherent Grid](/images/uniformcoherentgrid.gif)

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
* After decreasing the cell width to max of rule distances, the program crashed with illegal memory access @ 5000 objects (triggered at buffer reset?), and crashed with a runtime error at higher values, but 4000 ojects works fine and is used for performance analysis
    
**Performance Analysis:**  
  
Performance was tested on all three solutions by taking the average FPS during the runtime, which was timed for approximately 1 minute. The default standard for comparison is the number of boids at 5000, block size of 128, and cell width of twice the search radius. Visualization was disabled to get a more accurate measure of performance, especially when a large number of particles would need rendering.  
  
* Changing # of Boids:  
  Increasing the number of boids has the overall trend of decreasing performance. This is especially true for the naive implementation as the processing time is proportional to N^2, due to each boid checking all other boids. However, the grid-based searches have abnormalities in their performance, dipping at low boid counts, then increasing and following a less sharp decrease in performance. Due to errors described prior I had difficulties getting performance data for certain ranges of boid counts.
  ![FPS Graph w/ Change in Boid Count](/images/defaultFPS.PNG)

* Changing Block Size:  
  Changing the block size from the default 128, to 256 and 1024, changes the number of threads sharing the same block, which affects such things as memory sharing. Surprisingly, there is a slight decrease in performance seen in testing as the block size increases, but why this occurs is uncertain.
  ![FPS at Block Size 256](/images/block256FPS.PNG)
  ![FPS at Block Size 1024](/images/block1024FPS.PNG)

* Changing Cell Width:  
  At smaller boid counts, we can see a decrease in performance when the cell width decreases due to more overhead in checking more cells. I was unable to test at higher boid counts due to crashes I was unable to resolve. The performance was tested at both the default of cell width = twice the search radius, and cell width = search radius.
  ![FPS Graph w/ Change in CellWidth](/images/cellWidthFPS.PNG)  
    
  The coherent grid proved to perform significantly superior to the scattered grid. While both greatly improve on the Naive implementation at high boid counts, at the highest numbers tested the coherent grid had twice the FPS of the scattered grid. Low boid counts showed better performance for the naive solution, possibly due to the overhead of the extra sorting, buffers, and array accesses outweighing the gain of reduced looping through boids. This does not explain why the FPS at 5000 boids is so low compared to FPS at much higher counts for the grid solutions, though.

  
