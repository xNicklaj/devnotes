The **open-closed** principle states that a class is supposed to be "**open for extension, closed for modification**", meaning that it is possible to extend the behaviour of such entity without knowing its source code.

## Area calculator without Open-Closed design
The classic example to showcase open-closed design is to assume an area calculator that needs to retrieve the area for different types of polygons. Each polygon has a different formula to find the area, and without using some kind of closed inheritance you have to create different functions to calculate the area of different areas.

```c#
public class AreaCalculator
{
	public float GetRectangleArea(Rectangle rectangle)
	{
		return rectangle.width * rectangle.height;
	}
	
	public float GetCircleArea(Circle circle)
	{
		return circle.radius * circle.radius * Mathf.PI;
	}
}

public class Rectangle
{
	public float width;
	public float height;
}

public class Circle
{
	public float radius;
}
```

## Area calculator with Open-Closed design
By using inheritance it's possible to create a Shape abstract class that only defines a method `CalculateArea()`.
The many polygons will inherit from the shape class and will have to implement this function, so that now it's possible to create a simple AreaCalculator that calls the shared `CalculateArea()` function to retrieve the area.

```c#
public abstract class Shape
{
	public abstract float CalculateArea();
}

public class Rectangle : Shape
{
	public float width;
	public float height;
	
	public override float CalculateArea()
	{
		return width * height;
	}
}

public class Circle : Shape
{
	public float radius;
	
	public override float CalculateArea()
	{
		return radius * radius * Mathf.PI;
	}
}

public class AreaCalculator
{
	public float GetArea(Shape shape)
	{
		return shape.CalculateArea();
	}
}
```
