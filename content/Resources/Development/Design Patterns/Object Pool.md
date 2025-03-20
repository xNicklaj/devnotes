When a new GameObject is created or destroyed, the CPU and especially the Garbage Collector are put under heavy duty. When instantiating one gameobject every once in a while it's not an issue, but when instantiating multiple gameobjects all at once very often it can lead to **stutter**.

Object Pooling is a technique where many instances of an object are created (and deactivated) all at once and then are kept alive for the rest of the pool's lifecycle. Whenever a new instance is required, instead of re-instantiating one, you can ask the pool to activate one of the instances and return it. This leads to **higher average memory** consumption, but **less stutter**, hence a more stable frametime.

## Definition
Let's start by creating the `PooledObject` that will be contained by the pool.

```csharp
public class PooledObject : MonoBehaviour
{
	private ObjectPool pool;
	public ObjectPool Pool { get => pool; set => pool = value; }
	
	public void Release()
	{
		pool.ReturnToPool(this);
	}
}
```

Now on to the `ObjectPool`. 
This object will have an initial size, the number of `PooledObject` to instantiate on start, and a reference to the prefab that will be the `PooledObject`.

```csharp
public class ObjectPool : MonoBehaviour
{
	[SerializeField] private uint initPoolSize;
	[SerializeField] private PooledObject objectToPool;
	
	// store the pooled objects in a collection
	private Stack<PooledObject> stack;
	
	private void Start()
	{
		SetupPool();
	}
	
	// creates the pool (invoke when the lag is not noticeable)
	private void SetupPool()
	{
		stack = new Stack<PooledObject>();
		PooledObject instance = null;
		for (int i = 0; i < initPoolSize; i++)
		{
			instance = Instantiate(objectToPool);
			instance.Pool = this;
			instance.gameObject.SetActive(false);
			stack.Push(instance);
		}
	}
	
	// returns the first active GameObject from the pool
	public PooledObject GetPooledObject()
	{
		// if the pool is not large enough, instantiate a new PooledObjects
		if (stack.Count == 0)
		{
			PooledObject newInstance = Instantiate(objectToPool);
			newInstance.Pool = this;
			return newInstance;
		}
		// otherwise, just grab the next one from the list
		PooledObject nextInstance = stack.Pop();
		nextInstance.gameObject.SetActive(true);
		return nextInstance;
	}
	
	public void ReturnToPool(PooledObject pooledObject)
	{
		stack.Push(pooledObject);
		pooledObject.gameObject.SetActive(false);
	}
}
```

To get a new `PooledObject` you just need to call `GetPooledObject()`, and it will return the first available Object in the stack, or create a new one if none is available.
A variation of the ObjectPool also includes a cap for maximum poolable objects that prevents it from generating more objects when none is available.

## UnityEngine Pool
This behaviour is used so much that Unity has its own implementation of the `ObjectPool` via the [**IObjectPool**](https://docs.unity3d.com/6000.0/Documentation/ScriptReference/Pool.ObjectPool_1.html).

