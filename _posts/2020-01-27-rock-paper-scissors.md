---
layout: post
title:  "javascript: rock, paper, scissors"
author: tyler
date:   2020-01-27 18:00:00 -0600
categories: javascript odin
---

The next deliverable in the `Web Dev 101` course of my Odin Project curriculum is a simple implementation of the game 'rock, paper, scissors' in Javascript. Due to my previous experience with Python, I did not run into any problems conceptually with this exercise. However, it was a useful method for learning the differences in syntax and code formatting that comes with Javascript. Additionally, this led to more exposure to the console and built-in debugging utlities found in Chrome Dev Tools.

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <title>Document</title>
  </head>
  <body>
    <script>
      function computerPlay() {
        const choices = ["rock", "paper", "scissors"];
        return choices[Math.floor(Math.random() * 3)];
      }

      function playRound(playerSelection, computerSelection) {
        outcome = new String();
        switch (computerSelection) {
          case "rock":
            outcome =
              playerSelection.toLowerCase() == "paper"
                ? "You win! Paper covers rock."
                : playerSelection.toLowerCase() == "scissors"
                ? "You lose! Rock beats scissors."
                : playerSelection.toLowerCase() == "rock"
                ? "Tie! Both played rock."
                : "Please enter only rock, paper, or scissors.";
            break;
          case "paper":
            outcome =
              playerSelection.toLowerCase() == "rock"
                ? "You lose! Paper covers rock."
                : playerSelection.toLowerCase() == "scissors"
                ? "You win! Scissors cuts paper."
                : playerSelection.toLowerCase() == "paper"
                ? "Tie! Both played rock."
                : "Please enter only rock, paper, or scissors.";
            break;
          case "scissors":
            outcome =
              playerSelection.toLowerCase() == "rock"
                ? "You win! Rock beats scissors."
                : playerSelection.toLowerCase() == "paper"
                ? "You lose! Scissors cuts paper."
                : playerSelection.toLowerCase() == "scissors"
                ? "Tie! Both played rock."
                : "Please enter only rock, paper, or scissors.";
            break;
        }
        if (outcome.includes("win")) {
          wins++;
        } else if (outcome.includes("lose")) {
          losses++;
        }
        return outcome;
      }

      function game() {
        while (true) {
          let playerSelection = prompt("Choose rock, paper, or scissors:");
          let computerSelection = computerPlay();
          console.log(playRound(playerSelection, computerSelection));
          if (wins == 3 || losses == 3) break;
        }
        let message = wins == 3 ? "You won the match!" : "You lost the match!";
        return message;
      }
      let wins = 0,
        losses = 0;
      console.log(game());
    </script>
  </body>
</html>
```