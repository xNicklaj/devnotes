---
tags: 
aliases:
  - Dependency Inversion Principle
---
The dependency inversion principle states that **high level modules should never depend directly on low level modules, but rather depend on abstractions**.
But what does this really mean? What are High Level modules? What are Low Level modules?

High Level modules are scripts that define the logic flow of an application, that define what should be done rather than how it should be done, hence these should never depend directly on how these tasks are implemented.
Low Level modules instead define the specific implementation of a task.

Generally you want to make sure that your code has as few direct dependencies as possible, as those are generally strong links that go against flexibility.
![[Pasted image 20250318130221.png|600]]
## Switch & Door example
Say that we have a door that should be opened from a switch, the easiest thing to do would be to expose a Open/Close function from the `Door` object, and make the `Switch` depend on `Door` with a function that checks the state of door and toggles between these two functions, a `Toggle`.
![[Pasted image 20250318131041.png]]
But what if you want the switch to enable more than just a door? Then you'll have to either make a new switch script that depends on a new object type, or find a way to go around it. The simple, yet very effective thing to do, which is what this principle is trying to teach is to use **abstraction**.

Let's define an `ISwitchable` interface that exposes its state, a method to `Activate` and a method to `Deactivate`.
```csharp
public interface ISwitchable
{
	public bool IsActive { get; }
	public void Activate();
	public void Deactivate();
}
```
Now we can make `Door` implement an `ISwitchable` and 

```csharp
public class Door : MonoBehaviour, ISwitchable
{
	private bool isActive;
	public bool IsActive => isActive;
	
	public void Activate()
	{
		isActive = true;
		Debug.Log("The door is open.");
	}
	
	public void Deactivate()
	{
		isActive = false;
		Debug.Log("The door is closed.");
	}
}

public class Switch : MonoBehaviour
{
	public ISwitchable client;
	
	public void Toggle()
	{
		if (client.IsActive)
		{
			client.Deactivate();
		} else
		{
			client.Activate();
		}
	}
}
```