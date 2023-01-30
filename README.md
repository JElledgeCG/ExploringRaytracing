# Exploring Raytracing

### My short description goes here

<div>
    <table width = 100%>
        <tr>
            <td width = 30%>
                <ul>
                    <div><img src="icons/gmail.png" width = "16" height = "16"></img> JElledgeCG@gmail.com</div>
                    <div><img src="icons/twitter.png" width = "16" height = "16"></img> @JElledgeCG</div>
                    <div><img src="icons/website.png" width = "16" height = "16"></img> Github Pages</div>
                    <div><img src="icons/discord.png" width = "16" height = "16"></img> Josh E#6435</div>
                    <div><img src="icons/linkedin.png" width = "16" height = "16"></img> LinkedIn</div>
                </ul>
            </td>
            <td width = 70%>
                <img src="icons/banner.png"></img>
            </td>
        </tr>
    </table>
</div>

<p align="left">
  <a href="#introduction">Introduction</a> •
  <a href="#part-i-ray-tracing-in-one-weekend">PART I: Ray Tracing in One Weekend</a> •
  <a href="#part-ii">Part II</a> •
  <a href="#credits">Credits</a>
</p>

## Introduction

Welcome to Exploring Raytracing! As part of my computer graphics journey, I've decided to dive into offline raytracing as a starting point for understanding PBR. This repo serves as a devlog and progress tracker where I can store code, thoughts about what I'm learning, and plans for the future of this project!

## PART I: Ray Tracing in One Weekend

To begin, I decided to follow along with the book "Ray Tracing in One Weekend" by Peter Shirley and build a raytracer from scratch in C++. This resource was very useful and taught me many things. Much of the code in this repo comes directly from this source (and has been marked as such), but I still learned enough to understand the concepts and algorithms and to be inspired to extend the renderer (see Part II). Below are some key learnings and thoughts that I had while following along:

### Rays

The early chapters of "Ray Tracing in One Weekend" immediately showcase one of the benefits of raytracing over rasterization: *SIMPLICITY*. For each pixel in the target output, a ray is cast from the camera through pixels in a virtual viewport and into the scene. The function performing the raycast returns a color for the pixel based on what the ray collided with, if anything. 

In order to determine whether a ray collides with anything (and what it collides with), each ray must be tested against each object in the scene for intersection (for now). The raytracer that I ended up with after completing chapter 6 introduced a test for ray-sphere intersection. This test involves clever algebra that leads to a quadratic equation with unknown t, where t is the distance along the ray where it intersects with the target sphere. Calculating the discriminant of this equation leads to an easy intersection test - if the discriminant is greater than 0, the sphere will be hit; otherwise, it will not. This led to the following code for sphere intersection and raycasting:

```
double hit_sphere(const point3& center, double radius, const ray& r) {
    vec3 oc = r.origin() - center;
    auto a = dot(r.direction(), r.direction());
    auto b = 2.0 * dot(oc, r.direction());
    auto c = dot(oc, oc) - radius*radius;
    auto discriminant = b*b - 4*a*c;

    if (discriminant < 0) {
        return -1.0;
    } else {
        return (-b - sqrt(discriminant) ) / (2.0*a);
    }
}

color ray_color(const ray& r) {
    auto t = hit_sphere(point3(0,0,-1), 0.5, r);
    if (t > 0.0) {
        vec3 N = unit_vector(r.at(t) - vec3(0,0,-1));
        return 0.5*color(N.x()+1, N.y()+1, N.z()+1);
    }
    vec3 unit_direction = unit_vector(r.direction());
    t = 0.5*(unit_direction.y() + 1.0);
    return (1.0-t)*color(1.0, 1.0, 1.0) + t*color(0.5, 0.7, 1.0);
}
```

This code models basic raytracing of a sphere, using the surface normal of the sphere at the location hit by each ray as the returned ray color:

<img src="images/PNGs/normals.png" width = 150% height = 150%></img>

### Antialiasing and Blur

Chapter 7 describes antialiasing for our raytracing algorithm. The idea is very straightforward and was very interesting to learn about: For each pixel, rather than send one ray into the center of the pixel, send several rays, each with a random offset within some range. Making this range 0 results in no antialiasing. Going within a 1 pixel range in both the x and y direction results in  antialiasing that removes the terrible jagged edges from the previous image:

<img src="images/PNGs/normals_AA.png"></img>

I started experimenting with this, and realized that by increasing the range that the random offset can be generated (I called this "blur factor" in my code) results in a very convincing and very simple blur:

<img src="images/PNGs/normals_blur.png" width = 49.5%></img>
<img src="images/PNGs/normals_blur_EX.png" width = 49.5%></img>

*Left: 200 samples per pixel, blur factor 10. Right: 200 samples per pixel, blur factor 100.

### Materials: Diffuse



## Part II

Another section

## Credits

Credit Section
