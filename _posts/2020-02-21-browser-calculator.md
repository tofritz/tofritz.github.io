---
layout: post
title:  "a calculator in the browser"
author: tyler
date:   2020-02-21 18:00:00 -0600
categories: javascript odin html css
---

The [`calculator`](https://www.github.com/tofritz/calculator) repository houses the code which is hosted live [here](https://tofritz.com/calculator). See if you can break my calculator --- I'm sure there are a few ways to accomplish that. Let me know what you find out. I think I've already found one (try hitting the plus/minus operator after hitting some operator buttons).

This project was built using techniques that were introduced in the past few projects but using concepts introduced in practice exercises found [here](https://github.com/tofritz/example-work/tree/master/odin-project-exercises/javascript-exercises) and [here](https://github.com/tofritz/example-work/tree/master/javascript-30). Many of the solutions I've seen for this project using event listeners to check for button presses that will then append text content to an array which is then parsed to evaluate an expression by using logic to follow correct order of operations on the array of strings (which are dynamically cast by javascript to numbers). My approach was different in that I mainly used three variables: a previously displayed value, a currently loaded value, and the last operator indicated. Even just using these three variables, I was able to mimic the functionality set out in the project [description](https://www.theodinproject.com/courses/web-development-101/lessons/calculator?ref=lnav). I had to also implement some boolean flags to disable/enable buttons and certain logic to make sure the calculator kept a running tally if numerous operations were strung together without pressing the evaluation button.

I was able to complete the project and most of the 'Extra Credit' functionalities that were proposed. However, I have not yet sucessfully implemented keyboard support. I understand how to set up event listeners to listen for the keycodes of keyboard presses which are associated with operators or numbers, but I currently have written my script using functions that listen for MouseEvents so when I try to call these functions on keyboard presses, I get errors where I try to reference attributes that do not exist for keyboard events. I think I can work around this by adding logic to each function that will divert to two different methods of aquiring the operator/button information depending on the event type.

One of the downsides to the approach I took is that there is no recording of the calculation history. Other implementations let the user input and build a complex expression, whereas my implementation only will keep a running evaluation of the most recent attempted operation.

```javascript
/* DOM selectors */

const calcDisplay = document.querySelector(".calc-display");
const numButtons = document.querySelectorAll(".button-num");
const dotButton = document.querySelector(".button-dot");
const opButtons = document.querySelectorAll(".button-op");
const evalButton = document.querySelector(".button-eval");
const posnegButton = document.querySelector(".button-posneg");
const clearButton = document.querySelector(".button-clear");
const delButton = document.querySelector(".button-del");

/* display text & functions */

let currentDisplayVal = "";
let previousDisplayVal = "";

const updateDisplayText = text => (calcDisplay.textContent = text);

/* calculator functions & variables */

const add = (a, b) => a + b;
const sub = (a, b) => a - b;
const mul = (a, b) => a * b;
const div = (a, b) => a / b;

const operate = (operator, a, b) => {
  if (operator === "+") return add(a, b);
  if (operator === "-") return sub(a, b);
  if (operator === "×") return mul(a, b);
  if (operator === "÷") return div(a, b);
};

let currentOperator;
let currentResult;
let resultFlag = false;

/* state functions */

checkDisplayValsExist = () => {
  return currentDisplayVal != "" && previousDisplayVal != "";
};

disableEvalButton = () => {
  evalButton.style.pointerEvents = "none";
  resultFlag = true;
};

enableEvalButton = () => {
  evalButton.style.pointerEvents = "auto";
};

checkResultPossible = () => {
  return (
    currentDisplayVal != "" && previousDisplayVal != "" && currentOperator != ""
  );
};

disableButtons = () => {
  document.querySelectorAll(".calc-button").forEach(button => {
    button.style.pointerEvents = "none";
  });
  clearButton.style.pointerEvents = "auto";
};

enableButtons = () => {
  document.querySelectorAll(".calc-button").forEach(button => {
    button.style.pointerEvents = "auto";
  });
};

function evaluateValues() {
  if (currentOperator === "÷" && currentDisplayVal == 0) {
    updateDisplayText("This kills the calculator");
    return;
  }
  currentDisplayVal = String(
    operate(
      currentOperator,
      Number(previousDisplayVal),
      Number(currentDisplayVal)
    )
  );

  updateDisplayText(currentDisplayVal);
}

/* event functions */

function numButtonPress(event) {
  if (resultFlag) {
    //if the result button is previously hit, clear values to start
    resultFlag = false;
    currentDisplayVal = "";
    currentOperator = "";
  }
  currentDisplayVal += event.target.textContent.trim();
  updateDisplayText(currentDisplayVal);

  if (checkDisplayValsExist()) {
    enableEvalButton();
  }
}

function opButtonPress(event) {
  if (resultFlag) {
    //if the result button is previously hit, reset the values so the op button doesnt evaluate
    resultFlag = false;
    previousDisplayVal = "";
    currentOperator = "";
  }
  if (checkResultPossible()) {
    evaluateValues();
  }
  currentOperator = event.target.textContent.trim();
  if (currentDisplayVal != "") previousDisplayVal = currentDisplayVal;
  currentDisplayVal = "";
}

function evalButtonPress(event) {
  evaluateValues();
  disableEvalButton();
}

// figure out bug here when pressed after opkey

function posnegButtonPress(event) {
  currentDisplayVal *= -1;
  currentDisplayVal = currentDisplayVal.toString(); // retype to string
  updateDisplayText(currentDisplayVal);
}

function clearButtonPress(event) {
  disableEvalButton();
  currentDisplayVal = "";
  previousDisplayVal = "";
  currentOperator = "";
  previousOperator = "";
  updateDisplayText(currentDisplayVal);
}

function delButtonPress(event) {
  currentDisplayVal = currentDisplayVal.slice(0, -1);
  updateDisplayText(currentDisplayVal);
}

function dotButtonPress(event) {
  if (currentDisplayVal.includes(".")) {
    currentDisplayVal = currentDisplayVal.replace(".", "");
  }
  currentDisplayVal += ".";
  updateDisplayText(currentDisplayVal);
}

/* event listeners */

numButtons.forEach(button => button.addEventListener("click", numButtonPress));
opButtons.forEach(button => button.addEventListener("click", opButtonPress));
dotButton.addEventListener("click", dotButtonPress);
evalButton.addEventListener("click", evalButtonPress);
posnegButton.addEventListener("click", posnegButtonPress);
clearButton.addEventListener("click", clearButtonPress);
delButton.addEventListener("click", delButtonPress);

/* initialize states */
disableEvalButton();
```