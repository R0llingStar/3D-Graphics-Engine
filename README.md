
# To build and run program on IDE, below steps are followed:
1.	Open 3D-Shpere project in the IDE.
2.	Open 3D-Sphere2.2.c file.
3.	Connect the LPC 1769 board to your computer with a micro-USB cable.
4.	Build the project by clicking the hammer button. Observe the console’s output. Make sure there are no errors.  
5.	Click the debug button. It’s the green bug button. A window will pop up asking you to select your LPC-Link.
6.	Once selected, code is loaded on the target and Eclipse will switch to a debug perspective.
7.  Click the resume (F8) button to execute the code.


# 3D Graphics Processing Engine Design with Diffuse Reflection and Shadow Computation
The project's goal is to build a 3D graphics engine out of LPC1769 and a color LCD display out of Serial Peripheral Interface (SPI)communication. On the global coordinate system, a solid 3D picture of a cube with an elevation from the xy plane needs to be displayed and a solid half sphere with letter "S" must be displayed around the origin.The cube must be rotated by 5 dgress clockwise direction around the arbitary vector. A light point source is employed, and the top surface of the cube has diffused reflection with the greatest color dynamic range. The diffuse reflection at four corners of the cube's top surface was computed. A linear decorating technique has been devised, which inserts a tree on one of the cube's two frontal surfaces.


## Objectives
1. Make a world coordinate system and a solid cube and plot them.
2. The virtual camera location E = (ex,ey,ez) = (150, 150, 100), and the cube side length is set at 50. The cube is floated in the world coordinate system and is rotated 5 degree clockwise, with a floating height of 10 from the xw-yw plane  axis.
3. Calculate the diffuse reflection at four corners of the cube's top surface using K r = 0.8 and devise a post-processing strategy to match the LPC1769 LCD's entire dynamic range. To span the display range of 20 to 255, apply the linear equation to scale up the diffuse reflection color.
4. To Place a point light source P_s(xs, ys, zs)=(-20,-20,220) in the world coordinate system, and design its location.
5. The ray equation and its intersection with the x w-y w plane must be computed.
6. To compute diffuse reflection on the top surface of the cube,using one primitive color.
7. Place a letter "S" using your customer-designed font on the top surface of the half sphere, then adorn the one of the two visible cube surfaces with trees(must be implemented using linear decoration algorithm).


## Methodology

### Changes in Configuration for Better Output

Camera Location E = (250,250,200)
Cube Side length S = 100
Cube Center  = (0,150,100)
cube rotation  = -10 (-0.174533 rad)
Point Light Source = (0,150,250)
Half Sphere Orogin = (0,0,0)
Half Sphere Radius = 150

### 3D Cube and Half Sphere
The first part of the project consists of displaying a 3D cube and a half sphere around the origin. The transformation matrix is used to convert World coordinate system in viewer coordinate system.


### Solid Cube
1. Calculate the 7 Points to draw the cube based on configuration
2. Rotate these points along vector Parellal to hte Y-axis and going through the center of the cube
3. Fill 3 visible sides of the cube

### Shadow
To draw the shadow:
1. Calculated the ray equations from a light point source to each of the cube's four vertices.
2. Find the spot where each of these ray equations intersects the x-y plane.
3. To get the shadow, draw lines between all of the line points calucluates and fill with color.
 
### Diffused Reflection
Light source Is(x, y) consists of r,g,b 3 primitive colors as follows, but we need to simplify it as white color and have the highest value.
1. Only the top surface of the cube is considered for diffused reflection.
2. The reflectivity of the primitive colors is given green = 0, blue =0, red = 0.8.
3. The diffused reflection is only computed in the world coordinate system.
4. A scaling factor is required since diffused reflection is very small and scaling it up is necessary for the dynamic range of display devices. An extra offset of 20 is taken to make it more visible.
5. The diffused reflection on boundary points needs to be computed. A diffused reflection model is used, and the color and intensity are computed. Boundary color to be computed by a simpler technique interpolation. The color change is taken as a function of both x and y:
a. Contribution of color changing from x
b. Contribution of color changing from y
Then the final color is the average of contribution x and contribution from y. Intensity of diffuse reflection contribution from y
6. Bilinear interpolation in conjunction with DDA algorithm could be used to compute reflection at the boundary line.
7. The following steps are taken to compute diffused reflection at interior points:
a) Scan the display conceptually from top left corner to right, top to bottom.
b) Scanning line L has 2 intersection points which are boundary points. We use interpolation techniques to find interpolation points.


### Draw Half Sphere
1. Calculate the point on surface of half sphere by 2 angles (ranging from 0 to 2 PI)
2. Draw each pixel on surface of the half sphere

### Decorate S
To display the letter "S":
1. Collect the array of points that represent the letter S during half sphere computation according to change in angles
2. Draw the collected points on LCD

### Tree
1. Created patch of tree by modifying one parent tree;
2. Randomized angles for the branches;
3. Transpose the x and y coordinates as per y-z plane and put them at an elevation of 50 from x-y plane and at a distance of half cube size (50)from y-z plane


