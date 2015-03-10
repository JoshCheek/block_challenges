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

<div class="interactive-code">def a
end
puts method(:a).arity

def b(arg)
end

def c(arg1, arg2)
end

def d(arg1, arg2, &block)
end</div>

## There can only be one block argument

## Block param when no block is given
## Alternative way to call a block
## Blocks don't give a shit about arguments
## Curly braces vs do/end
## Lambda blocks vs Proc blocks
## Some challenges!
