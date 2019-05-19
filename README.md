# Embankment
A numerical approach to determining the flow beneath an embankment.

<p align="center">
  <img src="./images/embankment_diagram.jpg">
  Figure 1. Cross-section through a conceptual embankment overlying a permeable stratum.
</p>


## Method

### 1. Discretization
The solution domain must be broken down into a set of representative points (discretized). This will allow the domain below the embankment to be visualized as a mesh.

### 2. Set of Governing Equations
Laplace equation is used to represent the physical process through the embankment, expressed as:
<p align="center">
  <img src="https://latex.codecogs.com/gif.latex?$$\nabla^2&space;H=\frac{\partial^2&space;H}{\partial&space;x^2}+\frac{\partial^2&space;H}{\partial&space;y^2}$$">
</p>

### 3. Numerical Representation of Governing Equations
Using partial derivatives that are accurate to second order in the mesh spacing. The 2nd order finite difference equation is:
<p align="center">
<img src="https://latex.codecogs.com/gif.latex?$$-−2(h^2+k^2)H_{i,j}+k^2&space;H_{i-1,j}+k^2&space;H_{i+1,j}&plus;h^2&space;H_{i,j-1}+h^2&space;H_{i,j+1}=0$$">
</p>

where <img src="https://latex.codecogs.com/gif.latex?$H_{i,j}$">  is the head at any internal grid point i, j.

### 4. Boundary Conditions
The domain is bounded and at each boundary there are potential effects of the boundary on the interior of the domain. The mesh must be constructed so that all boundary points coincide with the boundary of the mesh. An equation must be specified at every boundary point.

- At the inflow and outflow boundaries, the equation is trivial and the value of <img src="https://latex.codecogs.com/gif.latex?$H_{i,j}$">  at these point is simply the total head. For example, at each inflow point:
<p align="center">
<img src="https://latex.codecogs.com/gif.latex?$$H_{i,j}=h_1$$">
</p>

- At the other boundaries, a finite difference expression must be constructed by combining the governing equation with another equation stating that the gradient in <img src="https://latex.codecogs.com/gif.latex?$$H$$"> normal to the boundary is zero. We need four expressions for the left, right, upper (that is, beneath the embankment) and lower boundaries which are respectively:
<p align="center">

<img src="https://latex.codecogs.com/gif.latex?$$−-2(h^2+k^2)H_{i,j}+2k^2&space;H_{i+1,j}+h^2&space;H_{i,j-1}+h^2&space;H_{i,j+1}=0$$">
<br />
<img src="https://latex.codecogs.com/gif.latex?$$−-2(h^2+k^2)H_{i,j}+2k^2&space;H_{i-1,j}+h^2&space;H_{i,j-1}+h^2&space;H_{i,j+1}=0$$">
<br />
<img src="https://latex.codecogs.com/gif.latex?$$-−2(h^2+k^2)H_{i,j}+k^2&space;H_{i-1,j}+k^2&space;H_{i+1,j}+2h^2&space;H_{i,j-1}=0$$">
<br />
<img src="https://latex.codecogs.com/gif.latex?$$−-2(h^2+k^2)H_{i,j}+k^2&space;H_{i-1,j}+k^2&space;H_{i+1,j}+2h^2&space;H_{i,j+1}=0$$">

</p>

- There are two special boundary condition points: the lower left grid point and the lower right grid point. The applicable boundary conditions at these are respectively:
<p align="center">
<img src="https://latex.codecogs.com/gif.latex?$$-−2(h^2+k^2)H_{i,j}+2k^2&space;H_{i+1,j}+2h^2&space;H_{i,j+1}=0$$">
<br />
<img src="https://latex.codecogs.com/gif.latex?$$−-2(h^2+k^2)H_{i,j}+2k^2&space;H_{i-1,j}+2h^2&space;H_{i,j+1}=0$$">
</p>

### Solution Method for Numerical Equations
Constructs the full set of linear equations that incorporates an equation for each boundary and internal point. This will form a matrix that maps each point i,j to a row and column number in the matrix.

The solution will be:
<p align="center">
<img src="https://latex.codecogs.com/gif.latex?$$A&space;\hat&space;H&space;=\hat&space;b$$">
</p>

where <img src="https://latex.codecogs.com/gif.latex?$$A$$"> is the coefficient matrix, <img src="https://latex.codecogs.com/gif.latex?$$\hat&space;H$$"> is the unknown column vector of head values at each grid point and <img src="https://latex.codecogs.com/gif.latex?$$\hat&space;b$$"> is the column vector containing the constant value from the equation corresponding to each grid point.

Thus in order to solve for <img src="https://latex.codecogs.com/gif.latex?$$\hat&space;H$$">, the equation is rearranged to <img src="https://latex.codecogs.com/gif.latex?$$\hat&space;H=A^{-1}\hat&space;b$$">
</p>

Once <img src="https://latex.codecogs.com/gif.latex?$$\hat&space;H$$">is solved, the velocity components can be computed using 2nd order finite differences again as:
<p align="center">
<img src="https://latex.codecogs.com/gif.latex?$$u=-K&space;\frac{\partial^2&space;H}{\partial&space;x^2}=\frac{H_{i-1,j}+H_{i+1,j}}{2h}$$">
<br />
<img src="https://latex.codecogs.com/gif.latex?$$v=-K&space;\frac{\partial^2&space;H}{\partial&space;y^2}=\frac{H_{i,j-1}+H_{i,j+1}}{2k}$$">
</p>

The total volume flux of water beneath the embankment can be determined by integrating the velocity normal to a defined surface. The total volume flux per unit length along the crest of the embankment $q$ can be computed as:
<p align="center">
<img src="https://latex.codecogs.com/gif.latex?$$q=\oint&space;\vec{u}&space;\cdot&space;\hat&space;n&space;\partial&space;A$$">
</p>

<img src="https://latex.codecogs.com/gif.latex?$$u$$"> is the velocity vector at the defined surface, <img src="https://latex.codecogs.com/gif.latex?$$\hat&space;n$$"> is the local unit normal vector of the defined surface and <img src="https://latex.codecogs.com/gif.latex?$\partial&space;$$A$$"> is an elemental area of the defined surface.

The surface beneath the center of the embankment is therefore planar and of unit thickness, thus the above equation reduces to:
<p align="center">
<img src="https://latex.codecogs.com/gif.latex?$$q=\int_{0}^{D}&space;u(y)&space;\partial&space;y$$">
</p>

Note: the integral in the equation above can be approximated using the Trapezoidal rule. Therefore q can be determined by:
<p align="center">
<img src="https://latex.codecogs.com/gif.latex?$$q=\int_{0}^{D}&space;u(y)&space;\partial&space;y&space;\approx&space;k&space;(\frac{u_{i,1}}{2}+u_{i,2}+u_{i,3}+u_{i,4}+...+u_{i.n-1}+u_{i,n}+\frac{u_{i,n+1}}{2})$$">
</p>
