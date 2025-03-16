---
tags: 
aliases:
---
The command pattern allows to encapsulate functions in **commands objects** that allows to split the execution of some code from the source input that triggers the execution. Moreover, with the command pattern it's possible to easily implement an Undo/Redo mechanism by saving all the executed commands in a list.

## Base Definition
A command can be created by implementing an interface with at least the Execute method. This is where you're supposed to put all the code logic that the command should execute. 
The undo method instead is what you're supposed to call when you want to restore the system to the state prior the execution. Note that there is no need for a redo method, as you can simply re-execute the command.

```c#
public interface ICommand
{
	public void Execute();
	public void Undo();
}
```

The interface is implemented in the CommandObject, which can contain different parameters depending on what you want it to do. In this case, simply as example we have a string parameters, but you can watch the examples further in the page to check a real case scenario of the command pattern in action.

```c#
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

All the commands are saved to an **invoker** (or scheduler) that can use either a **queue** (first-in-first-out) or a **stack** (last-in-first-out). This is what will detain the responsibility to execute 

```c#
// FIFO version
public class CommandInvoker
{
	private Queue<ICommand> _commandQueue = new Queue<ICommand>();

	public void ExecuteCommand(){
		ICommand command = _commandQueue.Dequeue();
		command.Execute();
	}

	public void AddCommand(ICommand command){
		_commandQueue.Enqueue(command);
	}
}
```

```c#
// LIFO version
public class CommandInvoker
{
	private Stack<ICommand> _commandStack = new Stack<ICommand>();

	public void ExecuteCommand(){
		ICommand command = _commandStack.Pop();
		command.Execute();
	}

	public void AddCommand(ICommand command){
		_commandStack.Push(command);
	}
}
```

## Decoupling code execution and triggering


## Undo / Redo mechanism




Additional resources:
- [Command Pattern by Robert Nystrom](https://gameprogrammingpatterns.com/command.html)