---
layout: post
title:      "Recursive Loops"
date:       2019-12-26 20:02:27 -0500
permalink:  recursive_loops
---


What we have learned so far in Ruby is Iterative Looping, whereby each "loop" is an interation of the code, and depending on the preset paremeters, your code would "loop" through each iteration of the code until the parameters are set.  For example with a while loop, the code executes while the arguement (parameter) remains true:

```
i = 0

while i < 4
i += 1

puts "The count is currently #{i}"

end

```

While learning about loops in Ruby and other programming languages, you may come across a "Recursive" loop which is similar except that the "loop" contains a call back to the entire function itself.  Recursion as it is called is sometimes defined as a concept (or program) where the problem is solved by calling functions within their own code.

A useful example of a recursive loop is finding factorials where you would need to determine what the product of an integer (and all the integers below it) is:

4! = 24

```
def factorial(num)

  if num == 0 || num == 1
return 1

  else 
return num * factorial(num - 1)
  end
end 

factorial(4)

```

The recursive loop calls itself to complete the function but with the next integer below the original so that factorial(4) breaks down as such:

factorial(4) = 4 * factorial(3) 
factorial(3) = 3 * factorial(2)
factorial(2) = 2 * factorial(1) 
factorial(1) = 1


So:

Factorial(2) = 2 * 1 
Factorial(3) = 3 * 2 * 1 
Factorial(4) = 4 * 3 * 2* 1




