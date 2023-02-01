# Solid Principles in GO
Original Article: https://towardsdev.com/golang-solid-principles-e7641dee90b0

# What is SOLID?
Taking as a referece the book “Clean Architecture” by Robert C. Martin we can say the following:

Good software system being with clean code. On the one hand, if the bricks aren’t well made, the architecture of the building doesn’t matter much. On the other hand, you can make a substantial mess with well-made brick. This is where the SOLID principles come in.

The SOLID principles tell us how to arrange our functions and data, and the goal of the principles is the creation of mind-level software structures that:

- Tolerate changes
- Are easy to understand
- Are the basis of components that could be used in many software systems

Well, now we have an overview of SOLID, and now I going to tell you about each one of the principles that form part of SOLID, those are the following:

## Single responsability:
“A module should have one, and only one reason to change”

## Open-Close principle:
“A software artifact should be open for extension but closed for modifications”

## Liskov substitution principle
“What is wanted here is something like the following substitution property: If for each object o1 of type S there is an object o2 of type T such that for all programs P defined in terms of T, the behavior of P is unchanged when o1 is substituted for o2 then S is a subtype of T”

## Interface segregation
“Clients should not be forced to depend on methods they don’t use”

## Dependency inversion
>“High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should not depend on abstractions”

We going to take base on each one of the principles in the next section.

Note: For more information about SOLID I recomend to read the book “Clean Architecture” by Robert C. Martin

# SOLID and Go
As we can see in the previous section, SOLID is thought for Object-Oriented programming but that is not a limitation for languages like Golang and I will show you how you can arrange it and implement it in your Golang projects.

Let us start with each one of the principles and define how are implemented with Golang.

## Single Responsibility Principle — SRP
— Definition: “A module should have one, and only one reason to change”


We already know what the principle means, now is time to learn how to implement it in Golang.

For golang we could define this priciple as “One function or type should have one and only one job, and one and only one responsibility”, Telling that is time to go ahead with an example to see it in action:

Having the next code:


As we can see is not bad at all, but, what is happening in the code?. If we stop a moment and read the lines 14 and 23 inside of area methods we can discover something, the code is calculating the area and is printing the result, it is breaking the “single responsibility principle”.

Now, applying the “single responsibility principle”:

Step 1: Modify the methods to just do one action, in this case, calculate the area

Step 2: We could define a new type to handle the area output, in our example, we want to print it on the console. Then we delegate the string conversion to a new type and method named outPrinter which has a method to return a string with the shape area.

In line 11 we are passing as a parameter the shape interface, it helps us to use any type that implements the interface methods, in this case, square and circle types whos define the area method.

Step 3: Finally in the main we could do the following:

As we can see in the example the code defines the codes with separate responsibilities, in one hand the square and circle types define the method area with the action to calculate the area and return the value, on the other hand, the type outPrinter will define all the need to generate the needed string output.

## Open-Close Principle — OCP
— Definition: “A software artifact should be open for extension but closed for modifications”


Lets start with the following code:


The previous code doesn’t implement the “open-close principle“, the reason is the following:

The calculator type has a method “sumAreas” that defines as a parameter “shapes” of interface{} type. Lines ahead we can find a switch case to map each possible type, which is breaking the principle because at the time to define a new shape “triangle” by example, we will need to modify the “sumAreas” method to handle the new type.
How to fix it?

We can follow the next approach:

A “shape” interface is defined with a method area
Each one of our example types defines the method “area”

The other needed change is for the calculator “sumAreas” method, this time the param is defined as a shape, the switch-case was removed and in its place, the code executes the “area” method from each shape.

And here is the way to use the new implementation:


## Liskov Substitution principle — LSP
— Definition: “What is wanted here is something like the following substitution property: If for each object o1 of type S there is an object o2 of type T such that for all programs P defined in terms of T, the behavior of P is unchanged when o1 is substituted for o2 then S is a subtype of T”


Golang doesn’t have inheritance but has composition. we can compose multiple structs and it will work for our example of the “Liskov substitution principle”. Let’s go to the code example:


In the code above we can analyze the following:

The code defines the “vehicle” type that defines the interface method “getName”.
“Car” and “motorcycle” has access to “vehicle” using composition, which means that apart to get access to the properties, the car and motorcycle have access to the “vehicle” methods.
The printer method will work with vehicles, cars, and motorcycles.
In a conclusion, we can say the following, the “Liskov substitution principle” is applied because the “car” type and “motorcycle” type could be substituted by the “vehicle” type.

In the following code, we can see the main function and the console output.



## Interface Segregation — ISP
— Definition: “Clients should not be forced to depend on methods they don’t use”


Is simple as its definition, and we can say “Keep interfaces simple, preferable just one method”.

In the following example I’ll show you how to apply this principle in Golang:

Let us start, in the image below we can see defined an interface with two methods, one for the area and another one for the volume, all is ok until the moment.


Now, as we can observe in the image below the code defines two shapes, one square and one cube, each one implementing the method area() and volume().


And here we have two functions, one to sum the areas and another one to sum the volumes.


All looks good in the code, but if we analyze a bit and remember the principle “Clients should not be forced to depend on methods they don’t use” we can see the following thing in the shapes:

Square doesn’t need the volume method because is a flat shape
Only cube needs the volume method because is an object shape
Based on those points we could fix the code by implementing the next changes:

- Add a new interface named “shape” to define the “area()” method.
- Add a new interface for “objects” shapes to define the “volume()” method, and is composed with a “shape” interface to allow the use of the “area()” method.

In our example square implements “area()” and cube implements “area()” and “volume()” that covers the “interface segregation principle” because we are splitting the interfaces to don’t force them to use or define a method that won’t be used by the chape, in the example case the square.

And the last change, the functions to sum areas and volumes need to be changed to just accept the correct shapes.


## Dependency Inversion -DIP
- Definition: “High-level modules should not depend on low-level modules. Both should depend on abstractions. Abstractions should not depend on details. Details should not depend on abstractions”


Here is an example to explain more what the principle means.

In the image below we can see a very basic example that implements a “database” connection and a repository that is used to query the data.

The code doesn’t comply with the principle because it is defining in the repository the struct type to access the query methods.


Now, Implementing the “Dependency inversion” principle the code is modified in the next points:

- Added a new interface to define the Database methods
- Modified the repository, instead of defining the property of type “MySQL” now defines the abstraction that is the interface type.
- In the image below we can see the changes applied based on the principle definition:


Note: We could improve the code using factories to initialize the repository properties.

## Conclusion

As software developers, we should always look for the best way to write our codes, and applying the SOLID principles in our projects will help us write code more rigid, scalable, and tolerant to modifications since we are correctly defining the foundations of our application.

Recommended lectures:

- Clean Architecture by Robert C. Martin
- Design Patterns by Erich Gamma