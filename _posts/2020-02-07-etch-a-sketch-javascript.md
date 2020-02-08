---
layout: post
title:  "another week of javascript: etch-a-sketch"
author: tyler
date:   2020-02-07 18:00:00 -0600
categories: odin javascript
---

This week I have continued focusing on Javascript in my `Web Dev 101` coursework of the Odin Project. I started the week with some simple exercises to learn more about the Document Object Model and how Javascript interacts with HTML and CSS through Event Listeners, such as clicking a button or mousing over elements. My solutions to these simple exercises can be found in the relevantly-named folder in my `example-work` repository. These exercises helped me understand the concepts to a basic level which I put into use by adding some GUI elements to my `rock-paper-scissors` Javascript implementation. The current version can be found [here](https://tofritz.com/rock-paper-scissors).

To fully get comfortable with DOM concepts and using Javascript to manipulate CSS and HTML, I embarked on an attempt to construct a Javascript-powered Etch-A-Sketch. Putting together this script helped me better understand a number of things: 

* selecting and inserting HTML tags using classes and ids
* constructing functions in javascript and variable scoping
* creating different types of event listeners
* manipulating rgb values programatically
* using the css grid layout system

I had some frustrations building this out, but those mostly came from the searching on how to lexically describe in Javscript syntax what I knew I wanted to do conceptually. I also approached the problem of shading/darkening a background color in a few different ways. I thought utilizing the HCL color system would be more elegant, but I ended up going the route of just modifying RGB because it seems that Javascript properties seem to return color values in this format by default.

The live version can be found [here](https://tofritz.com/etch-a-sketch) and the relevent code found in my [`etch-a-sketch`](https://github.com/tofritz/etch-a-sketch) repository.

```javascript
function createCells(dimension) {
  const n = dimension ** 2;
  for (i = 0; i < n; i++) {
    let cell = document.createElement("div");
    cell.setAttribute("class", "cell");
    document.querySelector(".container").appendChild(cell);
  }
}

function styleCells() {
  document.querySelectorAll(".cell").forEach(element => {
    element.style.backgroundColor = "white";
    element.style.height = "100%";
    element.style.width = "100%";
    element.addEventListener("mouseover", shadeCell);
  });
}

function styleContainer(dimension) {
  container = document.querySelector(".container");
  container.style.gridTemplateColumns = `repeat(${dimension}, 1fr)`;
  container.style.gridTemplateRows = `repeat(${dimension}, 1fr)`;
  container.style.width = "980px";
  container.style.height = "980px";
  container.style.margin = "auto";
  container.style.gridGap = "1px";
  container.style.border = "1px solid #000";
  container.style.background = "#000";
}

function createContainer() {
  const container = document.createElement("div");
  container.setAttribute("class", "container");
  container.style.display = "grid";
  document.body.appendChild(container);
}

function shadeCell(event) {
  cellColor = event.target.style.backgroundColor;
  if (cellColor == "white") {
    event.target.style.backgroundColor = getRandomColorRGB();
  } else {
    event.target.style.backgroundColor = darkenRGB(cellColor);
  }
}

function resizeCell(event) {
  dimension = parseInt(
    prompt("Enter a number for the desired squares per side of the sketchpad:"),
    10
  );
  if (isNaN(dimension)) return console.log("Please enter a valid number.");
  styleContainer(dimension);
  createCells(dimension);
  styleCells();
}

// color functions

function getRandomColorRGB() {
  let r, g, b;
  do {
    r = Math.round(Math.random() * 255);
    g = Math.round(Math.random() * 255);
    b = Math.round(Math.random() * 255);
  } while (r == 255 && b == 255 && g == 255);
  return `rgb(${r}, ${g}, ${b})`;
}

function darkenRGB(rgb) {
  let sep = rgb.indexOf(",") > -1 ? "," : " ";
  rgb = rgb
    .substr(4)
    .split(")")[0]
    .split(sep);
  let r = Math.round(rgb[0] * 0.75);
  let g = Math.round(rgb[1] * 0.75);
  let b = Math.round(rgb[2] * 0.75);
  return `rgb(${r}, ${g}, ${b})`;
}

// variables
let dimension = 16;

// add event listeners
document.querySelector("#queryButton").addEventListener("click", resizeCell);

// set up page
createContainer();
```
