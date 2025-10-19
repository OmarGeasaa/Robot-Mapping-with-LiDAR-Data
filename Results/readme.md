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

This section outlines the methodology used to transform LiDAR data from the robot’s local coordinate frame into the world coordinate frame. It is divided into three main parts:
1.  2D geometric transformations
2.  2D point cloud correction
3.  3D LiDAR mapping and visualization

### Step I: 2D Transformations and Homogeneous Coordinates

#### Step 1.A: Translation
The first transformation applied is translation, which moves points by a constant offset ($t_x$, $t_y$). The shape is shifted by -0.5 units along both axes.
$$
\begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} x \\ y \end{bmatrix} + \begin{bmatrix} t_x \\ t_y \end{bmatrix} = \begin{bmatrix} x \\ y \end{bmatrix} + \begin{bmatrix} -0.5 \\ -0.5 \end{bmatrix}
$$

#### Step 1.B: Translation Using Homogeneous Coordinates
Using homogeneous coordinates, translation can be expressed as a matrix multiplication.
$$
T = \begin{bmatrix} 1 & 0 & t_x \\ 0 & 1 & t_y \\ 0 & 0 & 1 \end{bmatrix}
$$

#### Step 1.C: Rotation in Homogeneous Coordinates
Next, a rotation is applied counterclockwise by an angle $\theta$. For this step, the square is rotated by 45° ($\pi/4$ radians).
$$
\begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix} = \begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix}
$$

#### Step 1.D: Scaling
Scaling adjusts the size along the x- and y-axes by factors $S_x$ and $S_y$. In this case, $S_x = 1.5$ and $S_y = 0.5$.
$$
\begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix} = \begin{bmatrix} S_x & 0 & 0 \\ 0 & S_y & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix}
$$

#### Step 1.E: Combining Transformations: Affine Transformation
All transformations can be combined into a single composite matrix by multiplying them in order.
$$
T_{composite} = T_{scale} \times T_{rotate} \times T_{translate}
$$

![Figure 2: Demonstration of 2D geometric transformations on a square.](path/to/your/figure2.png)

### Step 2 – Working with 2D Point Clouds
This step corrects a distorted 2D point cloud. The data is a $3 \times N$ matrix where the first two rows are coordinates and the third is intensity. The correction is applied using a given affine transformation matrix:
$$
T = \begin{bmatrix} -0.09239 & 0.038268 & 300 \\ -0.38268 & -0.923879 & 165 \\ 0 & 0 & 1 \end{bmatrix}
$$

![Figure 3: Comparison of 2D point clouds before and after applying affine transformations.](path/to/your/figure3.png)

### Step 3 – 3D LiDAR Mapping and Visualization
This step extends the concepts to 3D LiDAR data from the Cassie robot. Each scan is transformed from the moving sensor frame to a fixed world frame. The `data_parser(id)` function provides the point cloud data, intensity, and the necessary transformation components ($R$ and $t$) for each time interval.

The transformation for each frame is constructed as:
$$
T = \begin{bmatrix} R_{3 \times 3} & t_{3 \times 1} \\ 0_{1 \times 3} & 1 \end{bmatrix}
$$

#### Part A – Single Frame
First, a single frame of raw LiDAR data is visualized. Then, the transformation is applied to correctly align the point cloud with the robot's pose in the world frame.

![Figure 4: Raw LiDAR data before processing.](path/to/your/figure4.png)
![Figure 5: Corrected LiDAR point cloud for a single frame.](path/to/your/figure5.png)

#### Part B – Building the Map (Fusing Scans)
To build a global map, multiple LiDAR scans (from time 9s to 13s) are fused. Each scan is transformed using its unique $R$ and $t$ matrices and then appended to a global map, creating a dense, unified 3D point cloud.

#### Animation
An animation is generated to visualize the mapping process, showing the robot's movement (white dot) as the map is built incrementally.

![Figure 6: Animated visualization (GIF) showing Cassie’s trajectory and the accumulated LiDAR map.](path/to/your/figure6.gif)

---

## IV - Results & Discussion

The affine transformations successfully corrected the misalignment in the raw LiDAR scans. Initially, plotting the raw data resulted in overlapping and displaced point clouds due to the robot's motion. By applying the correct homogeneous transformation matrix for each frame, the data was accurately projected into a common world coordinate frame. The final fused point cloud produced a coherent and detailed 3D map of the environment.

### Assumptions and Limitations
This project assumes that the robot’s true position and orientation (which form the $R$ and $t$ components) at each frame are known and accurate. In real-world applications, these values must be estimated using algorithms like SLAM (Simultaneous Localization and Mapping). This project focuses solely on the "mapping" part, assuming the "localization" is already solved.

---

## V - Conclusion

This project successfully demonstrated how affine and homogeneous transformations are applied to real LiDAR data to reconstruct a 3D map from local sensor measurements. The results confirm that properly designed transformation matrices allow for the consistent alignment of LiDAR scans across multiple frames. While key parameters were provided, the process mirrors the core mathematical pipeline used in real-world robotic perception systems.

---

## Acknowledgement
This project is based on materials from the **ROB 101: Computational Linear Algebra** course at the University of Michigan. Original concept and dataset by Prof. Jessy Grizzle, Prof. Maani Ghaffari, and T.ribhi Kathuria. The implementation, code, and documentation presented here are my own.
