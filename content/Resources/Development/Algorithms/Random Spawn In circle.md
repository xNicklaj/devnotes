For a circle of radius `R` you do:

```c
double a = random() * 2 * PI
double r = R * sqrt(random())

// If you need it in Cartesian coordinates
double x = r * cos(a)
double y = r * sin(a)
```

where `random()` gives a uniformly random number between 0 and 1.

![[Pasted image 20250323101131.png]]

Source: [Generating a random point within a circle (uniformly)](https://programming.guide/random-point-within-circle.html)