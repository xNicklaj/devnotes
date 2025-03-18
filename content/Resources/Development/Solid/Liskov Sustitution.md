Liskov Sustitution (LSP) states that **a derived class should be substitutable to its base class without breaking the code**. 

Some criterias that help you understand whether you're breaking Liskov Substitution are:
- When you remove a feature in a subclass then likely you are breaking the LSP. Dead giveaways of this are blank methods or NotImplementedExceptions.
- Logic-rich base classes. The more logic your base classes have, the more likely you will break LSP when subclassing.
- A subclass that doesn't have the same public members as the base class is likely breaking LSP.

To help in this task, it's very useful to use **composition over inheritance** by creating a class or interface that encapsulates a certain part of the logic component and then build a composition of elements.

>[!tip] Composition over inheritance
>Whenever in doubt whether to use composition or inheritance, keep in mind that these two techniques are answering two different questions. 
>When pondering about inheritance, ask your self **is** `Man` **a** `Human`?
>Instead when pondering about composition, ask your self **is** `Arm` **part of a** `Human`?
>Moreover keep in mind that our knowledge of the world sometimes leads us astray when creating Class APIs, as you will see later with the train example.

## 