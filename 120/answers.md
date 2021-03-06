## Object Oriented Programming

1. Describe "what is object in Ruby"?

- In Ruby, objects can be thought of as conceptual entities which have states(instance variables) and behaviors(methods), objects can exchange information with each other through different types of interfaces.

2. What are procedural programming and object-oriented programming?(optional question)

- procedural programming and OOP are both a type of programming paradigms.
  - precedural programming does things in [an imperative, step-by-step, logical fashion](https://launchschool.com/curriculum/courses/79f19170).
  - OOP does things by encapsulating data into objects and let them interact with each other.

  - (from wiki)[Object-oriented programming](https://en.wikibooks.org/wiki/C%2B%2B_Programming/Programming_Languages/Paradigms#Object-oriented_programming) can be seen as an extension of procedural programming in which programs are made up of collections of individual units called objects that have a distinct purpose and function with limited or no dependencies on implementation.
- examples:

Procedural style: steps are visible.

```ruby
bag = []
loop do
  batter = make_batter
  cake = bake_cake(batter)
  bag << cake
  break if bag.size == 5
end
```

OOP: closer to English but logic process is not so obvious

```ruby
class Batter
end

class Cook
  attr_reader :bag

  def initialize
    @bag = []
  end

  def make_5_cakes
    while bag.size < 5
      bag << bake_cake
    end
  end

  private

  def bake(batter)
  end

  def bake_cake
    bake(Batter.new)
  end
end

Cook.new.make_5_cakes
```

3. Observe the example you just wrote, think of some of the pros and cons of OOP.

- pros
  - easier to understand on a conceptual level
  - individuality of objects makes program more flexible
  - easier to manage interfaces
- cons
  - hard to observe program procedures in linear way
  - needs to write more code
  - too flexible may cause confusion on design(especially for novices)
  - may take more resources

4. In essence, OOP is just a programming paradigm, is there any other paradigms(y/n)?
- yes of course

5. Describe Encapsulation in 30 words.
- The behavior of deliberately hiding pieces of functionality(some code) while exposing interfaces that the designer only want to expose. It's a concern about both program design and security.

6. Describe Polymorphism in 30 words.
- In Ruby, Polymorphism describes the feature(implementation) that different receiver may return various values when we send same message to them.

```ruby
2.send(:*, 5)
'2'.send(:*, 5)
[2].send(:*, 5)
```

In above cases, same message `:*` and `5` are sent to different objects `2`(Integer), `'2'`(String), `[2]`(Array), but all the return values are different.

7. Extract a key word(concept) from the definitions of Encapsulation and Polymorphism, this word should be one of the focusing points they all share.

interface

8. What the `???` should be?

factory -- products, mould -- swords, blueprint -- buildings, ??? -- objects

- class

9. This type of relationship can be analogized to what relationship?

parent gives birth to child becomes parent gives birth to child becomes ...

- class and object

### Instance variable

10. Instance variables keep track of ??? and instance methods expose ??? for objects.

- states
- behaviors

11. What method will be called automatically every time you create a new object?

- `initialize`

12. Can an instance variable's lifespan be longer than the object which owns it?

- can an instance variable keep existing after the the object which owns it 'died'?

- no, because instance variables are used to keep track of object's states
- no

14. Uninitialized new instance variables can be referenced in an instance method, what is the return value?
  - is this the same with instance variables for Class?
  - is this the same with class variables?
  - give example(s) to demonstrate this

- `nil`
- yes
- no

- examples:

```ruby
class Dog
  def print_instance_variable
    p @var
  end

  def self.print_class_instance_variable
    p @var
  end

  def self.print_class_variable
    p @@var
  end
end

dog = Dog.new
dog.print_instance_variable
Dog.print_class_instance_variable
Dog.print_class_variable
```

### `to_s`, `puts`, `p`, string interpolation

15. Describe the relationships among `to_s`, `puts` and `p` in a reasonable way.
  - be aware of the difference between output message and return value

- `to_s` returns an object's string representation, or say returns a string which contains the certain information about the object. What would the information be is determined by how the object's class defines its `to_s` instance method
- build on `to_s`, `puts` can be understood as "print out the string representation of the object and return `nil`"
- `inspect` also returns string which contains information about the object, but different from `to_s` it contains some extra information for example its instance variables, `inspect` returns string.
- `p` is build on `inspect`, it will call `inspect` on the object and append a new line character then print that out, but `p` returns the object itself.

16. Among `puts`, `p` and string interpolation, which ones will call `to_s`, if called, when?

- `puts` will call `to_s` if the given argument(s) is not a string
- `inspect` will not call `to_s` automatically but the return value is also string
- string interpolation will call `to_s` at the very end of its interior.

17. Would this code work out okay? Can you guess the message being output and what is the final return value?

```ruby
"#{
class Dog
  attr_accessor :name, :age
end

dog = Dog.new
dog.name = 'Babo'
dog.age = 3
puts "#{dog.name} is a #{dog.age} years old #{dog.class.name.downcase}"
}"
```

- it works.
- the printed out message would be `Babo is a 3 years old dog`
- the final return value is an empty string
  - the last line inside the whole string interpolation is `puts ...`, so the return value in the string interpolation is `nil`
  - so it's same as `"#{nil}"`, `nil` into a string interpolation will evaluate to nothing
  - so finally we get an emtpy string `""`

### Inheritance and mixin

18. When a class inherits from another class, what the two main things it can get from the superclass?
  - hint: an object can has its own () and ().

- states(attributes) and behaviors(methods)

19. Think of these words: lookup path, `super`, inherit, override. Choose one word to describe the directionality of these words.
  - (outward / inward / upward / downward)

- upward

20. Modules have two main features, what are they?

- holding reusable methods which can be mixed into other classes.
- namespacing: organize similar(or same type) classes into one module which can facilitate management and avoid class name collision

21. Given this code, how to call `shine` method within the `show` method in `Face` class then print out `Shining`

```ruby
module Features
  def self.shine
    puts "Shining"
  end
end

class Face
  include Features
  def show
    # how?
  end
end

Face.new.show
```

```ruby
def show
  Features.shine
end
```

22. What is the output of the code?
  - what is the rule underlying?

```ruby
module Breathable
  def breathe_in
    puts 'Oxygen...'
  end

  def breathe_in
    puts 'Whatever...'
  end
end

class Creature
  include Breathable
end

class Plant < Creature
end

class Animal < Creature
end

class Human < Animal
end

Human.new.breathe_in
```

- the outputted message is `Whatever...`
  - human object didn't find `breathe_in` in its class
  - so it keep looking for it upwards following the inheritance chain
  - until it went into `class Creature` which included `module Breathable`
  - in `module Breathable`, the method lookup order is down to up, so the 'Whatever' one will override the 'Oxygen' one

### Assignment Branch Condition Size - ABC size

23. Based on the class definition:

```ruby
class Dog
  attr_reader :age
end
```

How many method calls are there in this method?

```ruby
def a_method
  if age < 5
  elsif age > 10
  elsif age % 10 == 0
  end
end
```
- after we wrote `attr_reader :age`, `age` would be a method call, so
  - `age` + 3
  - `<`, `>`, `%` and `==` are all fake operators + 4
- totally 7

What about this?

```ruby
def a_method
  age_value = age
  if age_value < 5
  elsif age_value > 10
  elsif age_value % 10 == 0
  end
end
```

- `age` was called once + 1
- `age_value` is a variable points to `age`'s value so it's not a method call
- `<`, `>`, `%` and `==` are all fake operators + 4
- totally 5

How many condition branches are there in the method?

```ruby
def a_method
  if age < 5
  elsif age > 10
  elsif age % 10 == 0
  end
end
```

- `if` + `elsif` + `elsif` = 3

How about this?

```ruby
def a_method
  if !(5..10).include?(age)
  elsif age % 10 == 0
  end
end
```

- `if` + `elsif` = 2

Count the exact number of Assignments, Branches and Conditions in `a_method`

```ruby
class Dog
  attr_accessor :age

  def a_method
    if age > 10
      self.age = 9
    elsif age > 8
    elsif age > 7
    elsif age > 6
    elsif age > 5
    elsif age > 4
    elsif age > 3
    elsif age > 2
    elsif age > 1
    end
  end
end
```

Method call:
  - `age` + 9
  - `age=()` +1
  - `>` + 9

Assignment
  - None: (`self.age = 9` is a method call)

Condition branches:
  - + 9

### Collaborator objects

24. Can an object be the state of another object?
  - give an example

- yes
- example

```ruby
class Person
end

class Dog
  attr_reader :name, :age, :owner

  def initialize(name, age)
    @name = name
    @age = age
    @owner = Person.new
  end
end

dog = Dog.new('Puppy', 2)
dog.name.is_a?(Object)
dog.age.is_a?(Object)
dog.owner.is_a?(Person)
```

### Exceptions

25. Look at the code below:

```ruby
class RustyError < StandardError; end

class Clown
  attr_accessor :balls
  def throw
    balls.each do |ball|
      puts "Throwing #{ball}. Catch it!"
      raise(ball, "Didn't catch it...") if ball == RustyError
    end
  end

  def play
    throw
  rescue StandardError => e
    puts e.message
  end
end

balls = [NameError, ArgumentError, RustyError, RuntimeError]
clown = Clown.new
clown.balls = balls
# clown.play
```

Before running the last line
  - how many exceptions will be raised
  - how many times the message `"Throwing ... . Catch it!"` will be printed out?
  - what is the last printed out message?
  - in `balls` array, which exception classes can be rescued by `rescue StandardError` if any of them is been raised? Why?

- only `RustyError` is raised(but not shown because it has been rescued), or say no visible exception is raised, that's because we specified a condition `if ball == RustyError`
- 3 times.
- the last printed out message is `"Didn't catch it..."`, it's printed by `puts e.message` in `play` method's `rescue` branch
- all of them. Because all of them are descendant classes of `StandardError`, `rescue StandardError` will rescue `StandardError` and all of its descendant classes

### Reference constant

26. How to reference the constant `NUMBER` in `a_method` in this case?

```ruby
class A
  def a_method
  end
end

class B < A
end

class C < B
  NUMBER = 1
end
```

- `NUMBER` is first initialized in `class C`
- `class A` is at the upstream of `C` we cannot access a constant downwards

```ruby
class A
  def a_method
    C::NUMBER
  end
end
```

27. How to reference the constant `NUMBER` in `a_method` in this case?
  - come up with more than 5 ways, it doesn't have to be concise, just make it work

```ruby
class A
  NUMBER = 1
end

class B < A
end

class C < B
  def a_method
  end
end
```

- core rule
  - descendant class can access superclass' constants without specifying first initialized place(class)
  - looking upward

```ruby
class C < B
  def a_method
    # 1: NUMBER; 2: C::NUMBER; 3: B::NUMBER; 4: A::NUMBER; 5: self.class::NUMBER
    # 6: self.class.superclass::NUMBER; 7: self.class.superclass.superclass::NUMBER
    # 8: C.superclass::NUMBER; 9: B.superclass::NUMBER ....
  end
end
```

28. How to reference the constant `NUMBER` in `a_method` in this case?

```ruby
module X
  NUMBER = 1
end

class A
end

class B < A
  def a_method
  end
end
```

- answer

```ruby
class B < A
  def a_method
    X::NUMBER
  end
end
```

29. How to reference the constant `NUMBER` in `a_method` in this case?

```ruby
module X
  NUMBER = 1
end

class A
  include X
end

class B < A
  def a_method
  end
end
```

- answer
  - easiest way is ignoring classes, get it directly from the module
  - another way is referencing it without any prepended classes or modules

```ruby
class B < A
  def a_method
    X::NUMBER
  end
end
```

30. How to reference the constant `NUMBER` in `a_method` in this case?

```ruby
module X
  class A
    NUMBER = 1
  end
end

class B
  def a_method
  end
end
```

- answer

```ruby
class B
  def a_method
    X::A::NUMBER
  end
end
```

31. Look at the code:

```ruby
module X
  class A
    NUMBER = 1
  end

  class B < A
  end
end

class C
  def a_method
  end
end
```

- how to reference `NUMBER` inside `a_method` from `class C` without referencing `class A`?

- answer

```ruby
class C
  def a_method
    X::B::NUMBER
  end
end
```

### Truthiness

32. What message will be printed out? Why?

```ruby
def count_things(num = 2)
  if 'false' || num / 0 > 1
    puts "Cloudy day!"
  else
    puts "Sunny day!"
  end
end

def calculate
  count_things
rescue ArgumentError, ZeroDivisionError
  puts "Windy day!"
end

calculate
```

- calling `count_things` without passing in argument will not raise exception since we specified default value for the argument while defining this method
- `'false' || num / 0 > 1` will not raise exception, because the short circuiting mechanism of Ruby
  - when using `||`, once one truthy value appears, the whole expression will be evaluated as `true` so the rest of the expression will be skipped
  - in this case `'false'` will first evaluated to `true`, so Ruby never run `num / 0 > 1`
- no exceptions so program goes into the `if` branch in `count_things` method
- so the outputted message is `"Cloudy day!"`

### Others

33. What's the final returned value of `p Dog.new.age = 2`? Why?

```ruby
class Dog
  def age=(age)
    @age = age
    return 'I am a dog.'
    age += 100
  end
end

p Dog.new.age = 2
```

- whatever you do, the return value of a setter method in Ruby is always the original argument
- so the return value is 2

34. Can the code below work out ok?

```ruby
class Dog
  private
  attr_writer :age
end

Dog.new.age = 2
```

- no
  - private method `age=` cannot be used outside of the class definition

How about this?

```ruby
class Dog
  def set_age(age)
    dog = self
    dog.age = age
  end

  private
  attr_writer :age
end

Dog.new.set_age(2)
```

- no
  - though setter method is the only exception which accepts a receiver, but
    - the receiver must exactly be `self`
    - any other variables pointing to current instance are not acceptted even they are pointing to the same thing as `self`

Then how about this?

```ruby
class Dog
  def set_age(age)
    self.age = age
  end

  private
  attr_writer :age
end

Dog.new.set_age(2)
```

- This works.

- why the first case failed?
- according to the last 2 cases, can you explain the rule of using private setter method?

Private setter method is an exception that can be invoked with a receiver, but the receiver has to be `self`.

 *Updated*

### Module and Multiple Inheritance

35. Give an example which can illustrate the use of mixin in Ruby core.

- An example is about `class String`, `class Hash`, `class Array` and `class Numeric`
- These classes are relating to the main data types that we use very often in Ruby.

If we check the ancestors for each of them:
```ruby
> [Array, Hash, String, Numeric].each { |main_type| p main_type.ancestors }
[Array, Enumerable, Object, Kernel, BasicObject]
[Hash, Enumerable, Object, Kernel, BasicObject]
[String, Comparable, Object, Kernel, BasicObject]
[Numeric, Comparable, Object, Kernel, BasicObject]
# => [Array, Hash, String, Numeric]
```

Tow things here need to be noticed:
- not all ancestors are classes
- their direct superclass is `Object` class

Actually `Enumerable`, `Comparable` and `Kernel` are modules, not classes. Modules are like pluggable packages that enable specific classes to do many things. Once a class includes a module, its instances and its subclasses' instances are able to do things defined in this module.(graph may not showed complete, click link below to see full)

https://s3-ap-southeast-1.amazonaws.com/image-for-articles/image-bucket-1/core.jpg

![](https://s3-ap-southeast-1.amazonaws.com/image-for-articles/image-bucket-1/core.jpg)
*Kernel is included in `class Object`, this is not labelled in the graph.*

36. `class C` inherits from `class B` inherits from `class A` inherits from `class Object`. Is this multiple Inheritance? If not, give an example of multiple inheritance

- No. It's not. Multiple inheritance means one class directly inherits from many classes.
- exmaple:

```ruby
class A; end

class B; end

class C < A; end
class C < B; end
```

[here's a discussion on stack overflow](https://stackoverflow.com/questions/10254689/multiple-inheritance-in-ruby)

37. Using a code example to illustrate why sometimes single inheritance cannot meet the needs.

38. To solve the problem mentioned in previous question, what is the solution in Ruby, what mechanism does Ruby use to simulate(mimic) multiple inheritance, give code example to illustrate this.

- answer 37 and 38

Say we have these classes:
- `Animal`
- `Mammal`, `Fish`, `Bird`
- `Whale`, `Dog`, `Cat`, `Penguin`, `Woodpecker`

Their relationships can be represented by code:

```ruby
class Animal; end

class Mammal < Animal
end

class Fish < Animal
  # able to swim
end

class Bird < Animal
end

class Whale < Mammal
   # able to swim
end

class Dog < Mammal
  # able to swim
  # able to walk
end

class Cat < Mammal
  # able to walk
end

class Penguin < Bird
  # able to swim
  # able to walk
end

class Woodpecker < Bird
  # able to fly
end
```

Then we can visualize the relationships in the graph(graph may not showed complete, click link below to see full)

https://s3-ap-southeast-1.amazonaws.com/image-for-articles/image-bucket-1/animals.jpg

![](https://s3-ap-southeast-1.amazonaws.com/image-for-articles/image-bucket-1/animals.jpg)

According to this graph we can see:
- we cannot include `Walkable` and `Swimmable` into `Mammal`, since one of the mammals `Cat` cannot swim, also `Whale` cannot walk
-  we cannot include `Flyable` into `Bird` because there is an exception `Penguin` cannot swim, it just walks...

When focusing on the classes which include 2 modules -- `Dog` and `Penguin` in this case
- `Dog` can swim and walk
  - but it cannot inherit from `class Fish` to enable itself to swim, because it is not fish
  - it cannot inherit from `class Mammal` to enable itself to walk since the existence of `Whale` disproves 'all mammals can walk'

- `Penguin` can swim and walk too
  - but it cannot inherit from `class Mammal` to enable itself to walk(same reason with Dog)
  - it cannot inherit from `class Fish` to enable itself to swim, because it is not fish

Even if Ruby allowed multiple inheritance, this problem cannot be solve. So we have to distribute different modules to different classes as needed.

39. Read this description about public methods

> A public method is a method that is available to anyone who knows either the class name or the object's name. These methods are readily available for the rest of the program to use and comprise the class' interface (that's how other classes and objects will interact with this class and its objects).

Write a code example to illustrate the things that are described by this paragraph

```ruby
class Human
  def eat(food)
  end
end

class Meat
  def cook
  end
end

Human.new.eat(Meat.new.cook) #or

bob = Human.new
cooked_meat = Meat.new.cook

bob.eat(cooked_meat)
```

In the above example
- `cook` method is a public instance method in `class Meat`
- but we can call `cook` once we
  - 1) know the class which `cook` resides in -- `Meat`, because once we know the name of the class, then we can instantiate instances from this class, then we can call the public instance methods without restriction
  - 2) get an instance of `Meat`(same reason)

We can even make many other classes which can eat cooked meat, like `Bear`, `Cat`, `Dog` ... No matter what the classes are, once they want interact with `Meat`, the `cook` method would be an "always there" (entrance)interface

for example

```ruby
Dog.new.feed(Meat.new.cook)
Bear.new.feed(Meat.new.cook)
```

Or even other classes's class methods which need cooked meat

```ruby
class Restaurant
  def self.serve(food)
  end
end

Restaurant.serve(Meat.new.cook)
```

So once I know:
- the public method's class name or
- an instance from its class

I can call this method without restriction.

40. Private methods are methods that cannot be called out of the class definition.

Given this example:

```ruby
class Dog
  private

  def self.dream
    puts "I am flying!"
  end

  public

  def self.sleep # 2
    dream
  end

  dream # 1
end
```

- look at the very end of the class definition, would the call to `dream` raise exception? What if we wrote `self.dream`? Why?
- find a way to print out `I am flying!"` outside of the class definition
- find a way to print out `I am flying!"` inside of the class definition without directly calling `dream`
- give an example about "call `dream` outside of the definition"

- answer

- No; but `self.dream` would. Because `dream` is a private class method, we cannot call a private method with any receiver
- write `Dog.sleep` outside of the class definition
- write `self.sleep` or `sleep` after the definition of class method `sleep`
- within the method definition of class method `sleep`; and the last line inside the class definition
- write `Dog.dream` outside of class definition(ignore the raised error)
