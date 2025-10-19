# Robot Mapping with LiDAR Data

**By Omar Ahmed Geasa**
*Mechatronics Engineering, Tanta University*
*Omargeasaa@gmail.com*

---

## Abstract

This mini-project demonstrates how to manipulate raw LiDAR data using point clouds to construct a visual map of the environment. The work focuses on applying affine transformations from linear algebra to convert data from the robot’s local coordinate frame into a world reference frame. This project shows how affine transformations enable accurate alignment of multiple LiDAR scans for real-time robotic mapping. The dataset used in this work was collected by the Cassie bipedal robot at the University of Michigan.

---

## I - Introduction

Many autonomous systems rely on perception sensors such as LiDAR to localize, map, and navigate in unknown environments. LiDAR systems generate dense point clouds that capture the 3D structure of the environment with high precision. However, LiDAR collects data in its own local coordinate frame, which moves with the robot. As the robot moves, this causes misalignment among successive scans. Therefore, it is necessary to transform all data into a common world coordinate frame to construct a coherent map. This is achieved by computing the affine transformation between the local and global frames as the robot moves.

This project demonstrates how local sensor data can be transformed into a unified global map by processing large-scale data using matrix operations, specifically expressing translation and rotation in homogeneous coordinates.

[Image of a 3D LiDAR point cloud map of an outdoor environment]

---

## II - Background

### Affine Transformation
An **affine transformation** is a geometric mapping that preserves straight lines and parallelism between points. It combines linear operations like rotation and scaling with translation into a single framework. By using homogeneous coordinates, a complex sequence of operations can be expressed as a single matrix multiplication.

$f(x) = y = Ax + t$

### Homogeneous Coordinates
While rotation and scaling are linear, translation is not. **Homogeneous coordinates** are a clever trick to express all affine transformations, including translation, within a single matrix multiplication. A 2D point $[x,y]^T$ becomes $[x,y,1]^T$, and a 3D point $[x,y,z]^T$ becomes $[x,y,z,1]^T$. This allows the transformation to be written as:

$$
\begin{bmatrix}
x' \\
y' \\
z' \\
1
\end{bmatrix}
=
\begin{bmatrix}
A & t \\
0 & 1
\end{bmatrix}
\begin{bmatrix
