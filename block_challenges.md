# Block Challenges

## Passing a block

We pass a block to our code the same way we pass it to
`#each` and `#map` and anywhere else that you've seen a block.

We receive the block with an ampersand.
You can think of it like "and then a block".

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
