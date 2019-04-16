---
title: "Unity Day Night Cycle"
layout: post
excerpt_separator: <!--more-->
---

While working on my side project, I wanted to make a Day Night Cycle system. I created a super rough one but it was amazing how easy Unity made it. To do it, all you will need are a few things

<!--more-->

1.	A Light game object
2.	Transform.LookAt(Vector3 worldPosition)
3.	Some Handy Dandy maths

Let’s look at why we need these

### Light Game Object

This is pretty self-explanitory. You need some kind of light in order to make a day night cycle. In this case we will need a directional light. This will give us a light direction much like what we would experience like with sun light in reality.

### Transform.LookAt(Vector3 worldPosition)

We are going medieval and having the sun revolve around the earth. In this case the light game object will be the object moving across the sky (will be discussed shortly). Because it is directional light, it doesn’t matter where the light object is placed. Light direction is dependent on the rotation. As the light moves across the sky, we will have the object look at the origin point (0, 0, 0). This will have the sun point in the right direction when sunset, sunrise, afternoon, ect.

In theory you could just rotate the sun rather than moving it _and_ rotating it. However, I haven’t been able to get the math right doing that, and moving the light object then doing a rotation seems to have the desired affect

### Handy Dandy Maths

Next comes the cool part.

Like  previously mentioned, we need the sun to move through the sky. For simplicity sake, we move the sun in a giant circle. Sin() and Cos() to the rescue. I didn’t know I was going to be using trig outside of high school.

Because we want the sun going in a circle, we need to change 2 axis, in our case the x and y. Y will represent the height in sky and X will represent our horizons. So when Y is 1 and x is 0, it is noon, and when Y is 0 and X is 1, it is dusk (I think, it doesn't really matter. It is is the horizon). This is where Sin() and Cos() come into play. 

First, we will need some count value that we can increment. In this case, we can easily increment with Time.deltaTime. We will apply Cos(count) to the Y and Sin(count) to our X (if you’re wondering why this specifically, the game will start in morning by default). 

Now to put it all together. I am placing a sample script below

```
public class SunMovement : MonoBehaviour
{
    private float currentTime = 0;
    private const float speed = .5f;
    private float startY = 100;
    private float startX = 100;
    // Use this for initialization
    void Start()
    {
        
    }

    // Update is called once per frame
    void Update()
    {
        // Update counter
        currentTime += (Time.deltaTime * speed) % 1.0f;

        // Update position
        this.transform.position = new Vector3(Mathf.Cos(currentTime) * startX, Mathf.Sin(currentTime) * startY, 0);

        // Make sure to look at origin
        this.transform.LookAt(Vector3.zero);
    }
}
```
    
There you go! As we change the position, we update the look at vector to make sure that we are always looking at the origin. When light goes below the world, it acts like night time so you could in theory put some stars and make it look really cool! 
Again, like I mentioned previously, there might be a way to have the light object rotate rather than having to do all this. I tried doing the line below.

`this.transform.rotation = Quaternion.Euler(new Vector3(Mathf.Cos(count), Mathf.Sin(count), 0))`

Sadly, it didn’t work. There would be some weird thing happening when the light was supposed to be at noon. I’ll keep working on it and try to update this post if I manage to figure it out.

Hopefully you learned how cool math could be. That is pretty much why I wrote this.
