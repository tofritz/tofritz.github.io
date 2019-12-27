---
layout: post
title:  "My first post."
date:   2019-12-23 16:35:26 -0600
categories: jekyll update
---
# Welcome

This is my first post to my personal webpage. I am using the `jekyll` static site-publishing system which builds my webpage each time that I push new commits from my repository to github.

I currently plan on having this website serve as a sort-of online resume as well as a blog which can keep interested people apprised of my activities. I think it will be a useful tool in temporally documenting my progress as well as serving as a tool to improve my front-end web development skills as I tweak and personalize it.


A first exercise
---
For my first post, I've decided to keep it simple and just work an easy practice coding problem:

>The game of Yahtzee is played by rolling five 6-sided dice, and scoring the results in a number of ways. You are given a Yahtzee dice roll, represented as a sorted list of 5 integers, each of which is between 1 and 6 inclusive. Your task is to find the maximum possible score for this roll in the upper section of the Yahtzee score card. Here's what that means. For the purpose of this challenge, the upper section of Yahtzee gives you six possible ways to score a roll. 1 times the number of 1's in the roll, 2 times the number of 2's, 3 times the number of 3's, and so on up to 6 times the number of 6's. For instance, consider the roll [2, 3, 5, 5, 6]. If you scored this as 1's, the score would be 0, since there are no 1's in the roll. If you scored it as 2's, the score would be 2, since there's one 2 in the roll. Scoring the roll in each of the six ways gives you the six possible scores: 0 2 3 0 10 6 The maximum here is 10 (2x5), so your result should be 10.

Here is my solution:
```python
# First solution
def yahtzee(rolls):
    counts = dict.fromkeys(rolls, 0)
    for roll in rolls:
        for key in counts:
            if key == roll:
                counts[key] += roll
    values = list(counts.values())
    return max(values)

# Stick with lists and use list comprehension for a oneliner
def onelineyahtzee(lst):
    return max([lst.count(x) * x for x in lst])
```