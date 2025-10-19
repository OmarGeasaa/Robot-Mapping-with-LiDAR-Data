# Robot Mapping with LiDAR Data

**By Omar Ahmed Geasa**
*Mechatronics Engineering, Tanta University*
*Omargeasaa@gmail.com*
$\sqrt{3x-1}+(1+x)^2$
---
### **Step 1.A: Translation**

## Abstract

This mini-project demonstrates how to manipulate raw LiDAR data using point clouds to construct a visual map of the environment. The work focuses on applying affine transformations from linear algebra to convert data from the robot’s local coordinate frame into a world reference frame. This project shows how affine transformations enable accurate alignment of multiple LiDAR scans for real-time robotic mapping. The dataset used in this work was collected by the Cassie bipedal robot at the University of Michigan.

---

## I - Introduction

Many autonomous systems rely on perception sensors such as LiDAR to localize, map, and navigate in unknown environments. LiDAR systems generate dense point clouds that capture the 3D structure of the environment with high precision. However, LiDAR collects data in its own local coordinate frame, which moves with the robot. As the robot moves, this causes misalignment among successive scans. Therefore, it is necessary to transform all data into a common world coordinate frame to construct a coherent map. This is achieved by computing the affine transformation between the local and global frames as the robot moves.

This project demonstrates how local sensor data can be transformed into a unified global map by processing large-scale data using matrix operations, specifically expressing translation and rotation in homogeneous coordinates.



---

## II - Background

### Affine Transformation
An **affine transformation** is a geometric mapping that preserves straight lines and parallelism between points. It combines linear operations like rotation and scaling with translation into a single framework. By using homogeneous coordinates, a complex sequence of operations can be expressed as a single matrix multiplication.

$f(x) = y = Ax + t$

### Homogeneous Coordinates
While rotation and scaling are linear, translation is not. **Homogeneous coordinates** are a clever trick to express all affine transformations, including translation, within a single matrix multiplication. A 2D point $[x,y]^T$ becomes $[x,y,1]^T$, and a 3D point $[x,y,z]^T$ becomes $[x,y,z,1]^T$. This allows the transformation to be written as:

$\begin{bmatrix} x' \\ y' \\ z' \\ 1 \end{bmatrix} = \begin{bmatrix} A & t \\ 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ z \\ 1 \end{bmatrix}$


### Application in Robotics
In robotics, this is essential for relating sensor frames to a world frame. For LiDAR mapping, the **homogeneous transformation matrix** takes this form:

$$
T = \begin{bmatrix} R_{3 \times 3} & t_{3 \times 1} \\ 0_{1 \times 3} & 1 \end{bmatrix}
$$

Where:
* $R_{3 \times 3}$ is a 3x3 **rotation matrix**
* $t_{3 \times 1}$ is a 3x1 **translation vector**
* $0_{1 \times 3}$ is a 1x3 row vector of zeros
* $1$ is a scaling factor

---

## III - Methodology

### Step 1: 2D Transformations
The project starts by illustrating 2D affine transformations (translation, rotation, and scaling) on a simple square.

#### Translation
Each point of the square is shifted by -0.5 units along both axes to center it.
$\begin{bmatrix} x \\ y \end{bmatrix} = \begin{bmatrix} x \\ y \end{bmatrix} + \begin{bmatrix} t_x \\ t_y \end{bmatrix}$.

#### Rotation
The square is rotated counterclockwise by 45° ($\pi/4$ radians).
$$
\begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix} = \begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix}
$$

#### Scaling
The square is stretched horizontally ($S_x=1.5$) and compressed vertically ($S_y=0.5$).
$$
\begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix} = \begin{bmatrix} S_x & 0 & 0 \\ 0 & S_y & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix}
$$



### Step 2: 2D Point Cloud Correction
Next, an affine transformation is used to correct a distorted 2D point cloud image stored in `question_image.csv`.
$$
T = \begin{bmatrix} -0.09239 & 0.038268 & 300 \\ -0.38268 & -0.923879 & 165 \\ 0 & 0 & 1 \end{bmatrix}
$$



### Step 3: 3D LiDAR Mapping
Finally, the concepts are extended to 3D LiDAR data from the Cassie robot. Each scan, captured at 10 Hz, is transformed from its local sensor frame to the fixed world frame. The `data_parser` function provides the point cloud, intensity, and the transformation components ($R$ and $t$) for each time interval.

The transformation for each frame is constructed as:
$$
T = \begin{bmatrix} R_{3 \times 3} & t_{3 \times 1} \\ 0_{1 \times 3} & 1 \end{bmatrix}
$$

By iterating from time = 9s to 13s, each point cloud is transformed and appended to a global map, fusing the scans into a single, coherent 3D representation of the environment.



---

## IV - Results & Discussion

The transformations successfully corrected the misalignment in the raw LiDAR scans. Initially, the data was jumbled due to the robot's motion. After applying the correct homogeneous transformation for each frame, the point clouds aligned perfectly, with LiDAR rings centered around the robot's true position. Fusing the scans over five seconds produced a dense, coherent 3D map of the environment.

### Assumptions and Limitations
This project assumes that the robot’s true position and orientation (which provide $R$ and $t$) are known and accurate. In the real world, these values must be estimated using algorithms like SLAM (Simultaneous Localization and Mapping). This work focuses only on the "mapping" part, assuming "localization" is solved.

---

## V - Conclusion

This project successfully demonstrated how affine and homogeneous transformations are applied to real LiDAR data to reconstruct a 3D map from local sensor measurements. The results confirm that properly designed transformation matrices allow for the consistent alignment of LiDAR scans across multiple frames, which is the mathematical backbone of robotic mapping.

---

## Acknowledgement
This project is based on materials from the **ROB 101: Computational Linear Algebra** course at the University of Michigan. Original concept and dataset by Prof. Jessy Grizzle, Prof. Maani Ghaffari, and T.ribhi Kathuria. The implementation, code, and documentation presented here are my own.
