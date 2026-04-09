# Cross-Platform Python Graphics Setup Guide

*(macOS + Windows)*

This guide walks through setting up Python projects for drawing on a
screen using Tkinter, Turtle, Pygame, or Arcade. It is designed for
collaboration between macOS and Windows users.

------------------------------------------------------------------------

# 1. Common Project Setup (Recommended for All Options)

## Step 1 --- Create a Project Folder

### macOS (Terminal)

``` bash
mkdir breakout-playground
cd breakout-playground
```

### Windows (PowerShell)

``` powershell
mkdir breakout-playground
cd breakout-playground
```

------------------------------------------------------------------------

## Step 2 --- Create and Activate a Virtual Environment

### macOS

``` bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
```

### Windows (PowerShell)

``` powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
```

If PowerShell blocks activation, run once:

``` powershell
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```

------------------------------------------------------------------------

## Step 3 --- Verify Python Works

``` bash
python --version
python -c "print('hello')"
```

------------------------------------------------------------------------

# 2. Option A --- Tkinter (Built-In)

### Installation

No installation required. Tkinter ships with most Python distributions.

### Test Program (`tk_test.py`)

``` python
import tkinter as tk

root = tk.Tk()
root.title("Tkinter Test")

canvas = tk.Canvas(root, width=400, height=300, bg="white")
canvas.pack()

canvas.create_rectangle(50, 50, 150, 120, fill="skyblue", outline="black")
canvas.create_text(200, 20, text="Hello Tkinter", fill="black")

root.mainloop()
```

Run:

``` bash
python tk_test.py
```

------------------------------------------------------------------------

# 3. Option B --- Turtle (Built-In)

### Installation

No installation required.

### Test Program (`turtle_test.py`)

``` python
import turtle

t = turtle.Turtle()
t.speed(0)

for _ in range(4):
    t.forward(100)
    t.right(90)

turtle.done()
```

Run:

``` bash
python turtle_test.py
```

------------------------------------------------------------------------

# 4. Option C --- Pygame (pygame-ce Recommended)

### Install

``` bash
python -m pip install pygame-ce
```

### Test Program (`pygame_test.py`)

``` python
import pygame

pygame.init()
screen = pygame.display.set_mode((640, 480))
clock = pygame.time.Clock()

running = True
x = 50

while running:
    for event in pygame.event.get():
        if event.type == pygame.QUIT:
            running = False

    x = (x + 2) % 640

    screen.fill((30, 30, 30))
    pygame.draw.rect(screen, (200, 200, 255), pygame.Rect(x, 200, 80, 20))
    pygame.display.flip()
    clock.tick(60)

pygame.quit()
```

Run:

``` bash
python pygame_test.py
```

------------------------------------------------------------------------

# 5. Option D --- Arcade

### Install

``` bash
python -m pip install arcade
```

### Test Program (`arcade_test.py`)

``` python
import arcade

WIDTH, HEIGHT = 640, 480

class MyGame(arcade.Window):
    def __init__(self):
        super().__init__(WIDTH, HEIGHT, "Arcade Test")
        self.x = 50

    def on_draw(self):
        self.clear()
        arcade.draw_rectangle_filled(self.x, HEIGHT // 2, 80, 20, arcade.color.AERO_BLUE)

    def on_update(self, delta_time: float):
        self.x = (self.x + 120 * delta_time) % WIDTH

game = MyGame()
arcade.run()
```

Run:

``` bash
python arcade_test.py
```

------------------------------------------------------------------------

# 6. Keeping Both Machines in Sync

If using Pygame or Arcade:

## Create requirements.txt

``` bash
python -m pip freeze > requirements.txt
```

Your collaborator installs with:

``` bash
python -m pip install -r requirements.txt
```

------------------------------------------------------------------------

# Suggested Learning Path

-   **Start**: Tkinter (no install friction)
-   **Then**: Arcade (clean game structure)
-   **Alternative**: Pygame (classic approach with many tutorials)

This progression allows gradual introduction to: - Variables and
functions - Event handling - Game loops - Collision detection - State
management

------------------------------------------------------------------------

End of Guide
