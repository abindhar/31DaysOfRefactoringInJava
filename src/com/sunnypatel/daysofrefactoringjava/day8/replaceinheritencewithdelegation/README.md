All too often inheritance is used in the wrong scenarios. Inheritance should only be used in logical circumstances but it is often used for convenience purposes. I’ve seen this many times and it leads to complex inheritance hierarchies that don’t make sense. Take the following code:

 ```Java
public class Sanitation {

	public String washHands(){
		return "washed!";
	}
}
public class Child extends Sanitation{

}
```

In this instance, a Child is not a “Sanitation” and therefore doesn’t make sense as an inheritance hierarchy. We can refactor by initializing an instance of Sanitation in the Child constructor and delegating the call to the class rather than via inheritance. If you were using Dependency Injection, you would pass in the Sanitation instance via the constructor, although then you would need to register your model in your IoC container which is a smell IMO, you get the idea though. Inheritance should ONLY be used for scenarios where inheritance is warranted. Not instances where it makes it quicker to throw down code.

```Java
public class Sanitation {
	public String washHands() {
		return "washed!";
	}
}

public class Child {
	private Sanitation sanitation;

	public Sanitation getSanitation() {
		return sanitation;
	}

	public void setSanitation(Sanitation sanitation) {
		this.sanitation = sanitation;
	}
	public String washHands(){
		return sanitation.washHands();
	}
}
```

This refactoring comes from Martin Fowlers Refactoring book. You can view the original refactoring [here](https://refactoring.com/catalog/replaceInheritanceWithDelegation.html).

#### Original C# code from the book:
```cs
public class Sanitation
{
    public string WashHands()
    {
        return "Cleaned!";
    }
}

public class Child : Sanitation
{
}
```

```cs
public class Sanitation
{
    public string WashHands()
    {
        return "Cleaned!";
    }
}

public class Child
{
    private Sanitation Sanitation { get; set; }

    public Child()
    {
        Sanitation = new Sanitation();
    }

    public string WashHands()
    {
        return Sanitation.WashHands();
    }
}
```
