---
layout: default
title: Why TDD?
---

# Why TDD?

Programming means teaching your computer something new. When we teach our computer, we want to know if it has understood what we just tried to explain. The way we do that is not unlike what we do when we teach a human being: We let them pass a test.

When we do TDD we now do what every good teacher should do: Learn about preexisting knowledge of our student and precisely prepare a curriculum. If we start with an empty file, our computer – or to be more precise Ruby – has some preexisting knowledge. It – for example – knows how to add two numbers. Therefore we don't need to put arithmetic into our curriculum. What our student doesn't know yet: We want to have a class `MyClass` with a certain behavior. So let us start with our curriculum or – as we call it in RSpec – **specification**.

You first want to check, if the class `MyClass` exists:

```ruby
describe MyClass do
end
```

This will first off fail. If it would not fail we would have done a bad job in determining the preexisting knowledge and would have to rethink our curriculum. But in this case it does, so let's teach the minimal amount necessary to fulfill this requirement:

```ruby
class MyClass
end

describe MyClass do
end
```

We now have 0 examples, but we don't have any errors anymore. Progress! Now let's teach our class to take an argument in its constructor and make it accessible:

```ruby
class MyClass
end

describe MyClass do
  it "has an attribute `a` that is accessible" do
    my_instance = MyClass.new(3)
    expect(my_instance.a).to eq 3
  end
end
```

(RSpec has tools to make this spec much nicer, you'll see them later on.)
This will now fail and tell you that you gave one argument instead of zero. Let's fix our implementation:

```ruby
class MyClass
  def initialize(a)
  end
end

describe MyClass do
  it "has an attribute `a` that is accessible" do
    my_instance = MyClass.new(3)
    expect(my_instance.a).to eq 3
  end
end
```

We now get another error, when we run the specs – there's no method `a` on the class. Let's fix that:

```ruby
class MyClass
  attr_reader :a

  def initialize(a)
  end
end

describe MyClass do
  it "has an attribute `a` that is accessible" do
    my_instance = MyClass.new(3)
    expect(my_instance.a).to eq 3
  end
end
```

We again get a new error. We now almost reached the minimal (non-nonsensical) implementation, but we still get the wrong value back:

```ruby
class MyClass
  attr_reader :a

  def initialize(a)
    @a = a
  end
end

describe MyClass do
  it "has an attribute `a` that is accessible" do
    my_instance = MyClass.new(3)
    expect(my_instance.a).to eq 3
  end
end
```

No matter how well you think you understand the problem you want to solve: TDD will help you clarify your thoughts. How exactly do you know, that your student has understood what you tried to teach it? In TDD or BDD we always write a piece of specification, try out if the program doesn't fulfill it yet and then write the minimum code needed to make that specification pass. This helps us to check, if our specification really specifies what we thought it would specify.

Imagine you would write a test after you write your implementation. You do some setup steps in your `it` block, then you get distracted. When you come back to your desk you have forgotten that you did not write your `expect(something)` line yet. These things happen – we're just humans. When you run your test, everything is green – you continue with your next implementation. But your specification **does not test** anything. If you had written the test before, your test would have passed even though you did not write any implementation. At this point you would have revised your specification. This is of course a simple example, but there are far more subtle ways that your specification does not test what you thought it would test.

## Refactoring

There's another reason why we write tests apart from understanding the problem better: The security that when we can part of our code and this changes the behavior of our program, we will be notified. This is useful if we enhance our code with new features, because we don't want those changes to break old code. But it's also essential in a practice called **refactoring**: Refactoring a piece of code means improving its structure (for example to reduce duplication or make it easier to understand) without changing its behavior. When you change your code and run your tests again and they still pass, you have not changed any existing behavior. You may still have introduced new features that are not covered by tests, but we will talk about this in [a later section](/ecosystem/metrics/).

Refactoring is often used in a process called *Red - Green - Refactor*:

1. **Red:** Write the specification and check that it is not fulfilled yet (the tests are *red*)
2. **Green:** Write the minimal necessary code to fulfill the specification (the tests are *green*)
3. **Refactor:** Change your code without changing its behavior (the tests stay *green*)
