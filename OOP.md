### Objectives
- Define what OOP (Object Oriented Programming) is
- Revisit the `new` keyword and understand the four things it does
- Use constructor functions to reduce duplication in our code
- Use call and apply to refactor constructor functions


### OOP Defined
- A programming model based around the idea of objects
- These objects are constructed from what are called "classes", which we can think of like a blueprint. We call these objects created from classes "instances"
- We strive to make our classes abstract and modular

### Object Creation 
Imagine we want to make a few house objects, they will all have bedrooms, bathrooms, and numSqft

```

var house = {
    bedrooms: 2,
    bathrooms: 2
    sqFeet: 1000
}

var house2 = {
    bedrooms: 2,
    bathrooms: 2
    sqFeet: 1000
}

var house3 = {
    bedrooms: 2,
    bathrooms: 2
    sqFeet: 1000
}

var house4 = {
    bedrooms: 2,
    bathrooms: 2
    sqFeet: 1000
}

```
// woof...imagine if we had to make 100 of these!


### A Solution
Instead of making an infinite number of different objects, let's see if we can create a function to construct these similar "house" objects.

### Constructor Functions

Let's use a function as a blueprint for what each house should be - we call these kinds of functions "constructor" functions

```
function House(bedrooms, bathrooms, numSqft){
    this.bedrooms = bedrooms;
    this.bathrooms = bathrooms;
    this.numSqft = numSqft;
}
```

Notice a few things...

- Capitalization of the function name - this is convention!
- The keyword 'this' is back!
- We are attaching properties onto the keyword 'this'. We would like the keyword 'this' to refer to the object we will create from our constructor function, how might we do that?

### Creating an object
So how do we use our constructor to actually construct objects?

```
function House(bedrooms, bathrooms, numSqft){
    this.bedrooms = bedrooms;
    this.bathrooms = bathrooms;
    this.numSqft = numSqft;
}
```

```
var firstHouse = House(2,2,1000) // does this work?
firstHouse // undefined...guess not!
```


- We are not returning anything from the function so our House function returns undefined
- We are not explicitly binding the keyword 'this' or placing it inside a declared object. This means the value of the keyword 'this' will be the global object, which is not what we want!


### The 'new' keyword
Our solution to the problem!

```
function House(bedrooms, bathrooms, numSqft){
    this.bedrooms = bedrooms;
    this.bathrooms = bathrooms;
    this.numSqft = numSqft;
}
```
```
var firstHouse = new House(2,2,1000) 
firstHouse.bedrooms // 2
firstHouse.bathrooms // 2
firstHouse.numSqft // 1000
```

So what does the new keyword do? A lot more than we might think...

- It first creates an empty object
- It then sets the keyword 'this' to be that empty object
- It adds the line `return this` to the end of the function, which follows it
- It adds a property onto the empty object called "__proto__"(dunder proto), which links the prototype property on the constructor function to the empty object (more on this later)

### quiz

Create a constructor function for a Dog - each dog should have a name and an age. As a bonus, add a function for each dog called 'bark', which console.log's the name of the dog added to the string 'just barked!'

// Your constructor function goes here

```
function Dog(name, age) {
	this.name = name;
	this.age = age;
	this.bark = function() {
		console.log(this.name + " just barked!");
	}
}
```
// this code should work if you have implemented it correctly
```
var rusty = new Dog('Rusty', 3); 
var fido = new Dog('Fido', 1);

rusty.bark() // Rusty just barked!
fido.bark() // Fido just barked!

```

### Multiple Constructors

Let's create two constructor functions, one for a Car and one for a Motorcycle - here is what it might look like
```
function Car(make, model, year){
    this.make = make;
    this.model = model;
    this.year = year;
    // we can also set properties on the keyword this
    // that are preset values
    this.numWheels = 4;
}

function Motorcycle(make, model, year){
    this.make = make;
    this.model = model;
    this.year = year;
    this.numWheels = 2;
}
```

Notice how much duplication is going on in the Motorcycle function. 
Is there any way to "borrow" the Car function and invoke it inside the Motorcycle function?

### Solution 
Using call/apply
We can refactor our code quite a bit using call + apply


```

function Car(make, model, year){
    this.make = make;
    this.model = model;
    this.year = year;
    // we can also set properties on the keyword this
    // that are preset values
    this.numWheels = 4;
}

function Motorcycle(make, model, year){
	Car.call(this, make, model, year);
	this.numWheels = 2;
}
```

Or
```
function Motorcycle(make, model, year){
	Car.apply(this, [make, model, year]);
	this.numWheels = 2;
}
```

Or
```
function Motorcycle(make, model, year){
	Car.apply(this, arguments);
	this.numWheels = 2;
}

```

### Recap
- Object Oriented Programming is a model based on objects constructed from a blueprint. We use OOP to write more modular and shareable code
- In languages that have built-in support for OOP, we call these blueprints "classes" and the objects created from them "instances"
- Since we do not have built-in class support in JavaScript, we mimic classes by using functions. These constructor functions create objects through the use of the new keyword
- We can avoid duplication in multiple constructor functions by using call or apply



