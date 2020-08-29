# Design-Patterns-In-Kotlin
### What is Design Patterns ?
<hr><p>
Design Patterns are a proven way to solve common programming problems. 
These issues or questions will usually arise during the software development period for programmers. 
Pattern design solutions are usually using object-oriented.
</p>

### Advantage of design pattern
<hr><p>
1. They are reusable in multiple projects. <br>
2. They provide the solutions that help to define the system architecture. <br>
3. They capture the software engineering experiences. <br>
4. They provide transparency to the design of an application. <br>
5. They are well-proved and testified solutions since they have been built upon the knowledge and experience of expert software developers.
</p>

### Type of design pattern 
<hr>

<h4><a href="#T1">&#9679; Creational Design Pattern</a></h4>
<P>

<a href="#C1">1. Factory Pattern </a><br>
<a href="#C2">2. Abstract Factory Pattern </a><br>
<a href="#C3">3. Singleton Pattern </a><br>
<a href="#C4">4. Prototype Pattern </a><br>
<a href="#C5">5. Builder Pattern</a>
</P>

<h4><a href="#T2">&#9679; Structural Design Pattern</a></h4>
<P>  
1. Adapter Pattern <br>
2. Bridge Pattern <br>
3. Composite Pattern <br>
4. Decorator Pattern <br>
5. Facade Pattern <br>
6. Flyweight Pattern <br>
7. Proxy Pattern <br>
</P>

<h4><a href="#T3">&#9679; Behavioral Design Pattern</a></h4>
<p>
&nbsp;&nbsp;1. Chain Of Responsibility Pattern <br>
&nbsp;&nbsp;2. Command Pattern <br>
&nbsp;&nbsp;3. Interpreter Pattern <br>
&nbsp;&nbsp;4. Iterator Pattern <br>
&nbsp;&nbsp;5. Mediator Pattern <br>
&nbsp;&nbsp;6. Memento Pattern <br>
&nbsp;&nbsp;7. Observer Pattern <br>
&nbsp;&nbsp;8. State Pattern <br> 
&nbsp;&nbsp;9. Strategy Pattern <br>
10. Template Pattern <br> 
11. Visitor Pattern
</p>

<h3 id="T1">Creational Design Pattern</h3>
<hr><p>
One of the sub-branches of Pattern Design is Creational Design Pattern. This pattern is a way to make things. 
This template is used when we want to create an object from a class in our program. 
But for many it may be very easy to build an object and just by new we create an object. 
But hard code is not a good solution Because we sometimes change the identity of a class in our code, and in such cases we have to use the Creational Design pattern.
</p>

<h4 id="C1"><a href="https://github.com/ghasem-79/Design-Patterns-In-Kotlin/tree/master/src/main/creational/factory">Factory Pattern</a></h4>
<hr><p>
The factory pattern is used to replace class constructors, 
abstracting the process of object generation so that the type of the object instantiated can be determined at run-time.
</p>

<img src="https://github.com/ghasem-79/Design-Patterns-In-Kotlin/blob/master/uml/FactoryPattern.png?raw=true" alt="factory" width="400" height="400"><br>

<p>
<b>Example</b><br>
</p>

    interface Shape {
        fun draw(): String
    }

    class Circle : Shape {
        override fun draw(): String {
            return "Shape : Circle"
        }
    }

    class Rectangle : Shape {
        override fun draw(): String {
            return "Shape : Rectangle"
        }
    }

    class Square : Shape {
        override fun draw(): String {
            return "Shape : Square"
        }
    }

    object ShapeFactory { // Factory create shapes
        enum class ShapeType {
            CIRCLE, RECTANGLE, SQUARE
        }
        
        fun createShape(type: ShapeType): Shape {
            return when (type) {
                ShapeType.CIRCLE -> Circle()
                ShapeType.RECTANGLE -> Rectangle()
                ShapeType.SQUARE -> Square()
            }
        }
    }

<p>
<b>Usage</b><br>
</p>

    val circle = ShapeFactory.createShape(ShapeFactory.ShapeType.CIRCLE)
    println(circle.draw())

    val rectangle = ShapeFactory.createShape(ShapeFactory.ShapeType.RECTANGLE)
    println(rectangle.draw())

    val square = ShapeFactory.createShape(ShapeFactory.ShapeType.SQUARE)
    println(square.draw())

<p>
<b>Output</b><br>
</p>

    Shape : Circle
    Shape : Rectangle
    Shape : Square
    
<h4 id="C2"><a href="https://github.com/ghasem-79/Design-Patterns-In-Kotlin/tree/master/src/main/creational/abstractfactory">Abstract Factory Pattern</a></h4>
<hr><p>
The abstract factory pattern is used to provide a client with a set of related or dependant objects. 
The "family" of objects created by the factory are determined at run-time.    
</p>

<img src="https://github.com/ghasem-79/Design-Patterns-In-Kotlin/blob/master/uml/AbstractFactoryPattern.png?raw=true" alt="abstarct factory" width="500" height="500"><br>

<p>
<b>Example</b><br>
</p>

    interface Shape {
        fun draw(): String
    }

    class Circle : Shape {
        override fun draw(): String {
            return "Shape : Circle"
        }
    }

    class Rectangle : Shape {
        override fun draw(): String {
            return "Shape : Rectangle"
        }
    }

    class Square : Shape {
        override fun draw(): String {
            return "Shape : Square"
        }
    }
    
    interface Color {
        fun fill(): String
    }
    
    class Blue : Color {
        override fun fill(): String {
            return "Color : Blue"
        }
    }
    
    class Green : Color {
        override fun fill(): String {
            return "Color : Green"
        }
    }
    
    class Red : Color {
        override fun fill(): String {
            return "Color : Red"
        }
    }
    
    class ShapeFactory : AbstractFactory() { // Factory create shapes
        enum class ShapeType {
            CIRCLE, RECTANGLE, SQUARE
        }
    
        override fun getColor(type: ColorFactory.ColorType): Color? {
            return null
        }
    
        override fun getShape(type: ShapeType): Shape {
            return when (type) {
                ShapeType.CIRCLE -> Circle()
                ShapeType.RECTANGLE -> Rectangle()
                ShapeType.SQUARE -> Square()
            }
        }
    }
    
    class ColorFactory : AbstractFactory() { // Factory create color
        enum class ColorType {
            BLUE, GREEN, RED
        }
    
        override fun getColor(type: ColorType): Color {
            return when (type) {
                ColorType.BLUE -> Blue()
                ColorType.GREEN -> Green()
                ColorType.RED -> Red()
            }
        }
    
        override fun getShape(type: ShapeFactory.ShapeType): Shape? {
            return null
        }
    }    
    
    abstract class AbstractFactory {
        abstract fun getColor(type: ColorFactory.ColorType): Color?
        abstract fun getShape(type: ShapeFactory.ShapeType): Shape?
    
        companion object {
            inline fun <reified T : AbstractFactory> createFactory(): AbstractFactory = when (T::class) {
                ShapeFactory::class -> ShapeFactory()
                ColorFactory::class -> ColorFactory()
                else -> throw IllegalArgumentException()
            }
        }
    }

<p>
<b>Usage</b><br>
</p>

    var factory = AbstractFactory.createFactory<ColorFactory>()
    val blue = factory.getColor(ColorFactory.ColorType.BLUE)!!
    println(blue.fill())   
    
    factory = AbstractFactory.createFactory<ShapeFactory>()
    val circle = factory.getShape(ShapeFactory.ShapeType.CIRCLE)!!
    println(circle.draw()) 
    
<p>
<b>Output</b><br>
</p>

    Color : Blue
    Shape : Circle

<h4 id="C3"><a href="https://github.com/ghasem-79/Design-Patterns-In-Kotlin/tree/master/src/main/creational/singleton">Singleton Pattern</a></h4>
<hr><p>
The singleton pattern ensures that only one object of a particular class is ever created. 
All further references to objects of the singleton class refer to the same underlying instance.
</p>

<p>
<b>Example</b><br>
</p>

    object DataBase {
        init {
            println("Initialize : $this")
        }
    
        fun open() = "Open data base : $this"
    }
    
<p>
<b>Usage</b><br>
</p>

    DataBase.open()
    DataBase.open()
    
<p>
<b>Output</b><br>
</p>

    Initialize : singleton.DataBase@3a5ed7a6
    Open data base : singleton.DataBase@3a5ed7a6
    Open data base : singleton.DataBase@3a5ed7a6

<h4 id="C4">Prototype Pattern</h4>
<hr><p>

</p>

<h4 id="C5">Builder Pattern</h4>
<hr><p>

</p>

<h3 id="T2">Structural Design Pattern</h3>
<hr>

<h3 id="T3">Behavioral Design Pattern</h3>
<hr>
