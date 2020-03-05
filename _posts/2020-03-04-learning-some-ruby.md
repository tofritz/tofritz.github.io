---
layout: post
title:  "title goes here"
author: tyler
date:   YYYY-MM-DD HH:MM:SS -0600
categories: odin ruby practice
---

I have lately been working through the section of the Odin Project `Web Dev 101` devoted to introducing concepts around 'The Back End' of web development. Ruby and the platform Ruby on Rails are highly featured in some coursework curricula. This section of the `Web Dev 101` has a few modules introducing the basics of Ruby and follows up with a number of coding exercises to get used to the unique idioms, syntax, and style of this popular web development language. At this point, I am fairly comfortable with picking up new languages. The concepts are the same, only the particulars change around how those concepts are implemented. The introduction to this module begins by warning that this section may be skipped if the NodeJS curriculum is going to be undertaken later (which I plan on doing), but I decided to complete it for my own edification. I've compiled the completed Ruby exercise scripts in my `example-work` repository (direct link [here](https://github.com/tofritz/example-work/tree/master/odin-project-exercises/ruby)).

I've included some examples below to show some of the concepts covered (defining classes, methods, class methods, attribute accessors, ruby idioms, etc.)


```ruby
# defining a class, its methods, instance variables
class Timer
  
  attr_accessor :seconds

  def initialize
    @seconds = 0
  end

  def time_string
    hours = @seconds / 3600
    s_rem = @seconds % 3600
    padded(hours) + ':' + padded(s_rem / 60) + ':' + padded(s_rem % 60)
  end

  def padded(num)
    '%02i' % num
  end

end
```


```ruby
# defining a method (bonus regex practice)
def translate s
    result = []
    s.split.map do |word|
        case word
        when /^[aeiou]/ then result << word + 'ay'
        when /^([^aeiou])([qu]+)(.*)$/ then result << $3 + $1 + $2 + 'ay'
        when /^([^aeiou]+)(.*)$/ then result << $2 + $1 + 'ay'
        end
    end
    result.join(' ')
end
```


In addition to the exercises from the `learn_ruby` repository, I solved the first three [Project Euler](https://projecteuler.net/) problems using Ruby. Here is the third:
```ruby
def largest_prime_factor number
    if number < 1  
        puts 'enter a number greater than 1' 
    end
    factors = [1, ] # always a factor
    while number % 2 == 0 # see how many times 2 a factor
        factors.push(2)
        number /= 2 # update number as factor found
    end
    for i in (3..Math.sqrt(number)).step(2) do # check all odd numbers up to updated number
        while number % i == 0 # if no remainder, push to array and update number
            factors.push(i)
            number /= i
        end
    end
    factors
end

puts largest_prime_factor(600851475143).last
```