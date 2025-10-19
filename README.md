# LiDAR Map Building from Bipedal Robot Data

This mini-project demonstrates how to process 3D LiDAR scans from a walking robot 
and transform them into a consistent global map using matrix algebra and affine transformations.

## Overview
Using CSV LiDAR scans containing (x, y, z, intensity) data and corresponding transformation matrices, 
the notebook reconstructs the robot's surrounding environment into a world coordinate frame.

## Tools
- Julia
- Jupyter Notebook
- CSV LiDAR dataset

## Results
- Applied 2D and 3D linear transformations.
- Visualized aligned point clouds forming a coherent map.

## Author
Omar Geasa

## Acknowledgments
This project is inspired by and based on materials from the
[ROB 101: Computational Linear Algebra](https://github.com/michiganrobotics/rob101/tree/main)
course at the University of Michigan.  
## Acknowledgement & Licensing

This project is based on materials from the **ROB 101: Computational Linear Algebra** course at the University of Michigan.

The original course content, including the project description and dataset, is licensed under the [Creative Commons Attribution-NonCommercial 4.0 International License (CC BY-NC 4.0)](http://creativecommons.org/licenses/by-nc/4.0/). Original concept and dataset by Prof. Jessy Grizzle, Prof. Maani Ghaffari, and Tribhi Kathuria.

The code in this repository, as a derivative work of the course's original source code, is licensed under the **GNU General Public License v3.0**. The full license text is available in the `LICENSE` file.

The implementation, code modifications, and documentation presented here are my own.
