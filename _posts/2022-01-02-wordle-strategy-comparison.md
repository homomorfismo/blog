---
layout: post
title: Wordle strategy comparison
---

Everyone is playing [Wordle](https://www.powerlanguage.co.uk/wordle/). Four days ago I read an article in [El País](https://english.elpais.com/science-tech/2022-01-14/a-spanish-data-scientists-strategy-to-win-99-of-the-time-at-wordle.html) about a data scientist, Esteban Moro, who created an algorithm to beat the game 99% of the times. It amounts to choosing a good initial guess and exploit the fact that the game chooses the words by frequency. Today I discussed with some friends the difference between two strategies one can use in the game, which I will call **depth first** and **breadth first search**, as is the usual in optimization.

The **depth first search** algorithm uses in every try the letters that it knows belong to the puzzle word. In each try, the algorithm formulates a new guess that satisfies:

- It contains all the letters that we know are part of the puzzle word.
- It does not contain the ones that we know that are not part of it.
- The letters whose exact location we know are in the correct place.

The **breadth first search** strategy tries to gain information as fast as possible during the first tries, and only at the end of the game (we can choose what round) uses that information to guess the correct word. In each try, the algorithm tries to find a new guess that satisfies:

1. It doesn't contain any letter that we know belongs to the puzzle word, if any, otherwise use the depth first.

2. It doesn't contain either any letter that we know does not belong to the puzzle word, if any, otherwise fall back to 1.

After a certain number of rounds, we use the depth first search to finish.

### Comparison

To compare both, I did as Esteban Moro in [his post](http://estebanmoro.org/post/2022-01-10-wordle/) and simulated a number of games. In his post, he coded everything in R using a `wordle` library, but I preferred to use my own code in Python. I created a class `Game` that stores the information about the current state of the game and contains methods to make the next guess and update (most probably my code can be improved).

```python
class Game:
    def __init__(self, word):
        self.word = word
        self.includes = ''
        self.excludes = ''
        self.exact = ['','','','','']
        self.correct = 0
        self.tries = 0
        self.last = ''
    
    def __str__(self):
        return """word = {}\nlast try = {}\nincludes = {}\nexact = {}\ntries = {}\n""".format(self.word, self.last, self.includes, '-'.join(self.exact), self.tries)

    def update_guess(self, guess):
        self.last = guess
        for k in range(5):
            if guess[k] in self.word:
                if guess[k] not in self.includes:
                    self.includes += guess[k]
                if guess[k] == self.word[k] and guess[k] != self.exact[k]:
                    self.exact[k] = guess[k]
                    self.correct += 1
            else:
                self.excludes += guess[k]
        self.tries += 1

    def breath(self):
        pass1 = list(filter(lambda x: all([l not in x for l in self.includes]), wordle_dict))
        if len(pass1) > 0:
            pass2 = list(filter(lambda x: all([l not in x for l in self.excludes]), pass1))
            if len(pass2) > 0:
                guess = pass2[0]
            else:
                guess = pass1[0]
        else:
            guess = self.depth()
    
    def depth(self):
        pass1 = list(filter(lambda x: all([l in x for l in self.includes]), wordle_dict))
        pass2 = list(filter(lambda x: all([l not in x for l in self.excludes]), pass1))
        pass3 = list(filter(lambda x: all([self.exact[k] == x[k] for k in range(5) if len(self.exact[k]) > 0]), pass2))
        return pass3[0]

    def new_guess(self):
        if self.strategy and self.tries <= 5:
            guess = self.breath()                
        else:
            guess = self.depth()
        return guess

    def play(self, strategy = 'depth', verbose=False):
        self.strategy = 0 if strategy == 'depth' else 1
        self.update_guess('orate')
        if verbose:
            print(self)
        for _ in range(1,6):
            if self.correct < 5:
                self.update_guess(self.new_guess())
                if verbose:
                    print(self)
        return self.correct == 5
```

I exported the list of words to a .txt to use it in python and made a function to run the simulation:

```python
from random import randint

with open('wordle_dict.txt','r') as f:
    wordle_dict = f.read().splitlines()

def simulate(n=1000, strategy='depth'):
    wins = 0
    for _ in range(n):
        word = wordle_dict[randint(1,12972)]
        game = Game(word)
        wins += game.play(strategy=strategy)
    return wins/n
```

### Simulation

In this simulations I used the full list of words and didn't make any consideration when choosing the puzzle word. This led to a lower winning rate compared to the post by Esteban Moro, where he uses a shorter list and imposes conditions on the likelihood of words to be chosen as puzzle word. However, in this post I am not interested in the absolute winning rate, only in which strategy is better for any given word.

As I mentioned above, the number of rounds of breadth search is a parameter $r$ we can choose. I ran the simulation for values of $r = 2,3,4, 5$, each for 1000 games. These are the results

| $r$ value | depth first search | breadth first search |
| --------- | ------------------ | -------------------- |
| 2         | 0.353              | 0.438                |
| 3         | 0.358              | 0.491                |
| 4         | 0.376              | 0.435                |
| 5         | 0.359              | 0.0                  |

Of course the depth first search has no dependence on $r$, and without much rigor we can say that it's winning rate is about 36% of the times. The best parameter value for the breadth first search seems to be $r=3$ rounds.