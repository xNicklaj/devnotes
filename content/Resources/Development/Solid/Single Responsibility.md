The Single Responsibility principle states that a class should only have one responsibility, or one reason to change. 
When a class has too many reasons to change then it tends to become a mess, so it's best to avoid monolithic class designs.

Similarly, each function should encapsulate only one piece of logic.

>[!attention] Do not oversimply your designs
>While simplifying classes and methods is always very useful, oversimplifying a structure can lead to unmeaningful results. As example you should avoid having classes that only have one method inside, unless it's strictly required (eg. partial / abstract classes).

> [!quote] Simple is not easy
> Many of the design patterns and principles help you enforce simplicity. 
> In doing so, they make your code more scalable, flexible, and readable. However, they require some extra work and planning. 
> 
> "Simple” does not equate to “easy".[^citation]
> 
> Though you can create the same functionality without the patterns (and often more quickly), something fast and easy doesn’t necessarily result in something simple. 
> Making something simple means making it focused. Design it to do one thing, and don’t overcomplicate it with other tasks.
## Monolithic Class Design
Here's what a monolithic class design looks like. This class is supposed to *read* the player input, *move* the player by a specific amount each frame and whenever a bounce happens *play* and AudioClip. However, all these behaviours are included in a single monolithic class that make it harder than it should to read and comprehend.

```c#
public class UnrefactoredPlayer : MonoBehaviour
{
	[SerializeField] private string inputAxisName;
	[SerializeField] private float positionMultiplier;
	private float yPosition;
	private AudioSource bounceSfx;
	
	private void Start()
	{
		bounceSfx = GetComponent<AudioSource>();
	}
	private void Update()
	{
		float delta = Input.GetAxis(inputAxisName) * Time.deltaTime;
		yPosition = Mathf.Clamp(yPosition + delta, -1, 1);
		transform.position = new Vector3(transform.position.x, yPosition * positionMultiplier, transform.position.z);
	}
	
	private void OnTriggerEnter(Collider other)
	{
		bounceSfx.Play();
	}
}
```
## Proper Class Design
Compared to the previous example, this Player class uses **composition** to simplify its code and by just glancing the name of the components it's pretty easy to understand what each of those components is doing.

```c#
[RequireComponent(typeof(PlayerAudio), typeof(PlayerInput), typeof(PlayerMovement))]
public class Player : MonoBehaviour
{
	[SerializeField] private PlayerAudio playerAudio;
	[SerializeField] private PlayerInput playerInput;
	[SerializeField] private PlayerMovement playerMovement;
	
	private void Start()
	{
		playerAudio = GetComponent<PlayerAudio>();
		playerInput = GetComponent<PlayerInput>();
		playerMovement = GetComponent<PlayerMovement>();
	}
}

public class PlayerAudio : MonoBehaviour
{
// Audio logic
}

public class PlayerInput : MonoBehaviour
{
// Input logic
}

public class PlayerMovement : MonoBehaviour
{
// Movmement logic
}
```

![[Pasted image 20250317222722.png|800]]

[^Citation]: [Level up your code with game programming patterns](https://unity.com/resources/level-up-your-code-with-game-programming-patterns)