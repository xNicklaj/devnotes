---
tags: 
aliases:
---
```c#
public interface ICommand
{
	public void Execute();
}
```

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
}
```

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

Additional resources:
- [Command Pattern by Robert Nystrom](https://gameprogrammingpatterns.com/command.html)