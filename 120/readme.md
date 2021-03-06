## Object Oriented Programming

1. Describe "what is object in Ruby" in less than 30 words?

2. What are procedural programming and object-oriented programming?(optional question)

3. Observe the example you just wrote, think of some of the pros and cons of OOP.

4. In essence, OOP is just a programming paradigm, is there any other paradigms(y/n)?

5. Describe Encapsulation in 30 words.

6. Describe Polymorphism in 30 words.

7. Extract a key word(concept) from the definitions of Encapsulation and Polymorphism, this word should be one of the focusing points they all share.

8. What the `???` should be?
  - factory -- products, mould -- swords, blueprint -- buidings, ??? -- objects

9. This type of relationship can be analogized to what relationship?
  - parent gives birth to child becomes parent gives birth to child becomes ...

### Instance variable

10. Instance variables keep track of ??? and instance methods expose ??? for objects.

11. What method will be called automatically every time you create a new object?

12. Can an instance variable's lifespan be longer than the object which owns it?
  - can an instance variable keep existing after the the object which owns it 'died'?

14. Uninitialized new instance variables can be referenced in an instance method, what is the return value?
  - is this the same with instance variables for Class?
  - is this the same with class variables?
  - give example(s) to demonstrate this

### `to_s`, `puts`, `p`, string interpolation

15. Observe the code below, describe the relationships among `to_s`, `puts` and `p` in a reasonable way.
  - be aware of the difference between output message and return value


16. Among `puts`, `p` and string interpolation, which ones will call `to_s`, if called, when?

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

### Inheritance and mixin

18. When a class inherits from another class, what the two main things it can get from the superclass?
  - hint: an object can has its own () and ().

19. Think of these words: lookup path, `super`, inherit, override. Choose one word to describe the directionality of these words.
  - (outward / inward / upward / downward)

20. Modules have two main features, what are they?

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

How many condition branches are there in the method?

```ruby
def a_method
  if age < 5
  elsif age > 10
  elsif age % 10 == 0
  end
end
```

How about this?

```ruby
def a_method
  if !(5..10).include?(age)
  elsif age % 10 == 0
  end
end
```

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

### Collaborator objects

24. Can an object be the state of another object?
  - give an example

### Exceptions

25. Look at the code below:

```ruby
class RustyError < StandardError; end

class Clown
  attr_accessor :balls
  def throw
    balls.each do |ball|
      puts "Throwing #{ball}. Catch me!"
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

29. How to reference the constant `NUMBER` in `a_method` in this case?

```ruby
module X
  NUMBER = 1
end

class A
  include module
end

class B < A
  def a_method
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

34. Can the code below work out ok?

```ruby
class Dog
  private
  attr_writer :age
end

Dog.new.age = 2
```

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

- why the first case failed?
- according to the last 2 cases, can you explain the rule of using private setter method?

 *Updated*

### Module and Multiple Inheritance

35. Give an example which can illustrate the use of mixin in Ruby core.

36. `class C` inherits from `class B` inherits from `class A` inherits from `class Object`. Is this multiple Inheritance? If not, give an example of multiple inheritance

37. Using a code example to illustrate why sometimes single inheritance cannot meet the needs.

38. To solve the problem mentioned in previous question, what is the solution in Ruby, what mechanism does Ruby use to simulate(mimic) multiple inheritance, give code example to illustrate this.

39. Read this description about public methods

> A public method is a method that is available to anyone who knows either the class name or the object's name. These methods are readily available for the rest of the program to use and comprise the class's interface (that's how other classes and objects will interact with this class and its objects).

Write a code example to illustrate the things that are discribed by this paragraph

40. Private methods cannot be called out of the class definition.

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
- give an example about "call `dream` outside of the definition"(ignore the raised error)
