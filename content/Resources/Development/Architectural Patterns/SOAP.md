---
tags: 
aliases:
  - Scriptable Objects Architectural Pattern
---
**Scriptable Objects** are a new way for Unity to store data, and are generally used only for static data (Player Stats, Item Stats, Enemy Stats, Settings, etc...) however they can be used for so much more... 

You can use Scriptable Objects to save anything that can be static in the game. Object Pools? Scriptable. Loot Tables? Scriptables. User Settings? Scene Loader? Scriptable.
You can basically replace all [[Singleton|singletons]] with Scriptable Objects.


> [!warning] 
> As useful as they are, what many developers fail to understand is that scriptable data changed in runtime does not persist across sessions. A scriptable's default state is generated when building the game, so if you need your data to persist you'll have to serialize it and save it somehow.


![[Pasted image 20250319214858.png]]

## Controlling GameObjects behaviour via ScriptableEvents
You can insert an **Event** in a scriptable and use it to control the execution order of functions for GameObjects.

![[Pasted image 20250319091349.png]]

This create some sort of **Single Entry Point** without even having to touch code.

## Runtime Variables with ScriptableObjects
It's possible to create different types of variables 
```csharp
[CreateAssetMenu(menuName = "Variables\Int Variable")]
public class IntVariable : ScriptableObject {
	[SerializeField] int _initialValue;
	[SerializeField] int _value;

	public UnityEvent<int> OnValueChanged = delegate {};

	public int Value {
		get => _value;
		set {
			if(this._value == value) return;
			this._value = value;
			OnValueChanged.Invoke(value);
		}
	}
}
```

```csharp
[CustomPropertyDrawer(typeof(IntVariable))]
public class IntVariableDrawer : PropertyDrawer{
	public override VisualElement CreatePropertyGUI(SerializedProperty property){
		VisualElement container = new VisualElement();
		ObjectField objectField = new ObjectField(property.displayName){
			objectType = typeof(IntVariable);
		};
		objectField.BindProperty(property);

		Label valueLabel = new Label();
		valueLabel.style.paddingLeft = 20;

		container.Add(objectField);
		container.Add(valueLabel);

		objectField.RegisterValueChangedCallback(evt => {
			IntVariable variable = evt.newValue as IntVariable;
			if(variable != null){
				valueLabel.text = $"Current Value: {variable.Value}";
				variable.OnValueChanged += newValue => valueLabel.text = $"Current Value: {newValue}";
			} else {
				valueLabel.text = string.Empty;
			}
		
		});


		IntVariable currentVariable = property.objectReferenceValue as IntVariable;
		if(currentVariable != null){
			valueLabel.text = $"Current Value: {currentVariable.Value}";
			variable.OnValueChanged += newValue => valueLabel.text = $"Current Value: {newValue}";
		}


		return container;
	}
}
```

[^1]: [Unity Architecture: Scriptable Objects Pattern](https://medium.com/@simon.nordon/unity-architecture-scriptable-object-pattern-0a6c25b2d741)