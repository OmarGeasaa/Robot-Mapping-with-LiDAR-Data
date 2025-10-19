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

Homogeneous Coordinates

While rotation and scaling can be represented by matrix multiplication, translation requires addition.
Homogeneous coordinates are a mathematical trick that allows all affine transformations — including translation — to be represented by a single matrix multiplication.

A 3D point $(x, y, z)$ becomes $(x, y, z, 1)$, allowing the transformation to be written as:

[
𝑥
′


𝑦
′


𝑧
′


1
]
=
[
𝐴
	
𝑡


0
	
1
]
[
𝑥


𝑦


𝑧


1
]
	​

x
′
y
′
z
′
1
	​

	​

=[
A
0
	​

t
1
	​

]
	​

x
y
z
1
	​

	​

Application in Robotics 🤖

In robotics, this is essential for relating sensor frames to a world frame.
For LiDAR mapping, the homogeneous transformation matrix takes this form:

𝑇
=
[
𝑅
3
×
3
	
𝑡
3
×
1


0
1
×
3
	
1
]
T=[
R
3×3
	​

0
1×3
	​

	​

t
3×1
	​

1
	​

]

Where:

$R_{3\times3}$: rotation matrix

$t_{3\times1}$: translation vector

$0_{1\times3}$: row vector of zeros

$1$: homogeneous coordinate

Methodology
Step 1: 2D Transformations

The project begins by illustrating 2D affine transformations on a simple square to build intuition.

Translation

Each point of the square is shifted by $-0.5$ units along both axes.

[
𝑥
′


𝑦
′
]
=
[
𝑥


𝑦
]
+
[
−
0.5


−
0.5
]
[
x
′
y
′
	​

]=[
x
y
	​

]+[
−0.5
−0.5
	​

]
Rotation

The square is rotated counterclockwise by 45° ($\pi / 4$ radians).

[
𝑥
′


𝑦
′


1
]
=
[
cos
⁡
𝜃
	
−
sin
⁡
𝜃
	
0


sin
⁡
𝜃
	
cos
⁡
𝜃
	
0


0
	
0
	
1
]
[
𝑥


𝑦


1
]
	​

x
′
y
′
1
	​

	​

=
	​

cosθ
sinθ
0
	​

−sinθ
cosθ
0
	​

0
0
1
	​

	​

	​

x
y
1
	​

	​

Scaling

The square is stretched horizontally ($S_x = 1.5$) and compressed vertically ($S_y = 0.5$).

[
𝑥
′


𝑦
′


1
]
=
[
𝑆
𝑥
	
0
	
0


0
	
𝑆
𝑦
	
0


0
	
0
	
1
]
[
𝑥


𝑦


1
]
	​

x
′
y
′
1
	​

	​

=
	​

S
x
	​

0
0
	​

0
S
y
	​

0
	​

0
0
1
	​

	​

	​

x
y
1
	​

	​

Step 2: 2D Point Cloud Correction

An affine transformation is then used to correct a distorted 2D point cloud image:

𝑇
=
[
−
0.09239
	
0.038268
	
300


−
0.38268
	
−
0.923879
	
165


0
	
0
	
1
]
T=
	​

−0.09239
−0.38268
0
	​

0.038268
−0.923879
0
	​

300
165
1
	​

	​

Step 3: 3D LiDAR Mapping

Finally, these concepts are applied to 3D LiDAR data from the Cassie robot.
Each scan is transformed from its local sensor frame to the fixed world frame using its unique transformation matrix, constructed from the provided rotation ($R$) and translation ($t$) vectors.

By iterating over time, each transformed point cloud is appended to a global map, fusing the scans into a single, coherent 3D representation.
