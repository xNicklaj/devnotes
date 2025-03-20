---
tags: 
aliases:
---
```csharp
public interface IState
{
	public void Enter()
	{
		// code that runs when we first enter the state
	}
	public void Update()
	{
		// Here we add logic to detect if the conditions exist to
		// transition to another state
	}
	public void Exit()
	{
		// code that runs when we exit the state
	}
}
```

```csharp
[Serializable]
public class StateMachine
{
	public IState CurrentState { get; private set; }
	
	public WalkState walkState;
	public JumpState jumpState;
	public IdleState idleState;
	
	public void Initialize(IState startingState)
	{
		CurrentState = startingState;
		startingState.Enter();
	}
	
	public void TransitionTo(IState nextState)
	{
		CurrentState.Exit();
		CurrentState = nextState;
		nextState.Enter();
	}
	
	public void Update()
	{
		if (CurrentState != null)
		{
			CurrentState.Update();
		}
	}
}
```