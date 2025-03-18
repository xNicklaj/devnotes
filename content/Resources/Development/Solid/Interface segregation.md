---
tags: 
aliases:
  - Interface Segregation Principle
---
The Interface Segregation Principle (ISP) states that a class should not depend on methods it doesn't use.

> [!quote] 
> In other words, **avoid large interfaces**[^1]. Follow the same idea as the single responsibility principle, which tells you to **keep classes and methods short**. This gives you maximum flexibility, keeping interfaces compact and focused.

Many times this translates to **favor composition over inheritance**, similarly to what you can see with the [[Liskov Sustitution|Liskov Susbtitution Principle]].

> [!tip] In Short
> Keep your interfaces short with a few methods. Clients only implement what they need.
## Distructable Prop
Let's say you have an RTS with multiple units that all have the usual entity components, health, damage, attack stats, defense stats. You will make all your units implement this interface.
```csharp
public interface IUnitStats
{
	public float Health { get; set; }
	public int Defense { get; set; }
	
	public void Die();
	public void TakeDamage();
	public void RestoreHealth();
	
	public float MoveSpeed { get; set; }
	public float Acceleration { get; set; }
	
	public void GoForward();
	public void Reverse();
	public void TurnLeft();
	public void TurnRight();
	
	public int Strength { get; set; }
	public int Dexterity { get; set; }
	public int Endurance { get; set; }
}
```
Now let's say you want a breakable barrel. It is not a unit, however it will need to have the concept of *health*. Having it implement `IUnitStats` will break the ISP so we can divide our interface in different interfaces that will each implement one and only one part of our unit.
```csharp
public interface IMovable
{
	public float MoveSpeed { get; set; }
	public float Acceleration { get; set; }
	public void GoForward();
	public void Reverse();
	public void TurnLeft();
	public void TurnRight();
}

public interface IDamageable
{
	public float Health { get; set; }
	public int Defense { get; set; }
	public void Die();
	public void TakeDamage();
	public void RestoreHealth();
}

public interface IUnitStats
{
	public int Strength { get; set; }
	public int Dexterity { get; set; }
	public int Endurance { get; set; }
}
```
Finally we can add an `IExplodable` for the barrel:
```csharp
public interface IExplodable
{
	public float Mass { get; set; }
	public float ExplosiveForce { get; set; }
	public float FuseDelay { get; set; }
	public void Explode();
}
```
By dividing our code into different interfaces, similarly to what the [[Single Responsibility|Single Responsibility Principle]] states we are achieving the same set of features, while being more flexible on how entities will implement those features.
The final UML diagram looks like this:
![[Pasted image 20250318094216.png|800]]


[^1]: [Level up your code with game programming patterns](https://unity.com/resources/level-up-your-code-with-game-programming-patterns)
