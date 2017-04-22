# Class for calculating Instantaneous Axis of Rotation

The instantaneous axis of rotation between two bodies is a useful concept in
biomechanics. In this post, we will dig into how to calculate the instantaneous
axis of rotation and show off an AnyScript `class_template` to do this
calculation and plot the axis between any two reference frames in the AnyBody
Modeling System.

The are several papers that discusses the
importance of the instantaneous axis of rotation in biomechanics. See for
example XX and XX.  But first, let us look briefly at the math behind.

## 3D Rotations

We can view any displacement of a body in a three-dimensional space as rotation
around a single axis and a translation along that axis. This gives rise to the
idea of a screw motion in 3D along what is also called the helical axis. If we
spit the movement up into infinitesimally small movements each of these will
have an rotation axis and a linear velocity along that axis. This is the
instantaneous axis of rotation, and as the name indicate will change direction
and location as the object moves. 

If we know the angular and linear velocity of any point on a rigid body, we can
calculate the properties of the screw motion and the instantaneous axis of
rotation.

From a mathematical point of view the rate of change of a 3x3 rotation matrix
$R$ is:

$$ \frac{{\mathrm d}}{{\mathrm d}t}R = \vec{\omega} \times R$$

where $\vec{\omega}$ is called the angular velocity vector. This angular
velocity is quantity shared by all points on the rigid body (including those on
the rotation axis). Using $\vec{\omega}$ we can write the linear velocity
$\vec{v}$ of any point at some distance $\vec{r} from the instantaneous axis of
rotation as:

$$\vec{v} = \vec{\omega} \times \vec{r}$$ (1)

Similarly we can also calculate backwards and find the intantanous axis of
rotation at some distance $-\vec{r}$ from any point if we know the linear
velocity of the point and the angular velocity:

This is given by:

$$ 
 -\vec{r}= \frac{\vec{\omega} \times \vec{v} }{ \vec{\omega}\cdot\vec{\omega} } 
$$

Here is short proof. Skip it if you like: 

>Start with $\vec{\omega} \times \vec{v}$, insert (1) and use the  tripple product to expand it:
>$$ 
>\vec{\omega} \times \vec{v} = \vec{\omega} \times (\vec{\omega} \times \vec{r})=\vec{\omega}(\vec{\omega}\cdot\vec{r})-\vec{r}(\vec{\omega}\cdot\vec>{\omega})
>$$
>
>The shortest vector $\vec{r}$ from the rotation axis to any point is by defintion perpendicular to angular velocity $\vec{\omega}$, hence:
>
>$$ 
>\require{cancel}\vec{\omega} \times \vec{v} =\vec{\omega}(\cancel{\vec{\omega}\cdot\vec{r}})-\vec{r}(\vec{\omega}\cdot\vec{\omega})=-r(\vec{\omega}>\cdot\vec{\omega})
>$$



Thus if know the position of a point $\vec{r}_P$ we can find closest point
$\vec{r}_C$ on the rotation axis as:

 $$\vec{r}_C = \vec{r}_P - \vec{r} = \vec{r}_P + \frac{\vec{\omega}\times\vec{v}_P}{\vec{\omega}\cdot\vec{\omega}}$$



## Properties

If for example we have a rigid body with angular velocity $\vec{\omega}$ and
some point $P$ with position $\vec{r}_P$ and velocity $\vec{v}_p$ then we can
define the different properties:

1. The magintude of agular rotation:
$$ \omega = \|\vec{\omega}\|$$

2. The direction of the intantanous axis of rotation:

$$ \vec{v}_{IOAR} = \frac{\vec{\omega}}{\omega}$$

3. The point C on the intantanous axis of rotation:

 $$\vec{r}_C = \vec{r}_P + \frac{\vec{\omega}\times\vec{v}_P}{\vec{\omega}\cdot\vec{\omega}}$$

4. Ratio of angular to linear angular velocity:

 $$ h = \frac{\vec{\omega} \cdot \vec{v}_P}{\vec{\omega}\cdot\vec{\omega}}$$

5. The linear velocity at point $C$ along the screw axis is then given by

 $$\vec{v}_C = h\,\vec{\omega}$$


All we need to know is the rotation velocity of a body and the velocity of any point to find the instantaneous axis of rotation. 

## AnyScript implementation

In AnyScript we can easily find the angular velocity between two reference frames using the class `AnyKinRotational` and the linear velocity using the class `AnyKinLinear`. 

We have created a custom class_template that makes it easy to add all this code to a model and display the instantaneous axis of rotation. Here is a short example on how to use the class. 


The class template is called `InstantaneousAxisOfRotation`.  does exactly this, and then calculate the properties listed above. 




------------------







EXAMPLE:
```c++
#include "<ANYBODY_PATH_TOOLBOX>/Rotations/InstantaneousAxisOfRotation.any"

Main = {


  // This is a simple example of using the InstantaneousAxisOfRotation class
  // to the calculate and vizualize the instantaneous axis of rotation 
  // between Body1 and Body2
  InstantaneousAxisOfRotation IAOR (AnyRefFrame& Body1, AnyRefFrame& Body2 ) = 
  {
      // The sphere drawn at the mid point of the vizualized axis.
      #var AnyVar SphereSize=0.02;
      
      // The length of the vizualized axis
      #var AnyVar AxisLength=2;
  
      // The line thickness of the vizualized axis.
      #var AnyVar LineThickness = 0.005;       

   };
```

The object will IAOR object will plot the axis, and provide the following outputs:

|---------------------|--------------|------------------------------------------------|
| Output variable     | Type         | Desciption                                     |
|---------------------|--------------|------------------------------------------------|
| MidPoint            | AnyVec3      | The mid point on axis in global coordinates    |
| Direction           | AnyVec3      | The direction of the axis in global coordinates|
|---------------------|--------------|------------------------------------------------| 
