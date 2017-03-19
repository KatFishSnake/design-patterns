## Personal take, on design-patterns-for-humans

Inspired by and forked from [design-patterns-for-humans](https://github.com/kamranahmedse/design-patterns-for-humans)

----

# Creational Design Patterns

### Simple factory

*Simple factory simply generates an instance for client without exposing any instantiation logic to the client*

**e.g** Consider, you are building a house and you need doors. It would be a mess if every time you need a door, you put on your carpenter clothes and start making a door in your house. Instead you get it made from a factory.

~~~js
class WoodenDoor {}
class SteelDoor {}

class DoorFactory {
	getWoodenDoor() { return new WoodenDoor() }
	getSteelDoor() {  return new SteelDoor()  }
}
~~~

> **Use:** When creating an object is not just a few assignments and involves some logic, it makes sense to put it in a dedicated factory instead of repeating the same code everywhere.




### Factory

*It provides a way to delegate the instantiation logic to child classes.*

**e.g** Consider the case of a hiring manager. It is impossible for one person to interview for each of the positions. Based on the job opening, she has to decide and delegate the interview steps to different people.

~~~js
class Developer {
	askQuestions() {}
}

class HiringManager {
	takeInterview() { 
		const interviewer = this.makeInterviewer()
		interviewer.askQuestions()
	}
}

class SeniorDeveloper extends HiringManager {
	constructor() { super() }
	makeInterviewer() { return new Developer() }
}

const devManager = new SeniorDeveloper()
devManager.takeInterview()
~~~

> **Use:** When there is some generic processing in a class but the required sub-class is dynamically decided at runtime. Or putting it in other words, when the client doesn't know what exact sub-class it might need.




### Abstract Factory

*A factory of factories; a factory that groups the individual but related/dependent factories together without specifying their concrete classes.*

**e.g** Extending our door example from Simple Factory. Based on your needs you might get a wooden door from a wooden door shop, iron door from an iron shop or a PVC door from the relevant shop. Plus you might need a guy with different kind of specialities to fit the door, for example a carpenter for wooden door, welder for iron door etc. As you can see there is a dependency between the doors now, wooden door needs carpenter, iron door needs a welder etc.

~~~js
class IronDoor {}
class Welder {}

class DoorShop {
	makeDoor()  { return new IronDoor()  }
	getExpert() { return new Welder()    }
}
~~~

> **Use:** When there are interrelated dependencies with not-that-simple creation logic involved




### Builder

*Allows you to create different flavors of an object while avoiding constructor pollution. Useful when there could be several flavors of an object. Or when there are a lot of steps involved in creation of an object.*

**e.g** Imagine you are at Hardee's and you order a specific deal, lets say, "Big Hardee" and they hand it over to you without any questions; this is the example of simple factory. But there are cases when the creation logic might involve more steps. For example you want a customized Subway deal, you have several options in how your burger is made e.g what bread do you want? what types of sauces would you like? What cheese would you want? etc. In such cases builder pattern comes to the rescue.

~~~js
class Burger {
	constructor (builder) {
		this.size = builder.size
		this.cheese = builder.cheese
		this.tomato = builder.tomato
	}
}

class Builder {
	constructor(size = 1) {
		this.size = size
		
		this.cheese = false
		this.tomato = false
	}
	
	addCheese() { return { ...this, cheese: true } }
	addTomato() { return { ...this, tomato: true } }
	
	build() { return new Burger(this) }
}
~~~

> **Use:** When there could be several flavors of an object and to avoid the constructor telescoping. The key difference from the factory pattern is that; factory pattern is to be used when the creation is a one step process while builder pattern is to be used when the creation is a multi step process.




### Prototype

*Create object based on an existing object through cloning.*

**e.g** Remember dolly? The sheep that was cloned! Lets not get into the details but the key point here is that it is all about cloning

~~~js
class Sheep {
	constructor(name, category = 'Mountain Sheep') {
		this.name = name
		this.category = category
	}
	
	setName(name) { this.name = name }
	getName() { return this.name }
	getCategory () { return this.category } 
}

const original = new Sheep('Jolly')

const cloned = { ...original }
cloned.setName('Dolly')
~~~

> When an object is required that is similar to existing object or when the creation would be expensive as compared to cloning.




### Singleton

*Ensures that only one object of a particular class is ever created.*

**e.g** There can only be one president of a country at a time. The same president has to be brought to action, whenever duty calls. President here is singleton.

~~~js
let instance;
class President {
	constructor() {
		if(!instance) {
			instance = this;
		}
		return instance;
	}
}
~~~

> This is useful when exactly one object is needed to coordinate actions across the system.




