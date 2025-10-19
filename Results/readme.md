# Robot Mapping with LiDAR Data

**By Omar Ahmed Geasa**  
*Mechatronics Engineering, Tanta University*  
*Omargeasaa@gmail.com*

---

## Abstract

This project demonstrates how to manipulate raw LiDAR data using point clouds to construct a visual map of an environment. The work focuses on applying **affine transformations** from linear algebra to convert data from the robot’s local coordinate frame into a global world frame. This process enables accurate alignment of multiple LiDAR scans — a fundamental task in robotic mapping.  

The dataset was collected by the Cassie bipedal robot at the University of Michigan.

---

## Introduction

Autonomous systems such as self-driving cars and mobile robots rely heavily on sensors like LiDAR to perceive, map, and navigate their surroundings. LiDAR systems generate dense **point clouds** — collections of 3D points that capture the environment's structure with high precision.

A key challenge is that LiDAR collects data in its own **local coordinate frame**, which moves along with the robot. As the robot moves, successive scans become misaligned. To build a coherent map, all these scans must be transformed into a single, fixed **global coordinate frame**.  

This is accomplished using an **affine transformation**, which accounts for the robot’s motion between scans.

---

## Background

### Affine Transformation

An **affine transformation** is a geometric operation that preserves lines and parallelism. It combines linear operations such as rotation and scaling with translation into a single mathematical framework.  

The general form is:

$$
f(x) = y = A x + t
$$

### Homogeneous Coordinates

While rotation and scaling can be represented by matrix multiplication, translation requires addition.  
**Homogeneous coordinates** are a mathematical trick that allows all affine transformations — including translation — to be represented by a single matrix multiplication.  

A 3D point $(x, y, z)$ becomes $(x, y, z, 1)$, allowing the transformation to be written as:

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
\begin{bmatrix}
x \\
y \\
z \\
1
\end{bmatrix}
$$

### Application in Robotics 🤖

In robotics, this is essential for relating sensor frames to a world frame.  
For LiDAR mapping, the **homogeneous transformation matrix** takes this form:

$$
T =
\begin{bmatrix}
R_{3\times3} & t_{3\times1} \\
0_{1\times3} & 1
\end{bmatrix}
$$

Where:  
- $R_{3\times3}$: rotation matrix  
- $t_{3\times1}$: translation vector  
- $0_{1\times3}$: row vector of zeros  
- $1$: homogeneous coordinate

---

## Methodology

### Step 1: 2D Transformations

The project begins by illustrating 2D affine transformations on a simple square to build intuition.

#### Translation

Each point of the square is shifted by $-0.5$ units along both axes.

$$
\begin{bmatrix}
x' \\
y'
\end{bmatrix}
=
\begin{bmatrix}
x \\
y
\end{bmatrix}
+
\begin{bmatrix}
-0.5 \\
-0.5
\end{bmatrix}
$$

#### Rotation

The square is rotated counterclockwise by 45° ($\pi / 4$ radians).

$$
\begin{bmatrix}
x' \\
y' \\
1
\end{bmatrix}
=
\begin{bmatrix}
\cos\theta & -\sin\theta & 0 \\
\sin\theta & \cos\theta & 0 \\
0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
1
\end{bmatrix}
$$

#### Scaling

The square is stretched horizontally ($S_x = 1.5$) and compressed vertically ($S_y = 0.5$).

$$
\begin{bmatrix}
x' \\
y' \\
1
\end{bmatrix}
=
\begin{bmatrix}
S_x & 0 & 0 \\
0 & S_y & 0 \\
0 & 0 & 1
\end{bmatrix}
\begin{bmatrix}
x \\
y \\
1
\end{bmatrix}
$$

---

### Step 2: 2D Point Cloud Correction

An affine transformation is then used to correct a distorted 2D point cloud image:

$$
T =
\begin{bmatrix}
-0.09239 & 0.038268 & 300 \\
-0.38268 & -0.923879 & 165 \\
0 & 0 & 1
\end{bmatrix}
$$

---

### Step 3: 3D LiDAR Mapping

Finally, these concepts are applied to 3D LiDAR data from the Cassie robot.  
Each scan is transformed from its local sensor frame to the fixed world frame using its unique transformation matrix, constructed from the provided rotation ($R$) and translation ($t$) vectors.  

By iterating over time, each transformed point cloud is appended to a global map, fusing the scans into a single, coherent 3D representation.

---

## Results & Discussion

The transformations successfully corrected the misalignment in the raw LiDAR scans. Initially, the data appeared as a jumbled mass of overlapping points. After applying the correct homogeneous transformation for each frame, the point clouds aligned accurately, forming LiDAR rings centered around the robot’s true position.  

Fusing the scans over five seconds produced a dense, coherent 3D map of the environment.

### Assumptions and Limitations

This project assumes that the robot’s true position and orientation (which provide $R$ and $t$) are known and accurate.  
In real-world applications, these values must be estimated using algorithms such as **SLAM** (Simultaneous Localization and Mapping).  

Thus, this work focuses only on the **mapping** stage, assuming the **localization** problem is already solved.

---

## Conclusion

This project successfully demonstrated how affine and homogeneous transformations can be applied to real LiDAR data to reconstruct a 3D map.  
The results confirm that properly designed transformation matrices form the mathematical backbone of consistent and accurate robotic mapping.

---

## Acknowledgement

This project is based on materials from the **ROB 101: Computational Linear Algebra** course at the **University of Michigan**.  
The original concept and dataset were provided by *Prof. Jessy Grizzle*, *Prof. Maani Ghaffari*, and *T.ribhi Kathuria*.  
The implementation and documentation are my own.
