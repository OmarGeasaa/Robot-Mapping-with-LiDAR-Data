# [cite_start]Robot Mapping with LiDAR Data [cite: 154]

[cite_start]**By Omar Ahmed Geasa** [cite: 155]  
[cite_start]*Mechatronics Engineering, Tanta University* [cite: 156, 157]  
[cite_start]*Omargeasaa@gmail.com* [cite: 158]

---

## [cite_start]Abstract [cite: 159]

[cite_start]This mini-project demonstrates how to manipulate raw LiDAR data using point clouds to construct a visual map of the environment[cite: 160]. [cite_start]The work focuses on applying affine transformations concepts from linear algebra to convert data from the robot’s local coordinate frame into a world reference frame[cite: 161]. [cite_start]This project shows how affine transformations enable accurate alignment of multiple LiDAR scans for real-time robotic mapping[cite: 162]. [cite_start]The dataset used in this work was collected by the Cassie bipedal robot at the University of Michigan[cite: 163].

## [cite_start]I - Introduction [cite: 164]

[cite_start]Many autonomous systems rely on perception sensors such as stereo cameras and LiDAR to localize, map, and navigate in unknown environments[cite: 165]. [cite_start]By using these sensors, the robot can construct a spatial representation of its surroundings and how it interacts with them[cite: 166]. [cite_start]LiDAR systems generate dense point clouds that capture the 3D structure of the environment with high precision which allows us to visualize how the robot operates within its surroundings[cite: 167]. [cite_start]However, LiDAR collects data in its own local coordinate frame, which does not always align with the robot’s reference frame[cite: 168]. [cite_start]As the robot moves, both the LiDAR and robot coordinate frames change with respect to the environment, causing misalignment among successive scans[cite: 169]. [cite_start]Therefore, it is necessary to transform all robot data into a common world or global coordinate frame in order to construct the map[cite: 170]. [cite_start]This can be achieved by computing the affine transformation between the local and global coordinates as the robot moves through the environment[cite: 171].

[cite_start]This project focuses on implementing affine transformations using linear algebra to transform LiDAR point cloud data[cite: 172]. [cite_start]The project demonstrates how local sensor data can be transformed into a unified global map representation suitable for robotic mapping applications in addition to how large-scale data can be efficiently processed using matrix operations[cite: 173]. [cite_start]This is achieved by using real dataset expressing translation, and rotation operations in homogeneous coordinates[cite: 174].

[cite_start]![Figure 1: Motivation: Expected project output showing LiDAR-based 3D mapping of the robot’s environment.](path/to/your/figure1.png) [cite: 198, 199]

## [cite_start]II - Background [cite: 175]

### [cite_start]i - Affine Transformation [cite: 176]
[cite_start]An affine transformation is a geometric mapping that preserves straight lines and parallelism between points, meaning that sets of parallel lines remain parallel after the transformation[cite: 177]. [cite_start]It combines multiple linear operations, such as translation, rotation, scaling, and sharing, into a unified mathematical framework, although it does not necessarily preserve angles or distances[cite: 178]. [cite_start]In computer graphics, affine transformations enable the rendering of dynamic scenes and the manipulation of 3D models[cite: 179]. [cite_start]In computer vision, they are applied to correct image distortions caused by wide-angle lenses or changes in camera perspective[cite: 180]. [cite_start]By representing these transformations through homogeneous coordinates, complex sequences of operations can be expressed as a single matrix multiplication[cite: 181].

[cite_start]$f(x) = y = Ax + t$ [cite: 182, 183]

### [cite_start]ii - Homogeneous Coordinates [cite: 184]
[cite_start]While affine transformations describe how points are mapped between coordinate frames through rotation, scaling, and translation, the translation component cannot be represented purely as a matrix multiplication in standard Cartesian coordinates[cite: 185]. [cite_start]Homogeneous coordinates are introduced to express all transformation operations in a single matrix[cite: 186]. [cite_start]In homogeneous coordinates, a 2D point $[x,y]^T$ becomes $[x,y,1]^T$, and a 3D point $[x,y,z]^T$ becomes $[x,y,z,1]^T$[cite: 187]. [cite_start]This allows the affine transformation to be written as a single matrix–vector multiplication[cite: 188]:

$$
\begin{bmatrix} x' \\ y' \\ z' \\ 1 \end{bmatrix} = \begin{bmatrix} A & t \\ 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ z \\ 1 \end{bmatrix}
$$
[cite_start][cite: 189]

[cite_start]Here, A represents the linear part of the transformation (rotation, scaling, or shear), and t is the translation vector[cite: 190]. [cite_start]The resulting matrix is known as the homogeneous transformation matrix[cite: 191]. [cite_start]This formulation makes it possible to combine multiple transformations into a single matrix multiplication[cite: 192, 193]. [cite_start]This is used to convert point clouds from the LiDAR’s local frame to the global world frame, aligning sensor data with the robot’s estimated pose[cite: 194].

### [cite_start]iii - Application in Robotics [cite: 205]
[cite_start]In robotics, homogeneous transformation matrices are essential for describing how sensors and robot components move relative to one another[cite: 206]. [cite_start]For example, in LiDAR-based mapping, a transformation matrix is of the form[cite: 207]:
$$
T = \begin{bmatrix} R_{3 \times 3} & t_{3 \times 1} \\ 0_{1 \times 3} & 1 \end{bmatrix}
$$
[cite_start][cite: 208]
Where:
* [cite_start]$R_{3 \times 3}$ is a 3 x 3 rotation matrix [cite: 210]
* [cite_start]$t_{3 \times 1}$ is a 3 x 1 translation vector [cite: 211]
* [cite_start]$0_{1 \times 3}$ is a 1 x 3 row vector of zeros [cite: 212]
* [cite_start]$1$ is the number one [cite: 213]

[cite_start]This is applied to LiDAR data collected by the robot[cite: 201]. [cite_start]Each LiDAR scan is originally captured in the sensor’s local frame[cite: 202]. [cite_start]By applying the homogeneous transformation matrix composed of the rotation matrix R and translation vector t, the data is transformed into the global frame[cite: 203]. [cite_start]This step ensures that consecutive LiDAR scans are properly aligned, allowing a coherent 3D map of the environment to be constructed[cite: 204].

## [cite_start]III - Methodology [cite: 214]

[cite_start]This section outlines the methodology used to transform LiDAR data from the robot’s local coordinate frame into the world coordinate frame[cite: 215]. [cite_start]It is divided into three main parts[cite: 216]:
1.  [cite_start]2D geometric transformations [cite: 217]
2.  [cite_start]2D point cloud correction [cite: 218]
3.  [cite_start]3D LiDAR mapping and visualization [cite: 219]

### [cite_start]Step I: 2D Transformations and Homogeneous Coordinates [cite: 221]
[cite_start]In this step, simple geometric shapes are plotted and transformed using matrix operations to illustrate the concept of affine transformations[cite: 222]. [cite_start]These transformations combine translation, rotation, and scaling within a single mathematical framework[cite: 223].

#### [cite_start]Step 1.A: Translation [cite: 226]
[cite_start]The first transformation applied to the square is translation which moves the points by a constant offset ($t_x$, $t_y$)[cite: 227]. [cite_start]Each point of the square is shifted by 0.5 units along both the x-axis and y-axis to center the shape around the origin[cite: 228].
$$
\begin{bmatrix} x' \\ y' \end{bmatrix} = \begin{bmatrix} x \\ y \end{bmatrix} + \begin{bmatrix} t_x \\ t_y \end{bmatrix} = \begin{bmatrix} x \\ y \end{bmatrix} + \begin{bmatrix} -0.5 \\ -0.5 \end{bmatrix}
$$
[cite_start][cite: 230]

#### [cite_start]Step 1.B: Translation Using Homogeneous Coordinates [cite: 232]
[cite_start]To express translation as a matrix-vector multiplication, the points are represented in homogeneous coordinates[cite: 233]. [cite_start]A 2D point $[x,y]^T$ is converted to its homogeneous representation by appending a constant 1, forming $[x,y,1]^T$[cite: 235]. [cite_start]Translation is then expressed as[cite: 236]:
$$
T = \begin{bmatrix} 1 & 0 & t_x \\ 0 & 1 & t_y \\ 0 & 0 & 1 \end{bmatrix}
$$
[cite_start][cite: 237]

#### [cite_start]Step 1.C: Rotation in Homogeneous Coordinates [cite: 241]
[cite_start]Next, a rotation is applied using the 2D rotation matrix in a counterclockwise direction by angle $\theta$[cite: 242]:
$$
\begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix} = \begin{bmatrix} \cos\theta & -\sin\theta & 0 \\ \sin\theta & \cos\theta & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix}
$$
[cite_start][cite: 243]
[cite_start]For this step, the square is rotated by 45° ($\pi/4$ radians)[cite: 244].

#### [cite_start]Step 1.D: Scaling [cite: 246]
[cite_start]Scaling adjusts the size of the square along the x- and y-axes by factors $S_x$ and $S_y$[cite: 247]. [cite_start]It is a non-rigid transformation[cite: 248].
$$
\begin{bmatrix} x' \\ y' \\ 1 \end{bmatrix} = \begin{bmatrix} S_x & 0 & 0 \\ 0 & S_y & 0 \\ 0 & 0 & 1 \end{bmatrix} \begin{bmatrix} x \\ y \\ 1 \end{bmatrix}
$$
[cite_start][cite: 249]
[cite_start]In this case, $S_x = 1.5$ and $S_y = 0.5$[cite: 250].

#### [cite_start]Step 1.E: Combining Transformations: Affine Transformation [cite: 251]
[cite_start]All the transformations can be expressed within a single affine framework[cite: 252]. [cite_start]They can be multiplied together as homogeneous matrices[cite: 253]:
$$
T = \begin{bmatrix} 1.5 & 0 & 0 \\ 0 & 0.5 & 0 \\ 0 & 0 & 1 \end{bmatrix} \times \begin{bmatrix} \cos(\pi/4) & -\sin(\pi/4) & 0 \\ \sin(\pi/4) & \cos(\pi/4) & 0 \\ 0 & 0 & 1 \end{bmatrix} \times \begin{bmatrix} 1 & 0 & -0.5 \\ 0 & 1 & 0.5 \\ 0 & 0 & 1 \end{bmatrix}
$$
[cite_start][cite: 254]
[cite_start]This composite matrix represents the overall affine transformation[cite: 255].

![Figure 2: Demonstration of 2D geometric transformations applied to a square. The original shape (green) undergoes translation (black), rotation (red), and scaling (blue). [cite_start]The final output (blue) shows that performing all three steps sequentially yields the same result as applying a single combined affine transformation.](path/to/your/figure2.png) [cite: 256, 257, 258, 259]

### [cite_start]Step 2 – Working with 2D Point Clouds [cite: 260]
[cite_start]This step introduces point cloud manipulation[cite: 261]. [cite_start]For a 2D point cloud, the dataset can be represented as a $3 \times N$ matrix[cite: 264]:
$$
\begin{bmatrix} x_1 & x_2 & \dots & x_N \\ y_1 & y_2 & \dots & y_N \\ I_1 & I_2 & \dots & I_N \end{bmatrix}
$$
[cite_start][cite: 265]
[cite_start]To correct distortion, a known affine transformation is applied[cite: 266]. [cite_start]The transformation matrix is given as[cite: 267]:
$$
T = \begin{bmatrix} -0.09239 & 0.038268 & 300 \\ -0.38268 & -0.923879 & 165 \\ 0 & 0 & 1 \end{bmatrix}
$$
[cite_start][cite: 273]
[cite_start]After transformation, the scattered points re-align to form a readable image[cite: 277].

[cite_start]![Figure 3: Comparison of 2D point clouds before and after applying affine transformations.](path/to/your/figure3.png) [cite: 268, 269]

### [cite_start]Step 3 – 3D LiDAR Mapping and Visualization [cite: 278]
[cite_start]This step extends the concepts to 3D LiDAR data collected by the Cassie Blue robot[cite: 279]. [cite_start]Affine transformations are used to register multiple LiDAR scans into a single coherent map[cite: 280]. [cite_start]Cassie’s LiDAR captures point clouds at 10 Hz[cite: 281]. [cite_start]Each scan is defined in the LiDAR sensor’s local coordinate frame and must be transformed into a fixed world frame[cite: 282, 283]. [cite_start]The `data_parser(id)` function for each interval `id` returns[cite: 284]:
-   [cite_start]`pointcloud_data`: $3 \times N$ array of $[x, y, z]$ [cite: 286]
-   [cite_start]`Intensity_data`: vector of LiDAR intensities [cite: 287]
-   [cite_start]`R`: $3 \times 3$ rotation matrix [cite: 288]
-   [cite_start]`t`: $3 \times 1$ translation vector [cite: 289]
-   [cite_start]`pose`: Cassie’s position and orientation [cite: 290]

[cite_start]Each LiDAR scan is transformed to the world frame by the matrix[cite: 291]:
$$
T = \begin{bmatrix} R_{3 \times 3} & t_{3 \times 1} \\ 0_{1 \times 3} & 1 \end{bmatrix}
$$
[cite_start][cite: 292]

#### [cite_start]Part A – Single Frame [cite: 294]
[cite_start]![Figure 4: Raw LiDAR data collected before any processing or frame alignment.](path/to/your/figure4.png) [cite: 297, 298]
[cite_start]First, a single frame is processed[cite: 295]. [cite_start]After applying the transformation matrix `T`, the LiDAR rings should be centered around the robot[cite: 299, 300].

[cite_start]![Figure 5: Corrected LiDAR point cloud for frame 9 after applying the homogeneous transformation.](path/to/your/figure5.png) [cite: 316, 317]

#### [cite_start]Part B – Building the Map (Fusing Scans) [cite: 301]
[cite_start]To build a global map, multiple LiDAR scans from 9–13 s are fused together[cite: 302]. [cite_start]For each `id`, the local point cloud is transformed and appended to the global map array[cite: 303, 304, 306, 307]. [cite_start]After looping over all frames, the result is a dense, unified 3D point cloud[cite: 308].

#### [cite_start]Animation [cite: 311]
[cite_start]A GIF animation visualizes the mapping process in real time[cite: 312]. [cite_start]Each frame adds a new LiDAR scan, while a white dot marks the robot’s position[cite: 313].

[cite_start]![Figure 6: Animated visualization (GIF) showing Cassie’s trajectory and accumulated LiDAR map after frame alignment.](path/to/your/figure6.png) [cite: 318, 319]

## [cite_start]IV - Results & Discussion [cite: 320]

[cite_start]After applying the affine transformation, the corrected results clearly demonstrate the effectiveness of the method[cite: 321]. [cite_start]Initially, the raw LiDAR scans appeared misaligned due to the robot’s motion[cite: 321]. [cite_start]By constructing and applying the homogeneous transformation matrix, the data from each frame were accurately projected into the world coordinate frame[cite: 322, 324]. [cite_start]The corrected results display concentric LiDAR rings centered about the robot’s true position, confirming that the transformations successfully compensated for Cassie’s motion[cite: 324, 325]. [cite_start]Finally, by fusing LiDAR data across five time intervals, a global 3D map was constructed[cite: 326].

### [cite_start]Assumptions and Limitations [cite: 328]
[cite_start]The experiment assumes that Cassie’s true position and orientation at each frame are known and accurate[cite: 329]. [cite_start]In real-world robotics, these values must be estimated using algorithms such as SLAM[cite: 330]. [cite_start]The transformation matrix (R, t) is also assumed to be given and precise[cite: 331]. [cite_start]In practical scenarios, determining these parameters involves sensor calibration[cite: 332]. [cite_start]Despite these simplifications, the experiment effectively demonstrates the principles required for large-scale spatial transformations[cite: 333].

## [cite_start]V - Conclusion [cite: 334]

[cite_start]This project successfully demonstrated how affine and homogeneous transformations can be applied to real LiDAR data to reconstruct a 3D map from local sensor measurements[cite: 335]. [cite_start]The results confirm that properly designed transformation matrices allow consistent alignment of LiDAR scans across multiple frames[cite: 336]. [cite_start]Although certain parameters were provided, the process mirrors real-world robotic perception pipelines[cite: 336]. [cite_start]Future extensions could include integrating pose estimation algorithms to autonomously estimate the robot’s position and orientation in real time[cite: 337].

## [cite_start]Acknowledgement [cite: 338]
[cite_start]This project is based on materials from the ROB 101: Computational Linear Algebra course at the University of Michigan[cite: 339]. [cite_start]Original concept and dataset by Prof. Jessy Grizzle, Prof. Maani Ghaffari, and T.ribhi Kathuria[cite: 340]. [cite_start]The Implementation, code, and documentation here are my own[cite: 341].
