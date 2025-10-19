# Robot Mapping with LiDAR Data

**By Omar Ahmed Geasa**
*Mechatronics Engineering, Tanta University*
*Omargeasaa@gmail.com*

---

## Abstract

[cite_start]This mini-project demonstrates how to manipulate raw LiDAR data using point clouds to construct a visual map of the environment[cite: 160]. [cite_start]The work focuses on applying affine transformations from linear algebra to convert data from the robot’s local coordinate frame into a world reference frame[cite: 161]. [cite_start]This project shows how affine transformations enable accurate alignment of multiple LiDAR scans for real-time robotic mapping[cite: 162]. [cite_start]The dataset used in this work was collected by the Cassie bipedal robot at the University of Michigan[cite: 163].

---

## I - Introduction

[cite_start]Many autonomous systems rely on perception sensors such as LiDAR to localize, map, and navigate in unknown environments[cite: 165]. [cite_start]LiDAR systems generate dense point clouds that capture the 3D structure of the environment with high precision[cite: 167]. [cite_start]However, LiDAR collects data in its own local coordinate frame, which moves with the robot[cite: 168, 282]. [cite_start]As the robot moves, this causes misalignment among successive scans[cite: 169]. [cite_start]Therefore, it is necessary to transform all data into a common world coordinate frame to construct a coherent map[cite: 170]. [cite_start]This is achieved by computing the affine transformation between the local and global frames as the robot moves[cite: 171].

[cite_start]This project demonstrates how local sensor data can be transformed into a unified global map by processing large-scale data using matrix operations, specifically expressing translation and rotation in homogeneous coordinates[cite: 173, 174].



---

## II - Background

### Affine Transformation
[cite_start]An **affine transformation** is a geometric mapping that preserves straight lines and parallelism between points[cite: 177]. [cite_start]It combines linear operations like rotation and scaling with translation into a single framework[cite: 178]. [cite_start]By using homogeneous coordinates, a complex sequence of operations can be expressed as a single matrix multiplication[cite: 181].

$f(x) = y = Ax + t$

### Homogeneous Coordinates
While rotation and scaling are linear, translation is not. [cite_start]**Homogeneous coordinates** are a clever trick to express all affine transformations, including translation, within a single matrix multiplication[cite: 185, 186]. [cite_start]A 2D point $[x,y]^T$ becomes $[x,y,1]^T$, and a 3D point $[x,y,z]^T$ becomes $[x,y,z,1]^T$[cite: 187]. This allows the transformation to be written as:

$$
\begin{bmatrix} x' \\ y' \\ z' \\ 1 \end{bmatrix} = \begin{bmatrix} A & t \\ 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ z \\ 1 \end{bmatrix}
$$

### Application in Robotics
[cite_start]In robotics, this is essential for relating sensor frames to a world frame[cite: 205, 206]. [cite_start]For LiDAR mapping, the **homogeneous transformation matrix** takes this form[cite: 207, 208]:

$$
T = \begin{bmatrix} R_{3 \times 3} & t_{3 \times 1} \\ 0_{1 \times 3} & 1 \end{bmatrix}
$$

Where:
* [cite_start]$R_{3 \times 3}$ is a 3x3 **rotation matrix** [cite: 210]
* [cite_start]$t_{3 \times 1}$ is a 3x1 **translation vector** [cite: 211]
* [cite_start]$0_{1 \times 3}$ is a 1x3 row vector of zeros [cite: 212]
* [cite_start]$1$ is a scaling factor [cite: 213]

---

## III - Methodology

### Step 1: 2D Transformations
[cite_start]The project starts by illustrating 2D affine transformations (translation, rotation, and scaling) on a simple square[cite: 222].

#### Translation
[cite_start]Each point of the square is shifted by -0.5 units along both axes to center it[cite: 228].
$$
\begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} x \\ y \end{bmatrix} + \begin{bmatrix} t_x \\ t_y \end{bmatrix} = \begin{bmatrix} x \\ y \end{bmatrix} + \begin{bmatrix} -0.5 \\ -0.5 \end{bmatrix}
$$

#### Rotation
[cite_start]The square is rotated counterclockwise by 45° ($\pi/4$ radians)[cite: 244].
$$
\begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix} = \begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix}
$$

#### Scaling
[cite_start]The square is stretched horizontally ($S_x=1.5$) and compressed vertically ($S_y=0.5$)[cite: 250].
$$
\begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix} = \begin{bmatrix} S_x & 0 & 0 \\ 0 & S_y & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix}
$$



### Step 2: 2D Point Cloud Correction
[cite_start]Next, an affine transformation is used to correct a distorted 2D point cloud image stored in `question_image.csv`[cite: 262, 270].
$$
T = \begin{bmatrix} -0.09239 & 0.038268 & 300 \\ -0.38268 & -0.923879 & 165 \\ 0 & 0 & 1 \end{bmatrix}
$$



### Step 3: 3D LiDAR Mapping
[cite_start]Finally, the concepts are extended to 3D LiDAR data from the Cassie robot[cite: 279]. [cite_start]Each scan, captured at 10 Hz, is transformed from its local sensor frame to the fixed world frame[cite: 281, 283]. [cite_start]The `data_parser` function provides the point cloud, intensity, and the transformation components ($R$ and $t$) for each time interval[cite: 284].

[cite_start]The transformation for each frame is constructed as[cite: 292]:
$$
T = \begin{bmatrix} R_{3 \times 3} & t_{3 \times 1} \\ 0_{1 \times 3} & 1 \end{bmatrix}
$$

[cite_start]By iterating from time = 9s to 13s, each point cloud is transformed and appended to a global map, fusing the scans into a single, coherent 3D representation of the environment[cite: 302, 308].



---

## IV - Results & Discussion

[cite_start]The transformations successfully corrected the misalignment in the raw LiDAR scans[cite: 321]. [cite_start]Initially, the data was jumbled due to the robot's motion[cite: 321]. [cite_start]After applying the correct homogeneous transformation for each frame, the point clouds aligned perfectly, with LiDAR rings centered around the robot's true position[cite: 324]. [cite_start]Fusing the scans over five seconds produced a dense, coherent 3D map of the environment[cite: 326].

### Assumptions and Limitations
[cite_start]This project assumes that the robot’s true position and orientation (which provide $R$ and $t$) are known and accurate[cite: 329]. [cite_start]In the real world, these values must be estimated using algorithms like SLAM (Simultaneous Localization and Mapping)[cite: 330]. This work focuses only on the "mapping" part, assuming "localization" is solved.

---

## V - Conclusion

[cite_start]This project successfully demonstrated how affine and homogeneous transformations are applied to real LiDAR data to reconstruct a 3D map from local sensor measurements[cite: 335]. [cite_start]The results confirm that properly designed transformation matrices allow for the consistent alignment of LiDAR scans across multiple frames, which is the mathematical backbone of robotic mapping[cite: 335, 336].

---

## Acknowledgement
[cite_start]This project is based on materials from the **ROB 101: Computational Linear Algebra** course at the University of Michigan[cite: 339]. [cite_start]Original concept and dataset by Prof. Jessy Grizzle, Prof. Maani Ghaffari, and T.ribhi Kathuria[cite: 340]. [cite_start]The implementation, code, and documentation presented here are my own[cite: 341].
