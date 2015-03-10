Block Challenges
================

A block is a piece of code that can be executed later.
Its behaviour is very very similar to a method's.
You will know when you see a block, because it is always passed to a method,
and it has either curly braces or `do/end` wrapped around it.
Like `{ this }` and like `do this end`.

<h2>Passing/calling a block</h2>

We pass a block to our code the same way we pass it to
`#each` and `#map` and anywhere else that you've seen a block.

We receive the block with an ampersand.
You can think of it like "& I'd like a block".

We invoke the block with `#call`.
In the same way we can call a method and pass it arguments,
we can pass arguments to the block by passing them to `#call`.

<div class="interactive-code">class MahClass
  def gimme_numbahz(&block)
    block.call 123
    block.call 456
    block.call 789
  end
end

ary = [1, 2, 3]
ary.each do |n|
  puts "Array#each gave me: #{n}"
end

mah_instance = MahClass.new
mah_instance.gimme_numbahz do |n|
  puts "One of mah numbahz: #{n}"
end</div>


<h2>Every method takes a block</h2>

Whether the method says so or not, it takes a block.

<div class="interactive-code">def noblock_method
end
noblock_method { puts "noblock_method" }

def block_method(&block)
end
block_method { puts "block_method" }

def block_call(&block)
  block.call
end
block_call { puts "block_call" }</div>

<h3>Experiments!!</h3>

What would happen if you called the block twice in `block_call`?

What would happen if you passed a block to `puts`,
`local_variables`, and `Object.class`?
Why did that happen?


<h2>Normal arguments and a block</h2>

Blocks are declared as the last argument.

Can you define `have_some_args` so that it doesn't blow up?

<div class="interactive-code">have_some_args(1, 2, 3) { 4 }
</div>


<h2>Blocks aren't counted with arity</h2>

Arity is the number of arguments that a method takes.

What do you think the arity will be for these methods?
Go ahead and guess,
then mimic the first line against the other methods to find out if you're right.

The last one is a little tricky,
read the title of this section before you answer it :)

<div class="interactive-code">def a
end
puts method(:a).arity

def b(arg)
end

def c(arg1, arg2)
end

def d(arg1, arg2, &block)
end</div>

Can you think of any other arugment types?
Are you able to guess how they affect arity?
Were any of your guesses wrong?
If so, can you think of a reason you might have gotten a different answer than you expected?


<h2>There can only be one block argument</h2>

Ruby only allows you to pass one block.
This is built into [the structure of a method call](https://github.com/ruby/ruby/blob/c5c5e96643fd674cc44bf6c4f6edd965aa317c9e/vm_core.h#L164).

For a bit about the history of why we have blocks,
and why they are the way they are, see [this](http://devblog.avdi.org/2015/01/16/why-does-ruby-have-blocks/)
great blog post by Avdi.

I'd try showing an example, but I'm not even sure what it would look like to try it.

There was a pretty cool [Code Brawl](http://codebrawl.com/contests/methods-taking-multiple-blocks)
a while back where everyone submitted code to support the use case of multiple blocks.
Some cool stuff in those gists :)

<h2>Block param when no block is given</h2>

Take a guess what you think this will be.
Try passing a block, what do you think it will be?

<div class="interactive-code">def m(&block)
  p block
end

m</div>

<h2>You can store blocks in a Proc</h2>

So, you can't actually do anything with a block,
because it's not really an object (hence why you can only pass it to a method).
This is, of course, problematic, so they created a class whose job is to wrap the block.
That class is called `Proc`, and you can execute the code in the block with
`Proc#call`. You can get access to one by taking it in a method,
or by instantiating a `Proc` with a block, or by calling `lambda`,
which is a private method available to all objects.

<div class="interactive-code">def get_block(&block)
  block
end

block1 = get_block { puts 'block 1' }
puts block1.class
block1.call

# can you show that these create Proc objects
# And can be invoked with `Proc#call`, like the one above?
# Proc.new { }
# lambda {  }</div>


<h2>Alternative way to work with blocks</h2>

What we did above, is my favourite way to work with blocks,
because it handles everything that a block can do.
But there is another syntax for working with blocks,
you'll often see this in [Rails templates](https://github.com/rails/rails/blob/221e847a3bc4b6dc2559b4354417862b2e6c684a/railties/lib/rails/generators/rails/app/templates/app/views/layouts/application.html.erb.tt#L20).

This syntax never turns the block into a local variable.
Instead, you can use `block_given?` to see if the block exists,
and `yield` to call the block.

<div class="interactive-code">def keywords_with_blocks
  if block_given?
    yield
  else
    puts "No block given!"
  end
end

keywords_with_blocks
keywords_with_blocks { puts "Block was called" }</div>

<h3>Experiments</h3>

What would happen if you yielded to the block when it wasn't given?

How could you check whether the block was given if you received it in a parameter?
Try it!

Does this code work the same if you receive the block in a parameter
like we've been doing above?


<h2>Return values</h2>

Blocks, like methods (and basically all expresions in Ruby)
return the value of the last line.

<div class="interactive-code">block = lambda do
  123
  456
end
puts "Last line: #{block.call}"</div>

<h3>Experiments</h3>

Figure out if this will work the same for `yield`.

What would get returned if the block had nothing in it?
Try it to make sure you're right!


<h2>Blocks don't give a shit about arguments</h2>

Blocks, like methods, can receive arguments.

<div class="interactive-code">def gimme_moar_numbahz!(&block)
  block.call(10)
  block.call(100)
  block.call(1000)
end

gimme_moar_numbahz! { |n| puts n + 1 }</div>

Unlike methods, blocks don't care if you give the correct number of argumetnts.
This is kind of like a JavaScript function.
The case where we call it with `:a` and `:b`, it's pretty clear what to expect.
But what about when we give it fewer or more?
What do you expect to see? Are you correct?
Can you explain what you're seeing?

<div class="interactive-code">def moar_args_pls(&block)
  block.call()
  block.call(:a)
  block.call(:a, :b)
  block.call(:a, :b, :c)
end

moar_args_pls do |arg1, arg2|
  puts "My args: #{arg1.inspect}, and #{arg2.inspect}"
end</div>


<h3>Experiments</h3>

How are a block's parameters stored?
Here are some potentially useful methods you can try calling,
in order to check if you are right:
`global_variables`, `local_variables`, `instance_variables`, `Object.class_variables`

If you figured out the one above, is it true for method parameters, too?

Is this still true if you use `yield` instead of `block.call`?

Can a block receive optional arguments?

Can a block receive... a block?!?
(did you think about [this](https://www.youtube.com/watch?v=IXLDv-fUINM), too?)


<h2>Destructuring arguments</h2>

Get ready to "whaaaaa?! O.o"
So, block assignment is like local variable assignment.
Here, we can use destructuring to extract arguments from an array.

<div class="interactive-code">a = [1, 2]
p a

a, b = [1, 2]
p a
p b</div>

You can have multiple layers of destructuring through parentheses.

<div class="interactive-code">(a, (b, c), (d, e)) = [1, [2, 3], [4, 5]]
p a, b, c, d, e</div>

<h3>Thoughts</h3>

Where might something like this be useful?

<h3>Experiments</h3>

Hashes are key/value pairs. These are passed to the block as an array.
Show that you can receive the pair if you want (the array),
or use destructuring to get the key/value pair.

Does this work for keyword arguments, too?

Do methods work with this kind of assignment?

Can you destructure three levels deep?


<h2>Blocks can see their surrounding environment</h2>

This is one of the coolest things about blocks.
They can see variables in their surrounding environment.
We say that they "enclose" their environment,
which is why they are called "closures".

<div class="interactive-code">def add_one(&block)
  puts block.call(1)
end

local_var = 2
add_one { |arg| local_var + arg + 3 }</div>

<h3>Thoughts</h3>

Where have you seen things like this before?
(think about the Enumerable methods)

<h3>Experiments</h3>

Is this true for methods, too?

Based on the answer to the above question, what do you think will happen with this code?

<div class="interactive-code">var1 = 1
def add_one
  var2 = 2
  block = lambda { puts var1 + var2 + 3 }
  block.call
end
add_one</div>

Can blocks see instance variables, too?
If so... well, we know that instance variables are stored on the instance,
so what does that imply that `self` is?
Were you correct?
Try this in a few different contexts... does it hold?

<h2>Curly braces vs do/end</h2>

For the most part, people treat curly braces and do/end as if they are interchangeable.
But there is one tricky difference. Curly braces will be passed as an argument to the
method call directly to their left. But `do/end` will be passed as an argument to the
method call farthest to their left.

<div class="interactive-code"># The asterisk causes the method to ignore any arguments that are given to it
# Not part of this material, but can you think of why this might be?
def a(*)
  puts "a: #{block_given?}"
end

def b(*)
  puts "b: #{block_given?}"
end

def c(*)
  puts "c: #{block_given?}"
end

a b { }
puts "----------"
a b do
end
</div>


<h3>Experiments</h3>

What if you called `a b c` with curly braces and do/end?
Which method do you think they would be assigned to?
Try it to see if you are right :)

If you wanted to use curly braces,
but didn't want the block to go to the method direclty to their left,
what could you do to get them to be passed to the correct method?

You've seen this before.
Think about Rake tasks, have you ever seen curly braces in a rake task?
If not, why not, and can you find a way to use curly braces instead of do/end?
Think about RSpec test suites, can you define a test that uses curly braces,
and another that uses do/end?


<h2>Lambda blocks vs Proc blocks vs Arrows</h2>

So, some of the above is a bit of a lie.
See, "Proc blocks" can behave as you've seen so far...
but "Lambda blocks" behave like methods.
You can find out what kind you have with the `lambda?` method:

<div class="interactive-code">puts Proc.new { }.lambda?
puts lambda { }.lambda?
puts -> { }.lambda?</div>

There are two ways that they can differ.
The first is where return goes to.
The second is how they deal with arguments.

I'll show you how I would experiment to figure out how the return value works:
See if you can't figure out how to verify the argument one
(have you seen "wrong number of arguments" before? What did you do to get that?
try doing that for the block and proc to see how they behave).

<div class="interactive-code">def proc_return
  Proc.new { return :from_block }.call
  :from_method
end

def lambda_return
  lambda { return :from_lambda }.call
  :from_method
end

puts proc_return
puts lambda_return</div>

<h3>Experiments</h3>

What do you think this will return? `method(:puts).to_proc` can you prove it?

Do both of these attributes (where return returns from, and how arguments match up)
hold for both `lambda` and `->`?

What if you define a method from a block?
If you define a method from a block, can you get a method that behaves like a proc instead of a lambda?

There's another method that does a similar thing: `proc` how does this one behave?
If you have access to Ruby 1.8, run these same experiments there... do you get the same results?


<h2>Passing blocks from a local variable</h2>

Sometimes you have a block as a Proc in a local variables,
and you have a method that wants a block.
You can pass the block using the `&syntax` where you invoke it.

What do you expect this to print?

<div class="interactive-code">def block1(&block)
  block2(&block)
end

def block2(&block)
  block.call(1)
end

block1 { |n| puts "Got #{n}" }</div>


<h3>Experiments</h3>

If you passed the block as a local variable, can the next method invoke it with yield?

Can you do this with both Lambda style and Proc style blocks?

Does this work with local variables you defined through `Proc.new` and `lambda`?

Can you pass these blocks to things like `Array#map`?

What will happen here? `[1,2,3].each(&method(:puts).to_proc)`

What if you ran the above example without the `to_proc` method?
Why might that have worked or not worked?
Can you think of a way to check this?


<h2>Bindings return a binding into the block's closure</h2>

There is a method that all objects inherit, called `binding`,
which returns the current binding. This is where local variables are stored,
and what determines what `self` is.

This is made private, because it's implemented in such a way that implementation
details leak out and it makes no goddam sense:

<div class="interactive-code">puts Object.new.send(:binding).eval('self')
</div>

But you can call `binding` on a block, and it will give you a binding into its closure.

<div class="interactive-code">def print_block_locals(&block)
  p block.binding.eval('local_variables')
end

a, b = 1, 2
print_block_locals { }</div>

<h3>Experiments</h3>

If there were local variables in the block above,
would it print them, as well?

Find a way to get two different objects (you can check with `object_id`)
to have a reference to the same block. Is their `self` the same?
(You'll need to `eval("self")` on the block's binding, the same way we eval'd `local_variables`).

<h2>You can change the value of `self`</h2>

So here's some black fucking magic for you.
And I'm not telling you to go do this, because, I'm not your parents.
But I want what's best for you, so I'll just tell you that metaprogramming is the devil's work,
but your other instructors and I, we know that you're curious about these things,
and all your friends are experimenting with metaprogramming.
So, we'd rather you experiment here in school, where it's safe.

So, we've seen `eval`, which, you've probably heard is evil,
but if you really wanted to turn it up to eleven, then read on.
A block [has a reference to self](https://github.com/ruby/ruby/blob/ab38e5b5851288753e841182f112d778d713dc91/vm_core.h#L524),
So if Ruby exposed a way to change the value of self for a block... well...

<div class="interactive-code">"abc".instance_eval { p self }
123.instance_eval { p self }</div>

Now, you'll quickly find yourself frustrated because you can't pass arguments to the block.
There's a decent solution available, though. Try calling `methods` on the block,
do you see any that look promising? (their names are similar to `instance_eval`,
and you can filter the results by calling `grep` on them and passing it a regex).
Can you find the method? Can you find its limitations? [Here's a hint](https://www.youtube.com/watch?v=IXLDv-fUINM).

**WARNING** The following material may has been known to cause epistemological unsettledness.

And now, if you really want to revel with the devil,
figure out what the difference is between `instance_eval` and `class_eval/module_eval`.
And then, to truly stare into the abyss,
figure out how Ruby is able to accomodate these differences.
Be sure to say goodbye to your loved ones.
For one day, dear student, you may wake up wide-eyed and realize that metaprogramming is just programming.
And all that shit you thought was programming? All those keywords?
The `class`, the `module`, the `def`... how in the fuck do they pull that off?
And then you will see: everything you believed is a lie.


<h2>The transition from Proc to block and back remembers what object it is</h2>

Here's some more magic for you. When ou pass a Proc through the block slot,
it retains its idenity on the other side.

<div class="interactive-code">def m1(&b)
  p b.object_id
  m2(&b)
end

def m2(&b)
  p b.object_id
end

m1 { }
</div>


<h2>You can turn a method into a block, but not an unbound method</h2>

So here's one that will seem strange at first.
think about why this might be.

<div class="interactive-code">def m(&b)
  p b.call
end

# the method
m(&method(:object_id))

# the instance method bound to main
m(&method(:object_id).owner.instance_method(:object_id).bind(self))

# the unbound instance method
m(&method(:object_id).owner.instance_method(:object_id))</div>


<h2>Some challenges!</h2>

Create a class that is initialized with a block,
saves it in an instance variable,
and doesn't invoke it until you call `omghi` on it.

---

Fill in the body of `b2` to get it to print 3.

<div class="interactive-code">b1 = lambda { 2 }
b2 = lambda {  } # what can you put in the block to get it to print 3?

puts b2.call(1, &b1)</div>

---

Write your own map method: It takes an array and a block.
Each item in the array is passed to the block.
The method returns an array of items that were returned by the block.

---

Write your own select method: It takes an array and a block.
Each item in the array is passed to the block.
The method returns an array of items for which the block returned true.
