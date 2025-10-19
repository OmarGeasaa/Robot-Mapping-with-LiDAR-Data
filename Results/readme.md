# Robot Mapping with LiDAR Data

**By Omar Ahmed Geasa** *Mechatronics Engineering, Tanta University* *Omargeasaa@gmail.com*

---

## Abstract

This mini-project demonstrates how to manipulate raw LiDAR data using point clouds to construct a visual map of the environment. The work focuses on applying affine transformations concepts from linear algebra to convert data from the robot’s local coordinate frame into a world reference frame. This project shows how affine transformations enable accurate alignment of multiple LiDAR scans for real-time robotic mapping. The dataset used in this work was collected by the Cassie bipedal robot at the University of Michigan.

---

## I - Introduction

Many autonomous systems rely on perception sensors such as stereo cameras and LiDAR to localize, map, and navigate in unknown environments. By using these sensors, the robot can construct a spatial representation of its surroundings and how it interacts with them. LiDAR systems generate dense point clouds that capture the 3D structure of the environment with high precision which allows us to visualize how the robot operates within its surroundings.

However, LiDAR collects data in its own local coordinate frame, which does not always align with the robot’s reference frame. As the robot moves, both the LiDAR and robot coordinate frames change with respect to the environment, causing misalignment among successive scans. Therefore, it is necessary to transform all robot data into a common world or global coordinate frame in order to construct the map. This can be achieved by computing the affine transformation between the local and global coordinates as the robot moves through the environment.

This project focuses on implementing affine transformations using linear algebra to transform LiDAR point cloud data. The project demonstrates how local sensor data can be transformed into a unified global map representation suitable for robotic mapping applications in addition to how large-scale data can be efficiently processed using matrix operations. This is achieved by using real dataset expressing translation, and rotation operations in homogeneous coordinates.

![Figure 1: Motivation: Expected project output showing LiDAR-based 3D mapping of the robot’s environment.](path/to/your/figure1.png)

---

## II - Background

### i - Affine Transformation
An affine transformation is a geometric mapping that preserves straight lines and parallelism between points, meaning that sets of parallel lines remain parallel after the transformation. It combines multiple linear operations, such as translation, rotation, scaling, and sharing, into a unified mathematical framework. By representing these transformations through homogeneous coordinates, complex sequences of operations can be expressed as a single matrix multiplication.

$f(x) = y = Ax + t$

### ii - Homogeneous Coordinates
While affine transformations describe how points are mapped between coordinate frames, the translation component cannot be represented purely as a matrix multiplication in standard Cartesian coordinates. Homogeneous coordinates are introduced to express all transformation operations in a single matrix. In homogeneous coordinates, a 2D point $[x,y]^T$ becomes $[x,y,1]^T$, and a 3D point $[x,y,z]^T$ becomes $[x,y,z,1]^T$. This allows the affine transformation to be written as a single matrix–vector multiplication:

$$
\begin{bmatrix} x' \\ y' \\ z' \\ 1 \end{bmatrix} = \begin{bmatrix} A & t \\ 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ z \\ 1 \end{bmatrix}
$$

Here, $A$ represents the linear part of the transformation (rotation, scaling, or shear), and $t$ is the translation vector. The resulting matrix is known as the homogeneous transformation matrix.

### iii - Application in Robotics
In robotics, homogeneous transformation matrices are essential for describing how sensors and robot components move relative to one another. For example, in LiDAR-based mapping, a transformation matrix is of the form:

$$
T = \begin{bmatrix} R_{3 \times 3} & t_{3 \times 1} \\ 0_{1 \times 3} & 1 \end{bmatrix}
$$

Where:
* $R_{3 \times 3}$ is a 3x3 rotation matrix
* $t_{3 \times 1}$ is a 3x1 translation vector
* $0_{1 \times 3}$ is a 1x3 row vector of zeros
* $1$ is the number one

This matrix is applied to LiDAR data to transform scans from the sensor’s local frame into the global world frame, ensuring that consecutive scans are properly aligned.

---

## III - Methodology

This section outlines the methodology used to transform LiDAR data from the robotautonomously estimate the robot’s position and orientation in real time[cite: 337].

## [cite_start]Acknowledgement [cite: 338]
[cite_start]This project is based on materials from the ROB 101: Computational Linear Algebra course at the University of Michigan[cite: 339]. [cite_start]Original concept and dataset by Prof. Jessy Grizzle, Prof. Maani Ghaffari, and T.ribhi Kathuria[cite: 340]. [cite_start]The Implementation, code, and documentation here are my own[cite: 341].
