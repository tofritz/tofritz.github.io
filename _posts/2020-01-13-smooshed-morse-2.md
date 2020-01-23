---
layout: post
title:  "smooshed morse code - challenge 2 (recursion) "
author: tyler
date:   2020-01-13 20:00:00 -0600
categories: practice python
---

This challenge was taken from [this post](https://www.reddit.com/r/dailyprogrammer/comments/cn6gz5/20190807_challenge_380_intermediate_smooshed/) and uses this [list of inputs](https://gist.github.com/cosmologicon/415be8987a24a3abd07ba1dddc3cf389#file-smorse2-bonus1-in) for the bonus.

### Challenge Prompt

A *permutation* of the alphabet is a 26-character string in which each of the letters `a` through `z` appears once.

Given a smooshed Morse code encoding of a permutation of the alphabet, find the permutation it encodes, or any other permutation that produces the same encoding (in general there will be more than one). It's not enough to write a program that will eventually finish after a very long period of time: run your code through to completion for at least one example.

#### Examples

```python
smalpha(".--...-.-.-.....-.--........----.-.-..---.---.--.--.-.-....-..-...-.---..--.----..")
    => "wirnbfzehatqlojpgcvusyxkmd"
smalpha(".----...---.-....--.-........-----....--.-..-.-..--.--...--..-.---.--..-.-...--..-")
    => "wzjlepdsvothqfxkbgrmyicuna"
smalpha("..-...-..-....--.---.---.---..-..--....-.....-..-.--.-.-.--.-..--.--..--.----..-..")
    => "uvfsqmjazxthbidyrkcwegponl"
```

Note: decoding is ambiguous

#### Challenges

1. How fast can you find the output for 1000 unique inputs?

#### Solution

This challenge took much more effort than the initial morse code challenge. The resulting struggle introduced me to some new concepts --- primarily the use of generators and how they can be implemented to do recursive searching by backtracking. My thought process during my initial attempts centered around solving this problem by counting the frequency of the morse-encodings for each letter to determine a priority of replacing them within the smorse string.

```python
morseFreq = {morse: 0 for morse in morseDict.keys()}  # counter


def reset():
    for morse, freq in morseFreq.items():
        morseFreq[morse] = 0 # reset freqs to zero


def count(smorse: str):
    for morse, freq in morseFreq.items():
        morseFreq[morse] = smorse.count(morse)


def smalpha_freq(smorse: str):
    counter = 26 # need to replace 26 letters

    while counter > 0:
        count(smorse) # how many times does a letter appear in smorse
        rankFreq = list(morseFreq.items()) 
        rankFreq.sort(key=lambda x: x[1]) # list and sort by freq
        smorse = smorse.replace(rankFreq[0][0], morseDict[rankFreq[0][0]], 1) # replace lowest freq
        del morseFreq[rankFreq[0][0]] # remove from dict
        del rankFreq
        reset() # reset freq counts for remaining letters
        counter -= 1

    return smorse


print(smalpha_freq(test))
# jbmcvtlhiogsqf-uye.pr--kxa...z-
```

The output made apparent that this approach was not sufficient. Often, there would be multiple letters having the same lowest frequency, which made it ambiguous which letter would be best to replace at that step. Or replacement would leave encoding characters that only corresponded to letters that had  already been replaced previously. This coudl only work if at any moment of replacement there was only one occurance of a letter encoding. I knew that I would need some sort of recursive function that could do a similar replacement method but be able to start over if the remaining morse in the smorse string did not correspond with letters that had yet to be replaced.

After some googling, I came across the topic of generators and their ability to yield outputs without closing the function and losing all the information stored within the scope of that function. My eventual solution involved nesting a generator function within itself. The generator pretty much solves the smorse string in a brute force manner, but is able to backtrack to a previous state if it runs into a situation where there is no valid morse left in the string that is associated with any non-replaced letter.

```python
from string import ascii_lowercase
import time

morse_alphabet = ".- -... -.-. -.. . ..-. --. .... .. .--- -.- .-.. -- -. --- .--. --.- .-. ... - ..- ...- .-- -..- -.-- --..".split()
morse_to_char = dict(zip(morse_alphabet, ascii_lowercase))
char_to_morse = dict(zip(ascii_lowercase, morse_alphabet))
found = []


def recurse(smorse, remaining=set(morse_alphabet)):
    if not smorse:
        yield found
        return

    for morse in remaining:
        if morse == smorse[:len(morse)]:
            found.append(morse)
            trim = remaining.copy()
            trim.remove(morse)
            # recurse  with trimmed set once morse found
            yield from recurse(smorse[len(morse):], trim)
            # backtrack if no morse in set remaining match next smorse
            found.pop()


def smalpha(smorse):
    for found in recurse(smorse):
        return ''.join(morse_to_char[morse] for morse in found)


def encode(string: str):
    return ''.join(char_to_morse[char] for char in string)


def check(input):
    global found
    found.clear()
    output = smalpha(input)
    assert encode(output) == input
    assert set(output) == set(ascii_lowercase)
    return output


def challenge():
    check('.--...-.-.-.....-.--........----.-.-..---.---.--.--.-.-....-..-...-.---..--.----..')
    check('.----...---.-....--.-........-----....--.-..-.-..--.--...--..-.---.--..-.-...--..-')
    check('..-...-..-....--.---.---.---..-..--....-.....-..-.--.-.-.--.-..--.--..--.----..-..')
    return print('Challenge Passed!')


def bonus1():
    start = time.time()
    with open('inputs.txt', 'r') as file:
        for line in file:
            smorse = line.rstrip()
            check(smorse)
    end = time.time()
    print(
        f'Optional bonus 1 complete! Time elapsed: {end - start:0.2f} seconds.')


challenge()
bonus1()
```

The above script was able to process the 1000 smorse inputs in 75.57 seconds. Much faster times have been reported using languages such as Rust, Go, C, Julia, etc. Other solution methods I've seen include implementing a search [trie](https://www.geeksforgeeks.org/trie-insert-and-search/) string as morse encoding lends itself to this type of search:

![alt-text](https://www.researchgate.net/profile/Cleve_Moler/publication/265536380/figure/fig49/AS:669567332388876@1536648700580/The-binary-tree-defining-Morse-code-A-branch-to-the-left-signifies-a-dot-in-the-code.png)