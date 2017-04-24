# Class for calculating Instantaneous Axis of Rotation

We have created a custom class_template that makes it easy to calculate and display the intantaneous axis of rotation between two bodies in an AnyBody Model.

![Rolling without slipping](iaor_ball.gif)

Here is a short example on how to use the class. First add the following to the top of your main file.

```c++
#include "<ANYBODY_PATH_TOOLBOX>/Rotations/InstantaneousAxisOfRotation.any"
```

Then add the `InstantaneousAxisOfRotation` class in one of your studies. 

```c++
#include "<ANYBODY_PATH_TOOLBOX>/Rotations/InstantaneousAxisOfRotation.any"

Main = {


  // This is a simple example of using the InstantaneousAxisOfRotation class
  // to the calculate and vizualize the instantaneous axis of rotation 
  // between Body1 and Body2
  InstantaneousAxisOfRotation IAOR (AnyRefFrame& Body1, AnyRefFrame& Body2) = 
  {
      // The sphere drawn at the mid point of the vizualized axis.
      #var AnyVar SphereSize=0.02;
      
      // The length of the vizualized axis
      #var AnyVar AxisLength=2;
  
      // The line thickness of the vizualized axis.
      #var AnyVar LineThickness = 0.005;       

   };
```

## Output

The object will IAOR object will plot the axis, and provide the following outputs:

| Output variable       | Type       | Desciption                                     |
|:----------------------|------------|:-----------------------------------------------|
| RotationAxisDirections| `AnyVec3`  | The mid point on axis in Body1 coordinates     |
| RotationMagnitude     | `AnyVec3`  | The direction of the axis in Body1 coordinates |
| rc_2                  | `AnyVec3`  | Point on axis closest to Body2                 |
| rc_1                  | `AnyVec3`  | Point on axis closest to Body1                 |
| MotionPitch           | `AnyVar`   | The ratio of linear motion to angular motion   |
| rc_dot                | `AnyVec3`  | The linear velocity along the axis of rotation |
| omega                 | `AnyVec3`  | Anuglar velocity vector of Body2 wrt. Body1    |
| r                     | `AnyVec3`  | Position of Body2 wrt. Body1                   |
| r_dot                 | `AnyVec3`  | Linear velocity of Body2 wrt. Body1            |
