```csharp
public interface IProduct
{
	public string ProductName { get; set; }
	public void Initialize();
}
```

```csharp
public abstract class Factory : MonoBehaviour
{
	public abstract IProduct GetProduct(Vector3 position);
	// shared method with all factories
	// Other custom methods
}
```

```csharp
public class ProductA : MonoBehaviour, IProduct
{
	[SerializeField] private string productName = "ProductA";
	public string ProductName { get => productName; set => productName = value ; }
	private ParticleSystem particleSystem;
	
	public void Initialize()
	{
	// any unique logic to this product
		gameObject.name = productName;
		particleSystem = GetComponentInChildren<ParticleSystem>();
		particleSystem?.Stop();
		particleSystem?.Play();
	}
}
```

```csharp
public class ConcreteFactoryA : Factory
{
	[SerializeField] private ProductA productPrefab;
	public override IProduct GetProduct(Vector3 position)
	{
		// create a Prefab instance and get the product component
		GameObject instance = Instantiate(productPrefab.gameObject, position, Quaternion.identity);
		ProductA newProduct = instance.GetComponent<ProductA>();
		// each product contains its own logic
		newProduct.Initialize();
		return newProduct;
	}
}
```