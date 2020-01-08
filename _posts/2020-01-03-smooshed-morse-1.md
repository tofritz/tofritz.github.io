---
layout: post
title:  "smooshed morse code - challenge 1 "
author: tyler
date:   2020-01-03 20:00:00 -0600
categories: practice
---

This challenge was taken from [this post](https://www.reddit.com/r/dailyprogrammer/comments/cmd1hb/20190805_challenge_380_easy_smooshed_morse_code_1/) and uses the [enable1 word list](https://raw.githubusercontent.com/dolph/dictionary/master/enable1.txt).

### Challenge Prompt

For the purpose of this challenge, Morse code represents every letter as a sequence of 1-4 characters, each of which is either `.` (dot) or `-` (dash). The code for the letter `a` is `.-`, for `b` is `-...`, etc. The codes for each letter a through z are:
`.- -... -.-. -.. . ..-. --. .... .. .--- -.- .-.. -- -. --- .--. --.- .-. ... - ..- ...- .-- -..- -.-- --..`
Normally, you would indicate where one letter ends and the next begins, for instance with a space between the letters' codes, but for this challenge, just smoosh all the coded letters together into a single string consisting of only dashes and dots.

#### Examples

```python
smorse("sos") => "...---..."
smorse("daily") => "-...-...-..-.--"
smorse("programmer") => ".--..-.-----..-..-----..-."
smorse("bits") => "-.....-..."
smorse("three") => "-.....-..."
```

Note: decoding is ambiguous

#### Challenges

1. The sequence `-...-....-.--.` is the code for four different words (`needing`, `nervate`, `niding`, `tiling`). Find the only sequence that's the code for 13 different words.
2. `autotomous` encodes to `.-..--------------..-...,` which has 14 dashes in a row. Find the only word that has 15 dashes in a row.
3. Call a word *perfectly balanced* if its code has the same number of dots as dashes. `counterdemonstrations` is one of two 21-letter words that's perfectly balanced. Find the other one.
4. `protectorate` is 12 letters long and encodes to `.--..-.----.-.-.----.-..--.`, which is a palindrome (i.e. the string is the same when reversed). Find the only 13-letter word that encodes to a palindrome.
5. `--.---.---.--` is one of five 13-character sequences that does not appear in the encoding of any word. Find the other four.

#### Solution

```python
import string
morseList = ".- -... -.-. -.. . ..-. --. .... .. .--- -.- .-.. -- -. --- .--. --.- .-. ... - ..- ...- .-- -..- -.-- --.."
morseDict = dict(zip(string.ascii_lowercase, morseList.split()))
wordsDict = {word: encode(word) for word in words}

# function for encoding a given word into a string of morse.

def encode(word):
    return(''.join([morseDict[letter] for letter in word]))

print(encode('sos'))

with open('enable1.txt', 'r') as f:
    words = [line.rstrip() for line in f.readlines()]

# function for decoding a morse string into possible words.

def decode(query):
    return([word for (word, morse) in wordsDict.items() if morse == query])

print(decode('-....--....'))

# prompt 1: Find sequence that is code for n words.

def morseWithNumWords(num):
    morseFreq = dict.fromkeys(set(wordsDict.values()), 0)
    for word, morse in wordsDict.items():
        morseFreq[morse] = 1 + morseFreq.get(morse, 0)
    return([morse for morse, freq in morseFreq.items() if num == freq])


print(morseWithNumWords(13))  # '-....--....'

# prompt 2: Find word with n dashes in a row.

def wordWithDashes(dashes):
    dashString = '-' * dashes
    return([word for (word, morse) in wordsDict.items() if dashString in morse])


print(wordWithDashes(15))  # "bottommost"

# prompt 3: Find words that contain the same number of dots and dashes.

def balanced(chars):
    equal = []
    words = []
    for (word, morse) in wordsDict.items():
        if (morse.count('.') == morse.count('-')):
            equal.append(word)
    for item in equal:
        if len(item) == chars:
            words.append(item)
    return(words)


print(balanced(21))  # [counterdemonstrations, overcommercialization]

# prompt 4: Find morse palindrome with n characters.

def palindrome(chars):
    palindrome = []
    words = []
    for (word, morse) in wordsDict.items():
        if (morse == morse[::-1]):
            palindrome.append(word)
    for item in palindrome:
        if len(item) == chars:
            words.append(item)
    return(words)


print(palindrome(13))  # intransigence

# bonus 5: Find x-character morse sequences that do not appear in encoding of any word.

def missingSequence(chars):
    binarySequences = list(range(0, (2 ** chars) + 1)) # find all possible binary representations up to length n
    sequences = []
    for num in binarySequences:
        seq = bin(num).lstrip('0b').replace('1', '-').replace('0', '.') # filter list to n-length binaries and substitute 0,1 for .,-
        if len(seq) >= chars:
            sequences.append(seq)
    for morse in wordsDict.values(): # check if each seq is in morse and remove if it is. (much faster to remove, shortens subsequent array loops)
        if len(morse) >= chars:
            for seq in sequences:
                if seq in morse:
                    sequences.remove(seq)  

    return(sequences)


print(missingSequence(13))
# ['--.---.---.--', '--.---.------', '---.---.---.-', '---.---.-----', '---.----.----']
```
