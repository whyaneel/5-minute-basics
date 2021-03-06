<img width="494" alt="KotlinOverview" src="https://user-images.githubusercontent.com/29258789/114834700-6a461380-9e03-11eb-9454-587f1a5a6a76.png">


# Kotlin Overview

**Kotlin** *is all about experience and approach* to solving people’s problems. If we look at comparing IDEs like Eclipse and IntelliJ, over a period of time developers fall in love with IntelliJ, in the same fashion Kotlin will give you a better experience with its ***easy-syntax, functional nature***. We can onboard Kotlin easily, because of its ***interoperability*** with Java. It enables easy migration to Kotlin with small piece of code, for any brownfield (java) projects. Also can run alongside Java without any impact, this means that Java objects can be called directly from Kotlin code with Kotlin syntax and vice versa.

Kotlin ***Multiplatform*** allows you to run Kotlin in JVM, Android, JavaScript, iOS, and many. With this, it becomes simple in developing isomorphic solutions, also with single skillset can develop ServerSide API, Client Side Interactions, Mobile Apps etc, eventually saves a lot on the cost of operations.

[Spring Framework](https://spring.io/blog/2017/01/04/introducing-kotlin-support-in-spring-framework-5-0) and [Google Android](https://android-developers.googleblog.com/2017/05/android-announces-support-for-kotlin.html) announce support for Kotlin on Jan 2017 and May 2017 respectively. So, be confident to try Kotlin in production grade applications.

# Back To Basics 
## Kotlin  Syntax

Printing *Hello Kotlin* is as simple as below
```
fun main(args: Array<String>) {
    println("Hello Kotlin")
}
```

## String Templates
We can have variables and method calls as embedded expressions. Any variable can be declared with keyword **var** or **val**.
```
val language = "Kotlin"
println("Hello $language")
println("RGB: ${listOf("GREEN", "RED", "BLUE").joinToString(separator = ", ")}")
```

## Type Inference
Type can be inferred implicitly

 - **Function**, *add* function sums two integers and returns an integer

	> explicit return Type

	```
	fun add(a: Int, b: Int): Int {
	    return a + b
	}
	``` 

	> return Type inferred automatically

	```
	fun addTypeInference(a: Int, b: Int) {
	    a + b
	}
	```

	> even more simpler with Single Expression Functions

	```
	fun addTypeInferenceSingleExpression(a: Int, b: Int) = a + b
	```
	
 - **Class**, similarly for classes as well. Also, there’s no need for a new operator like Java.
    

	> here **speaker** holds a **KotlinSpeaker** object

	```
	val speaker = KotlinSpeaker("Anil", "Kumar")
	```


## Concise Class Definitions

 - **Class**
A class can be defined as simple as below, when it has 4 properties. The getters, setters, constructors will be automatically provided to us. Properties can have default values.
	```
	class KotlinSpeaker(var firstName: String, 
			    var lastName: String = "",
			    val company: String = "PALO IT", 
			    val skills : ArrayList<Skill> = arrayListOf(Skill()))
	```
  
- **Data Class**
If there is a class purely meant for holding the data, just by adding data before class, will automatically generate equals(), hashCode(), toString(), copy() implementations for the class.
	```
  data class Skill(val language: String = "Kotlin",
                   val description: String = "a functional programming language")
	```
**Usage of copy,** *copy* method creates an immutable **Skill** object with the data change we want. Here the new copy will be exact, except for the language property updated from "Kotlin" to "Java",
```
val defaultSkill = Skill()
val newSkill = defaultSkill.copy("Java")
```

## Null Safety

Say good-bye for runtime Null Pointer Exception (NPE). In Kotlin, the reference to objects are **non-null** by default. In case, if the code passes null anywhere, the compiler will highlight them immediately. But If we want a nullable object for any reason, we’ve to be explicit about it and must use a safety operator to avoid NPE.
  

## Kotlin Properties

-   **Properties** can be directly accessed to get or set the value, equivalent getters and setters will be auto generated.
  
  ```
  val speaker3 = KotlinSpeaker("Aneel", 
                               skills = arrayListOf(defaultSkill, 
                                                    defaultSkill.copy("Java"), 
                                                    defaultSkill.copy("JavaScript")))

  speaker3.firstName = "Anil"
  speaker3.lastName = "Kumar"
  ```
-   It is also possible to use as a **Backing Field**
    

## Extension Functions

An extension function is a member function of a class that is defined outside the class. E.g. If we need to use a method to the *String class* that returns a new string with sentence case, which is not already available in *String class*. Extension function can help to achieve it.
    

> here the “**String**” is receiver type and “**this**” is the receiver object

```
fun String.toSentenceCase() = this.substring(0, 1).toUpperCase() + this.substring(1)
```

> calling should be easy just with any String

```
println("a simple sentence".toSentenceCase())
```

> similarly for class also we can have extension functions, here “**KotlinSpeaker**” is receiver type

```	   
fun KotlinSpeaker.fullName(): String {
	  return "$firstName ${lastName.toUpperCase()}".trim()
}
```

## Infix Functions
Infix functions are member functions or extension functions, which have single parameter and with no default value, as well as must-have a receiver to call it.
    

E.g. **"authorOf"** added as a member function to KoltinSpeaker
```
infix fun authorOf(input: String): String{
    return "${fullName()} is author of $input"
}
```

> Invoke it as simple as below, here speaker3 is a KotlinSpeaker object which we created earlier

```
println(speaker3 authorOf "Kotlin Blog Post")
```
  

## Run on REPL

Kotlin can be run interactively with REPL.
To run REPL in IntelliJ IDEA, open **Tools | Kotlin | Kotlin REPL**.


## Immutability
-   Thread Safety and Maintainable Code are at your fingertips.
-   Becomes so simple by switching from the var keyword to val
-   A **copy** method from the data class produces an immutable object
    
	```
	val language = "Kotlin"
	println("Hello $language")

	//val language can not be assigned to new value
	language = "Something" 
	```


## Interoperability

We can have Kotlin code alongside Java code within the same project. This means Java objects can be called from Kotlin code with Kotlin syntax itself and vice versa. So, trying Kotlin in brownfield projects is very simple, and no need to migrate the entire project to experience Kotlin.


## Functional Programming
-   Use function without instantiating, because top-level functions are public & static by default
-   Functions that take a function as a parameter and/or returns function
    

	> calling **functionThatTakesFunction** with a *list of skills* and
	> *function name* as **parameters**

	```
	knowsHowManySkills(skills, ::skillsCounter)
	```


	> **functionThatTakesFunction** implementation

	```
	fun knowsHowManySkills(skills: ArrayList<Skill>, functionPlaceholder: (ArrayList<Skill>) -> Int): String {
	    val size = functionPlaceholder(skills)

	    //additional processing can be done here

	    return "Knows $size ${if (size <= 1) "Skill" else "Skills"}:"
	}
	```


	> function that being passed, implementation

	```
	fun skillsCounter(skills: ArrayList<Skill>): Int {
		return skills.size
	}
	```
  

-   We can also use high-order-functions like a filter, map with collections like Java
    
	```
	val newbeesKotlin = listOf("Ashish", "Mohan", "Yun Chung", "Tommy", "Minisha");
	val newbeesKotlinSpeakers = newbeesKotlin.map { KotlinSpeaker(it) }
						 .sortedBy { it.firstName }

	newbeesKotlinSpeakers.forEach(::println)
	```

:clap: Hope you learnt some basics of Kotlin
