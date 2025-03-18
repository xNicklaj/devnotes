The Interface Segregation Principle (ISP) states that a class should not depend on methods it doesn't use.

> [!quote] 
> In other words, **avoid large interfaces**[^1]. Follow the same idea as the single responsibility principle, which tells you to **keep classes and methods short**. This gives you maximum flexibility, keeping interfaces compact and focused.

## Distructable prop
Let's say you have an RTS with multiple units that all have the usual entity components, health, damage, attack stats, defense stats. You will make all your units implement this interface.
```c#
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
Now let's say you want a breakable barrel. It is not a unit, however it will need to have the concept of *health*. Having it implement `IUnitStats` will break the ISP so we can divide our interface in different interfaces that will each implement one and only one part of our unit


[^1]: [Level up your code with game programming patterns](https://unity.com/resources/level-up-your-code-with-game-programming-patterns)
