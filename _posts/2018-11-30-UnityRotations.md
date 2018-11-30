---
title: "Rotations in 3D space"
layout: post
excerpt_separator: <!--more-->
---

I recently started working in Unity and started learning 3D graphics. Starting simple, I tried to make a turret rotate on its y- and z- axis. Getting the turret to rotate around the y-axis was easy, with a simple rotation. 

<!--more-->
```csharp
// Mouse Moves Side to side
float rotY = Input.GetAxis("Mouse X") * speed * Time.deltaTime;     
Vector3 rotateValue = new Vector3(0, rotY, 0);
transform.eulerAngles = transform.eulerAngles + rotateValue;
```

Things got tricky when trying to tilt the turret up and down. As you can see from the code, I tried to rotate the object on the y- and z-axis. Inadvertently, I also changed the x-axis rotation. How was that happening if I am not telling the object to rotate on the x-axis?

![](https://i.stack.imgur.com/A9nOC.gif)

After some quick googling, I found this great [stack overflow post](https://gamedev.stackexchange.com/questions/136174/im-rotating-an-object-on-two-axes-so-why-does-it-keep-twisting-around-the-thir). There is interesting information about how rotations are not communitive, but the part helped me understand the problem was understanding relationship of rotations.

When I rotated the turret, I rotated it along the objects y-axis. There is nothing wrong with this approach, but I misunderstood a critical component: If I move the object on either the x- or z-axis, the y-axis is no longer perpendicular to the ground. So now when I rotated the y-axis, I rotated on a y-axis that was no longer perpendicular with the ground. If I rotated the z-axis, my camera looked like I was looking sideways.

![](https://i.stack.imgur.com/dadM6.gif)
![](https://i.stack.imgur.com/QJRYD.gif)

I found there are two solutions to the problem, both of which come from the stack exchange post.

The first is what you saw in the next GIF: Pitch locally, yaw globally. This means that we pitch our object along it’s local axis and yaw on the global axis. How does this help us? If we yaw on the global axis, we rotate on the axis that is perpendicular to the ground (assuming that is what we want).

```csharp
// Mouse Moves side to side
float rotY = Input.GetAxis("Mouse X") * speed * Time.deltaTime;     
// Mouse Moves up and down
float rotZ = Input.GetAxis("Mouse Y") * speed * Time.deltaTime;     
Vector3 rotateValue = new Vector3(0, rotY, rotZ);
transform.eulerAngles = transform.eulerAngles + rotateValue;
```

The solution that worked best for me was to apply a total net rotation to the transform object. By setting the rotation I controlled the rotation on the x-axis, essentially “locking” the x-axis. This is similar to the previous solution of locking the “y” axis. Now the x-axis shouldn’t be able to change.

