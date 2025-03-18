---
tags: 
aliases:
---
The command pattern allows to encapsulate functions in **commands objects** that allows to split the execution of some code from the source input that triggers the execution. Moreover, with the command pattern it's possible to easily implement an Undo/Redo mechanism by saving all the executed commands in a list.

In the words of Robert Nystrom[^citation]:

> [!quote] Definition of Command by Robert Nystrom
> **A command is a reified method call.**
> Let me unpack that a bit. “Reify”, in case you’ve never heard it, means “make real”. Another term for reifying is making something “first-class”.
> Both terms mean taking some _concept_ and turning it into a piece of _data_ — an object — that you can stick in a variable, pass to a function, etc. So by saying the Command pattern is a “reified method call”, what I mean is that it’s a method call wrapped in an object.
## Base Definition
A command can be created by implementing an interface with at least the Execute method. This is where you're supposed to put all the code logic that the command should execute. 
The undo method instead is what you're supposed to call when you want to restore the system to the state prior the execution. Note that there is no need for a redo method, as you can simply re-execute the command.

```csharp
public interface ICommand
{
	public void Execute();
	public void Undo();
}
```

The interface is implemented in the CommandObject, which can contain different parameters depending on what you want it to do. In this case, simply as example we have a string parameters, but you can watch the examples further in the page to check a real case scenario of the command pattern in action.

```csharp
public class CommandObject : ICommand
{
	private string _executionParameters;

	public CommandObject(string parameters)
	{
		_executionParameters = parameters;
	}

	public void Execute() {
		// Perform Command Logic
	}

	public void Undo(){
		// Perform Undo Logic
	}
}
```

All the commands are saved to an **invoker** (or scheduler) that can use either a **queue** (first-in-first-out) or a **stack** (last-in-first-out). This is what will detain the responsibility to execute and undo the command.

```csharp
// FIFO version
public class CommandInvoker
{
	private Queue<ICommand> _commandQueue = new Queue<ICommand>();

	public void ExecuteCommand(){
		if (_commandQueue.Count <= 0) return;
		ICommand command = _commandQueue.Dequeue();
		command.Execute();
	}

	public void AddCommand(ICommand command){
		_commandQueue.Enqueue(command);
	}
}
```

Typically you'll want to use a LIFO version if you need to add an undo functionality so that you can use the stack to hold the command history.

```c#
// LIFO version
public class CommandInvoker
{
	private Stack<ICommand> _commandStack = new Stack<ICommand>(); // Command History

	public void ExecuteCommand(ICommand command){
		command.Execute();
		_commandStack.Push(command);
	}

	public void UndoCommand(){
		if (_commandStack.Count <= 0) return;
		ICommand command = _commandStack.Pop();
		command.Undo();
	}
}
```

## Decoupling Code Execution and Triggering
Let's say we want to make a command that a game allows the user to have multiple player characters, and that such player characters have a moving script.

```c#
public class PlayerMover : MonoBehaviour
{
	public void Move(Vector3 movement)
	{
		transform.position = transform.position + movement;
	}
}
```

By giving the command a reference to which player to move, it's possible to apply the same command to multiple entities.

```c#
public class CommandObject : ICommand
{
	private PlayerMover _playerMover;
	private Vector3 _movingDirection;

	public CommandObject(PlayerMover playerMover, Vector3 movingDirection)
	{
		_playerMover = playerMover;
		_movingDirection = movingDirection;
	}

	public void Execute() {
		_playerMover.Move(_movingDirection);
	}
}
```

But the real magic happens when you realize that whatever is sending the move command doesn't necessarily have to be an InputManager, but it could also be a ad-hoc artificial intelligence or a network command. The only requirement is that whatever script decides to create the command sends a reference to which player to move and what direction it should go towards.
## Undo / Redo Mechanism
In the player script, let's create two methods, one to move the player and one to verify whether the executed move is valid.

```c#
public class PlayerMover : MonoBehaviour
{
	[SerializeField] private LayerMask obstacleLayer;
	private const float boardSpacing = 1f;
	
	public void Move(Vector3 movement)
	{
		transform.position = transform.position + movement;
	}
	
	public bool IsValidMove(Vector3 movement)
	{
		return !Physics.Raycast(transform.position, movement, boardSpacing, obstacleLayer);
	}
}
```

The move command will take in an argument a reference to the player and the vector that represents the direction in which it's supposed to move.

```c#
public class MoveCommand : ICommand
{
	PlayerMover playerMover;
	Vector3 movement;
	
	public MoveCommand(PlayerMover player, Vector3 moveVector)
	{
		this.playerMover = player;
		this.movement = moveVector;
	}
	
	public void Execute()
	{
		playerMover.Move(movement);
	}
	
	public void Undo()
	{
		playerMover.Move(-movement);
	}
}
```

The InputManager won't call directly `PlayerMover.Move()` but an additional layer will check whether the command is valid and if true, run the command.

```c#
private void RunPlayerCommand(PlayerMover playerMover, Vector3 movement)
{
	if (playerMover == null)
	{
		return;
	}
	
	if (playerMover.IsValidMove(movement))
	{
		ICommand command = new MoveCommand(playerMover, movement);
		CommandInvoker.ExecuteCommand(command);
	}
}
```

[^Citation]: [Command Pattern by Robert Nystrom](https://gameprogrammingpatterns.com/command.html)
