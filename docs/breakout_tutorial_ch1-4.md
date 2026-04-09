# Breakout Tutorial (Arcade) -- Chapters 1--4 Plan

## Philosophy

This tutorial is intentionally incomplete.

You are expected to: - Read the code - Modify it - Break it - Fix it -
Extend it

If something feels "missing," that is by design.

------------------------------------------------------------------------

# Chapter 1 -- What We're Building

## Goals

-   Understand what Breakout is
-   Understand what you will build step by step
-   Understand the idea of a *game loop*

## Concepts Introduced

-   Game loop (draw → update → input)
-   Iterative development

## Discussion

Breakout consists of: - A paddle (player-controlled) - A ball (moves
automatically) - Bricks (destroyed on collision)

The game runs continuously: 1. Draw everything 2. Update positions 3.
Handle input

## Exercise

-   Write down (in comments or notes):
    -   What happens every frame?
    -   What objects exist in the game?

## Challenge

-   Think about: what happens if the game runs at different speeds?

------------------------------------------------------------------------

# Chapter 2 -- Installation

## Goals

-   Install Python
-   Install Arcade
-   Run a simple script

## Core Steps

-   Install Python 3.9+

-   Install Arcade:

        pip install arcade

## Verification

Create a file:

    test_arcade.py

Add:

``` python
import arcade

print("Arcade imported successfully")
```

Run it.

## Exercise

-   What happens if you uninstall arcade and try again?
-   Where is arcade installed on your system?

------------------------------------------------------------------------

## Appendix (Platform Notes)

### Windows

-   Use python.org installer
-   Ensure "Add to PATH" is checked

### Linux

-   Use system Python or pyenv
-   May need additional OpenGL libraries

### macOS

-   Use python.org or Homebrew
-   Ensure Python version is correct (`python3 --version`)

------------------------------------------------------------------------

# Chapter 3 -- Hello World Window

## Goals

-   Create a window
-   Draw text
-   Understand `on_draw`

## Starter Code

``` python
import arcade

WIDTH = 800
HEIGHT = 600
TITLE = "Breakout"

class Game(arcade.Window):
    def __init__(self):
        super().__init__(WIDTH, HEIGHT, TITLE)

    def on_draw(self):
        arcade.start_render()
        arcade.draw_text("Hello, World", 100, 300, arcade.color.WHITE, 20)

game = Game()
arcade.run()
```

## Concepts

-   Subclassing (`arcade.Window`)
-   Overriding methods
-   Drawing each frame

## Exercise

-   Move the text
-   Change the color
-   Draw multiple pieces of text

## Challenge

-   Can you center the text?
-   Can you make the text move?

(Hint: You will need variables.)

------------------------------------------------------------------------

# Chapter 4 -- How an Arcade Game Runs

## Goals

-   Understand `on_update`
-   Introduce movement
-   Introduce state

## Starter Code (Modify Chapter 3)

Add:

``` python
class Game(arcade.Window):
    def __init__(self):
        super().__init__(WIDTH, HEIGHT, TITLE)
        self.x = 100

    def on_draw(self):
        arcade.start_render()
        arcade.draw_text("Moving Text", self.x, 300, arcade.color.WHITE, 20)

    def on_update(self, delta_time):
        self.x += 1
```

## Concepts

-   State (variables stored in the class)
-   Time step (`delta_time`)
-   Continuous updates

## Exercise

-   Change the speed
-   Make the text bounce off screen edges

## Challenge

-   Use `delta_time` instead of a fixed increment
-   What happens if you remove `on_update`?

------------------------------------------------------------------------

## Reflection Questions

-   What is the difference between `on_draw` and `on_update`?
-   Why is state stored in the class?

------------------------------------------------------------------------

# End of Chapter 4

Next: Paddle and player input.
