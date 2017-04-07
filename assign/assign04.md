---
layout: default
title: "Assignment 4: N-Body Simulation (GPU)"
---

Due: **Wednesday, April 24th** by 11:59 PM

# Getting Started

Download [CS365\_Assign04.zip](CS365_Assign04.zip). Unzip it on the cluster head node (or your local machine, if you are running Linux and have CUDA and `libui` installed).

If you are developing on the cluster, you will need to logged onto `bambleweeny`:

    ssh -X bambleweeny

To compile the sequential and parallel versions of the programs, run the command

    make

To run the programs:

    ./nbody_seq

    ./nbody_par

Note that the `nbody_par` program can take command line arguments:

* The `-p` argument specifies the number of bodies ("particles") to create
* The `-f` argument specifies the number of animation frames to display

For example, to run with 2000 bodies, displaying 200 animation frames, the command would be

    ./nbody_par -p 2000 -f 200

If you want to compile the program using Linux on your own computer, you will to build [libui](https://github.com/andlabs/libui).  You will also need to have CUDA installed.

# Your task

Your task is similar to the one in [Assignment 3](assign03.html): you will parallelize an N-Body simulation.

The difference is that in this version, you will use [CUDA](https://developer.nvidia.com/cuda-zone) to offload the computation onto the GPU.

You will make your code changes in `sim_par.cu`.

## Hints and specifications

Note: these hints are somewhat preliminary and will be updated soon.  However, this is not a complicated assignment, and should be easier than Assignment 3.  There is enough information here at this point that you should be able to get started.

Look at the **TODO** comments in `sim_par.cu`.  They indicate where you will need to add or modify code to execute the simulation on the GPU.

Note that the data representation has changed.  Rather than the main data structure being an array of `Particle` objects, the main data structure is now arrays called `x`, `y`, `dx`, `dy`, `mass`, and `color`.  A single particle is represented by the elements at a common index in these arrays.  See `particle.cu` and `sim_seq.cu` to see how the sequential computation works.

The `SimulationData` struct type is used to group pointers to all of the arrays into a single object.  The parallel computation (in `sim_par.cu`) will have *two* of these: one for the host (CPU), and one for the device (GPU).  Note that you must use `cudaMalloc` to allocate device buffers.

Your kernel function should take pointers to the 5 arrays `x`, `y`, `dx`, `dy`, and `mass`.  It will also need to know how many particles (bodies) are being simulated.  Your kernel function call should look something like the following:

```cuda
kernel<<<grid, THREADS_PER_BLOCK>>>(sim->pd_dev.x,
                                    sim->pd_dev.y,
                                    sim->pd_dev.dx,
                                    sim->pd_dev.dy,
                                    sim->pd_dev.mass,
                                    sim->num_particles);
```

Note that the functions `particle_dist`, `particle_force`, and `particle_compute_attraction` are defined to be available on *both* the CPU and GPU.  So, your kernel and device functions can call these functions as required to do the computation.

# Deliverables

Coming soon.

# Grading

Coming soon.

# Submitting

Run the command

    make submit

Type your Marmoset username and password when prompted.

<!-- vim:set wrap: -->
<!-- vim:set linebreak: -->
<!-- vim:set nolist: -->
