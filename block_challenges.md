# Block Challenges

A block is a piece of code that can be executed later.
Its behaviour is very very similar to a method's.
You will know when you see a block, because it is always passed to a method,
and it has either curly braces or `do/end` wrapped around it.
Like `{ this }` and like `do this end`.

## Passing/calling a block

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

## Every method takes a block

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

### Experiments!!

What would happen if you called the block twice in `block_call`?

What would happen if you passed a block to `puts`,
`local_variables`, and `Object.class`?
Why did that happen?


## Normal arguments and a block

Blocks are declared as the last argument.

Can you define `have_some_args` so that it doesn't blow up?

<div class="interactive-code">have_some_args(1, 2, 3) { 4 }
</div>


## Blocks aren't counted with arity

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


## There can only be one block argument

Ruby only allows you to pass one block.
This is built into [the structure of a method call](https://github.com/ruby/ruby/blob/c5c5e96643fd674cc44bf6c4f6edd965aa317c9e/vm_core.h#L164).

For a bit about the history of why we have blocks,
and why they are the way they are, see [this](http://devblog.avdi.org/2015/01/16/why-does-ruby-have-blocks/)
great blog post by Avdi.

I'd try showing an example, but I'm not even sure what it would look like to try it.

There was a pretty cool [Code Brawl](http://codebrawl.com/contests/methods-taking-multiple-blocks)
a while back where everyone submitted code to support the use case of multiple blocks.
Some cool stuff in those gists :)

## Block param when no block is given

Take a guess what you think this will be.
Try passing a block, what do you think it will be?

<div class="interactive-code">def m(&block)
  p block
end

m</div>

## You can store blocks in a Proc

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


## Alternative way to work with blocks

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

### Experiments

What would happen if you yielded to the block when it wasn't given?

How could you check whether the block was given if you received it in a parameter?
Try it!

Does this code work the same if you receive the block in a parameter
like we've been doing above?


## Passing Arguments to blocks



## Return values

## Blocks don't give a shit about arguments
## Curly braces vs do/end
## Lambda blocks vs Proc blocks
## Passing blocks from a local variable
## Some challenges!
