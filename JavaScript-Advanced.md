# how chrome v8 compile javascript

Chrome's V8 JavaScript engine uses a Just-In-Time (JIT) compilation process to execute JavaScript code. This process involves several steps:
1. Parsing: The V8 engine parses the source code of a JavaScript program to create an Abstract Syntax Tree (AST) that represents the program's structure.
2. Compilation: The V8 engine then compiles the AST into machine code. V8 uses a baseline compiler and an optimizing compiler. The baseline compiler generates code quickly but does not perform any optimization. The optimizing compiler generates highly optimized code, but it takes longer to compile.
3. Execution: The compiled code is executed by the V8 engine. If the code has already been compiled, the V8 engine can skip the parsing and compilation steps and execute the previously compiled code directly.

V8 also uses a technique called "hidden classes" to optimize object creation and property access. When an object is created, V8 assigns it a hidden class that contains information about the object's structure. This allows V8 to generate more efficient code for accessing object properties.

In addition to JIT compilation, V8 also includes a garbage collector that manages memory allocation and deallocation. The garbage collector frees memory that is no longer being used by a program, reducing the risk of memory leaks and improving overall performance.

# AST example

Here's an example of an Abstract Syntax Tree (AST) for a simple JavaScript function that adds two numbers:

```
function add(a, b) {
  return a + b;
}
```
The AST for this function would look something like this:

```
Program
└── FunctionDeclaration (add)
    ├── Identifier (a)
    ├── Identifier (b)
    └── BlockStatement
        └── ReturnStatement
            └── BinaryExpression (+)
                ├── Identifier (a)
                └── Identifier (b)
```

In this AST, the root node is a **Program** node that represents the entire JavaScript program. The **Program** node contains a **FunctionDeclaration** node, which represents the add function. The **FunctionDeclaration** node contains two **Identifier** nodes, which represent the function's parameters (**a** and **b**), and a **BlockStatement** node, which represents the function's body. The **BlockStatement** node contains a **ReturnStatement** node, which represents the **return** statement inside the function. The **ReturnStatement** node contains a **BinaryExpression** node, which represents the addition operation (**+**) between the two parameter variables (**a** and **b**).

# Hidden Class

In V8, a hidden class is a mechanism used to optimize object property access. It's a way for V8 to track the shape of an object and optimize access to its properties.

When an object is created, V8 assigns it a hidden class, which represents the object's shape or structure. The hidden class contains information about the object's properties, such as their names and offsets in memory. When an object's properties are accessed or modified, V8 uses the hidden class to quickly locate the properties in memory.

By using hidden classes, V8 can generate more efficient code for accessing object properties. When an object is created, V8 generates code that's optimized for the object's hidden class. If another object is created with the same shape, V8 can reuse the same hidden class and the same optimized code.

Using hidden classes can significantly improve performance in JavaScript programs that create and manipulate many objects with the same shape, such as in object-oriented programming or data processing applications. It's worth noting that V8 isn't the only JavaScript engine that uses hidden classes; other engines like SpiderMonkey and JavaScriptCore also use similar mechanisms to optimize property access.

# Example of hidden class

Here's an example of how hidden classes work in V8. Consider the following JavaScript code:

```
function MyClass(a, b) {
  this.x = a;
  this.y = b;
}
```

When an instance of **MyClass** is created, V8 assigns it a hidden class that represents the object's shape or structure. In this case, the hidden class might look something like this:

```
MyClassHiddenClass
├── x: offset 0
└── y: offset 1
```

The **MyClassHiddenClass** hidden class indicates that the object has two properties, **x** and **y**, with offsets 0 and 1, respectively.

When **obj.x** is accessed or modified, V8 uses the hidden class to locate the property in memory. Because **obj** has the **MyClassHiddenClass** hidden class, V8 knows that **obj.x** is located at offset 0 in memory. This allows V8 to quickly read or write the value of **obj.x** without having to search for it.

If another instance of **MyClass** is created with the same shape, V8 can reuse the same hidden class and the same optimized code. This can significantly improve performance in JavaScript programs that create and manipulate many objects with the same shape.

# How class is represented in javascript

In JavaScript, classes are represented using the **class** keyword, which was introduced in ECMAScript 2015 (also known as ES6).

Here's an example of how a class is defined in JavaScript:

```
class MyClass {
  constructor(a, b) {
    this.x = a;
    this.y = b;
  }
  
  getSum() {
    return this.x + this.y;
  }
}
```

In this example, **MyClass** is a class that has a constructor method and a **getSum** method. The **constructor** method is called when a new instance of **MyClass** is created, and it initializes the **x** and **y** properties of the object. The **getSum** method returns the sum of the **x** and **y** properties.

To create an instance of **MyClass**, you can use the **new** keyword:

```
var obj = new MyClass(1, 2);
console.log(obj.getSum()); // Output: 3
```

Under the hood, classes in JavaScript are implemented using functions and prototypal inheritance. When a class is defined using the **class** keyword, a constructor function is created that can be used to create new instances of the class. The methods defined inside the class are added to the prototype of the constructor function, which allows instances of the class to access them through inheritance.

Classes in JavaScript provide a more familiar syntax and syntax sugar for developers who come from object-oriented programming languages. They make it easier to create reusable code and organize complex codebases.

# Under the hood implementation of class

Under the hood, classes in JavaScript are implemented using functions and prototypal inheritance. When a class is defined using the **class** keyword, a constructor function is created that can be used to create new instances of the class. The methods defined inside the class are added to the prototype of the constructor function, which allows instances of the class to access them through inheritance.

Here's an example of how the **MyClass** class from the previous example might be implemented using functions and prototypal inheritance:

```
function MyClass(a, b) {
  this.x = a;
  this.y = b;
}

MyClass.prototype.getSum = function() {
  return this.x + this.y;
};
```

In this example, the **MyClass** constructor function is defined using a regular function. Inside the constructor function, the **this** keyword is used to initialize the **x** and **y** properties of the object.

The **getSum** method is defined on the prototype of the **MyClass** constructor function using the **prototype** property. This allows instances of **MyClass** to access the **getSum** method through inheritance.

To create an instance of **MyClass**, you can use the **new** keyword:

```
var obj = new MyClass(1, 2);
console.log(obj.getSum()); // Output: 3
```

Under the hood, the **new** keyword creates a new object and sets its prototype to the prototype of the constructor function (**MyClass.prototype** in this case). It then calls the constructor function with the new object as its context (**this**), and returns the new object.

While the **class** syntax provides a more familiar syntax for developers coming from object-oriented programming languages, it's important to understand the underlying implementation of classes in JavaScript to use them effectively.

# Prototype

In JavaScript, prototype is a mechanism that allows objects to inherit properties and methods from other objects. Every object in JavaScript has an internal property called **\[\[Prototype]]** (which can also be accessed using the **Object.getPrototypeOf()** method), which points to another object that serves as its prototype.

When a property or method is accessed on an object, JavaScript first looks for the property or method on the object itself. If it doesn't find the property or method on the object, it then looks for it on the object's prototype. If it still doesn't find it, it looks for it on the prototype's prototype, and so on, until it reaches the end of the prototype chain.

Here's an example to illustrate how prototype works in JavaScript:

```
var person = {
  name: 'John',
  age: 30,
  sayHello: function() {
    console.log('Hello, my name is ' + this.name + ' and I am ' + this.age + ' years old.');
  }
};

var employee = {
  position: 'Developer',
  salary: 50000
};

employee.__proto__ = person; // Set the prototype of employee to person

console.log(employee.name); // Output: John
console.log(employee.age); // Output: 30
employee.sayHello(); // Output: Hello, my name is John and I am 30 years old.
```

In this example, we define an object called **person** that has a **name** property, an **age** property, and a **sayHello** method. We then define an object called **employee** that has a **position** property and a **salary** property.

To make **employee** inherit from **person**, we set the prototype of **employee** to **person** using the **\_\_proto__** property. This creates a prototype chain where **employee** inherits properties and methods from **person**.

When we access the **name** property, the **age** property, and the **sayHello** method on **employee**, JavaScript first looks for them on the **employee** object. Since **employee** doesn't have these properties and methods, JavaScript then looks for them on the **person** object (which is **employee**'s prototype). Since **person** has these properties and methods, JavaScript returns their values and executes the method.

In summary, prototype is a powerful mechanism in JavaScript that allows objects to inherit properties and methods from other objects, which promotes code reuse and helps create clean and maintainable code.