# Ruby Classes & OOP

### Objectives
- Define classes in Ruby
- More exposure to instance variables
- Major Ruby "flaw" and how `attr_reader`, `attr_writer`, `attr_accessor`solve it
- Understanding what `self` is
- `private` methods
- Be able to inherit from superclasses and define subclasses

## Yesterday...
Ruby comes with a few types of Objects to get us started, things like Integer, String, Array, etc. We call these base types of Objects "Primitives." But what if we wanted to create a new type in our programming universe, a new kind of object for our code? That's what the class keyword and object orientation allows us to do.

Classes are the blueprints that define the behavior and information our objects will contain. They let us manufacture and instantiate new instances.


## Before we define a class in Ruby, let's take a look at how we defined a class in JS
```js
class Pokemon {
  constructor(name, number, fastAttack, chargeAttack){
    this.name = name;
    this.number = number;
    this.fastAttack = fastAttack;
    this.chargeAttack = chargeAttack;
  }
  walks(){
    return `I will follow you wherever you go, and I can also do a ${this.fastAttack} attack!`
  }
}

pokemon = new Pokemon("Pikachu", 12, "thunder", "brick break" )
```

## Defining a Class in Ruby

```ruby
class Person
  def initialize(name, age)
    @name = name
    @age = age
  end

  def say_hi
    puts "Hi, my name is #{@name} and I am #{@age} years old!"
  end
end

katie = Person.new('Katie', 30)
irving = Person.new('Irving', 27)
```
A few things here:
* We always use the `new` method to create a new instance.
* `initialize` is what gets called when creating the new instance.
  * We **never** call `initialize` in our code. We just define it
* We use the `@` sign to create _instance variables_ (aka _ivars_)
  * Instance variables can be referenced from any method inside of the class
  * Instance variables can **only** be referenced from methods inside of the class

## :running: Exercise :running:
-  Knowing what we know now, let's build a Dog class. The Dog class should have a `breed`, `color`, `name` as properties, and should have a `bark` method which he barks and then says his breed.
- Please instantiate 3 dogs :dog: :poodle: :paw_prints:

# But wait!

![Alt Text](https://media.giphy.com/media/9fXNQ2qZEgn3a/giphy.gif)

# Uh oh... looks like classes in Ruby has a massive flaw... :flushed:

## We run into an error if we try to do...
```ruby
irving.name
# => NoMethodError: undefined method `name' for Person
```

## How can we resolve this?



If we want `name` to be accessible outside of the class we can need to create a method

```ruby
class Person
  # ...

  def name
    @name
  end
end

# ...

irving.name # => "Irving"
```

This also means we can just use `name` in our `say_hi` method

```ruby
# ...
  def say_hi
    puts "Hi, name is #{name} and I am #{@age} years old!"
  end
# ...
```


If we also want `age` to be accessible:

```ruby
class Person
  # ...

  def age
    @age
  end
end
```

Now we can call `irving.age` (and also replace `@age` with `age` in `say_hi`).

### aka `attr_reader`

Because this pattern is so common there is a very nice method, `attr_reader`, which we can use as a short-hand.

```ruby
class Person
  # ...
  def age
    @age
  end

  def name
    @name
  end
  # ...
```

Is the same as:

```ruby
class Person
  # ...
  attr_reader :age, :name
  # ...
end
```

Neat.

## Ok great... but what about reassigning a new value to an existing property?

![Alt Text](https://media.giphy.com/media/AvAVxpOeUcxgY/giphy.gif)

Let's see if we can set the name

```ruby
irving.name = 'Mark'
# => NoMethodError: undefined method `name='
```

To fix this, we can write a method:

```ruby
class Person
  def name=(new_name)
    @name = new_name
  end
# ...
```

Now we can do

```ruby
irving.name = 'Mark'
mark.name # => 'Mark'
```

### aka `attr_writer`

Instead of having to define the method, we can just use `attr_writer`

```ruby
class Person
  attr_writer :name
# ...
```

And if we also wanted a writer for `age`

```ruby
class Person
  attr_writer :name, :age
# ...
```


### So commonplace that one phrase takes care of both `attr_accessor`

Now we have

```ruby
class Person
  attr_reader :name, :age
  attr_writer :name, :age

  # ...
```

We can merge together the first two lines with `attr_accessor`

```ruby
class Person
  attr_accessor :name, :age

  # ...
```

That one line creates 4 instance methods: `name`, `name=`, `age`, and `age=`.


### Why all these methods?

Why don't we just use the ivars everywhere instead of readers and writers? (eg. `@foo` to read and `@foo = bar` to write)

This is very error prone as instance variables always yield `nil` if they are not defined. You do NOT get an error.  This means if we mistype ivars anywhere we will get unexpected bugs that are hard to track down.

So, avoid using ivars directly.  Use our friends `attr_reader`, `attr_writer`, and `attr_accessor`.

## :running: Exercise! :running:

Go back to the 3 Dogs you created earlier and put your new `attr_reader`, `attr_writer`, `attr_accessor` to work. 
- one dog should have his `name` changed
- one dog should have his `breed` changed
- one dog should have his `color` changed

- Please take your 3 dogs, put the**M** into an **A**rray, and **P**lease iterate over this array returning just the names of the dogs

# Self

![waldo](https://user-images.githubusercontent.com/6153182/35915583-7650cf6c-0bd5-11e8-93e1-01de728c6ca6.jpg)

```ruby
class Person
  attr_accessor :name
  def initialize(name)
    @name = name
  end
  
  def greet(name)
    puts "Hi #{name}! My name is #{name}."
  end
end


elie = Person.new("Elie")
elie.greet("Avi") # "Hi Avi! My name is Avi."
```
## Self to the rescue!

```ruby
  def greet(name)
    puts "Hi #{name}! My name is #{self.name}."
  end
end
```

## Another Example
```ruby
class Person
  # ...

  def have_birthday
    puts 'happy birthday'
    self.age = age + 1
  end
```


* [For more on the concept of Self in Ruby](http://ruby-for-beginners.rubymonstas.org/writing_classes/self.html)



## `private` methods

Every method by default is `public`. This means we can call it from in our outside our class.

`private` methods can only be called from inside the class.  They are usually helper methods or methods for implementation.

```ruby
class Person
  # ...

  def have_birthday
    puts 'happy birthday'
    increment_age
  end

  # ...

  private

  def increment_age
    self.age = age + 1
  end
```

All we need is the `private` keyword.  Any method defined after that will be `private`.

```ruby
person.increment_age
# => NoMethodError: private method `increment_age' called for Person
```

This error is a good thing!  Our `increment_age` method is just used for implementation.  We do not want it to be called outside of the class.

## Another note about Self
`self` is _similar_ to JS's `this`.  If we just did `age = age + 1` that would be setting a local variable.  We need to actually call our `age=` method to change the instance variable.




## Inheritance

```ruby
class Programmer < Person
end
```
That's it! We don't even need to write an `initialize` if it does all the same setup as `Person`!

Let's confirm a few things:

```ruby
person = Person.new('Elie', 29)
programmer = Programmer.new('Aaron', 25)

person.is_a?(Person) # => true
person.is_a?(Programmer) # => false

programmer.is_a?(Person) # => true
programmer.is_a?(Programmer) # => true
```

### Re-defining initialize

We can do more work in `initialize`

```ruby
class Programmer < Person
  def initialize(name, age, options)
    super(name, age)
    @github = options[:github]
    @website = options[:website]
    @languages = options[:languages]
  end
end
```

Look familiar?


## Class Methods

Like `static` methods in JS

```ruby
class Person
  # ...
  def self.about
    puts 'People are people. We are all human'
  end
  # ...
```

We call these methods the way you would expect

```ruby
Person.about # => 'People are people. We are all human'
Programmer.about # => 'People are people. We are all human'
person.about # => NoMethodError: undefined method `about' for Person
programmer.about # => NoMethodError: undefined method `about' for Person
```

## Lab



## Resources

* [`Class` tutorial](http://ruby-for-beginners.rubymonstas.org/writing_classes/definition.html)
  - Almost everything in here is good. Click around to learn more than just classes!
* [Ruby in 20 Minutes](https://www.ruby-lang.org/en/documentation/quickstart/2/)
* [Previous Lecture](https://git.generalassemb.ly/wdi-nyc-hamilton/LECTURE_U04_D01_Ruby_101)


## Conclusion
- How are Ruby classes different from Javascript "classes"
- What are ivars used for? How do expose them outside the class?
- How do we create new instances of a class? What methods are called when we create them?
