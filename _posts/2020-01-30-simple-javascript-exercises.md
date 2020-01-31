---
layout: post
title:  "simple javascript exercises"
author: tyler
date:   YYYY-MM-DD HH:MM:SS -0600
categories: odin javascript
---

I've encountered some smaller exercises in the sub-chapters on the fundamentals of JavaScript during my progress in the `Web Dev 101` coursework of the Odin Project. I've included my solutions here and added the functions that may have usefulness to my `functions.js` "library" in my `scripts` repository. These were pretty simple, but I could have constructed some more elegantly with better formatting of my conditional logic. These exercises were a good way to introduce the `jasmine` JavaScript testing framework. It's much more efficient to design a battery of tests to check the output of a script than just freestyling something while concurrently feeding it inputs to see if its behaving the way you hope.

```javascript
const repeatString = function(string, number) {
  if (number < 0) return "ERROR";
  let output = "";
  for (i = 0; i < number; i++) {
    output += string;
  }
  return output;
};

const reverseString = function(string) {
  let array = string.split("");
  let output = array.reverse();
  return output.join("");
};

const leapYears = function(number) {
  if (number % 4 == 0) {
    if (number % 100 == 0) {
      if (number % 400 == 0) {
        return true;
      }
      return false;
    }
    return true;
  }
  return false;
};

const ftoc = function(numFarenheight) {
  numCelsius = (numFarenheight - 32) * (5 / 9);
  return Math.round(numCelsius * 10) / 10;
};

const ctof = function(numCelsius) {
  numFarenheight = numCelsius * (9 / 5) + 32;
  return Math.round(numFarenheight * 10) / 10;
};

const sumAll = function(from, to) {
  sum = 0;
  if (typeof from == "number" && from > 0) {
    if (typeof to == "number" && to > 0) {
      if (from < to) {
        for (i = from; i < to + 1; i++) {
          sum += i;
        }
      } else {
        for (i = to; i < from + 1; i++) {
          sum += i;
        }
      }
    } else {
      return "ERROR";
    }
  } else {
    return "ERROR";
  }
  return sum;
};
```