# Chapter 5 -- The Paddle

## Goals

- Draw a rectangle on the screen as a paddle
- Move the paddle left and right using the arrow keys
- Stop the paddle at the edges of the screen
- Understand how keyboard input works in Arcade

## Concepts Introduced

- Constants
- Booleans (`True` and `False`)
- Boolean flags
- The screen coordinate system
- Event-driven input (key press vs. key release)
- Clamping (keeping a value within limits)

---

## What Are We Building?

In Breakout, the player controls a paddle at the bottom of the screen. The paddle
moves left and right when the player presses the arrow keys.

By the end of this chapter, you will have a white rectangle near the bottom of 
the window that responds to keyboard input.

---

## Programming Concept: Constants

So far you have seen variables that change while the program runs (like `self.x`
in Chapter 4). But some values should stay the same for the entire program. We 
call these **constants**.

In Python, constants are written in `ALL_CAPS` by convention. This is not enforced 
by Python itself -- it is just a signal to anyone reading the code that says: 
*this value is not meant to change.*

At the top of `main.py`, we define four constants that describe the paddle:

```python
PADDLE_WIDTH = 100
PADDLE_HEIGHT = 15
PADDLE_Y = 40
PADDLE_SPEED = 5
```

| Constant        | What it means                               |
|-----------------|---------------------------------------------|
| `PADDLE_WIDTH`  | How wide the paddle is, in pixels           |
| `PADDLE_HEIGHT` | How tall the paddle is, in pixels           |
| `PADDLE_Y`      | How far up from the bottom the paddle sits  |
| `PADDLE_SPEED`  | How many pixels the paddle moves each frame |

Using constants instead of writing the number directly in the code has two
benefits:

1. **The code reads like English.** `PADDLE_SPEED` is easier to understand than 
   a bare `5`.
2. **You only have to change one place.** If you want a wider paddle, change 
   `PADDLE_WIDTH` once rather than hunting for every `100` in the file.

---

## Programming Concept: The Screen Coordinate System

Before drawing anything, you need to understand how positions work on the screen.

Arcade uses a coordinate system where:

- The **bottom-left corner** of the window is position `(0, 0)`
- Moving **right** increases the `x` value
- Moving **up** increases the `y` value

```
y
^
|
|          (640, 480)
|
+-----------> x
(0, 0)
```

This means a point at `(320, 240)` is in the middle of a 640×480 window.

When Arcade draws a filled rectangle using `XYWH`, it takes four values:

- `x` -- the **center** of the rectangle (left-to-right position)
- `y` -- the **center** of the rectangle (bottom-to-top position)
- `width` -- total width of the rectangle
- `height` -- total height of the rectangle

So a rectangle at `XYWH(320, 40, 100, 15)` would be centered horizontally, 
sitting 40 pixels up from the bottom, 100 pixels wide, and 15 pixels tall -- 
exactly where we want our paddle.

---

## State: Tracking Where the Paddle Is

The paddle needs a position that can change as the player moves it. We store 
this as `self.paddle_x` in `__init__`:

```python
def __init__(self):
    super().__init__(SCREEN_WIDTH, SCREEN_HEIGHT, SCREEN_TITLE)
    arcade.set_background_color(arcade.color.BLACK)
    self.paddle_x = SCREEN_WIDTH / 2
```

We start the paddle at `SCREEN_WIDTH / 2`, which is the horizontal center of the
window. The vertical position (`PADDLE_Y`) never changes, so it does not need to
be stored in `self` -- we can just use the constant directly.

---

## Drawing the Paddle

In `on_draw`, we create a rectangle and fill it with white:

```python
def on_draw(self):
    self.clear()
    paddle_rect = arcade.rect.XYWH(self.paddle_x, PADDLE_Y, PADDLE_WIDTH, PADDLE_HEIGHT)
    arcade.draw_rect_filled(paddle_rect, arcade.color.WHITE)
```

Step by step:

1. `self.clear()` -- wipes the screen clean before drawing. Without this, every 
    frame would draw on top of the last one, leaving a smear trail.
2. `arcade.rect.XYWH(...)` -- creates a rectangle object describing where and 
   how big to draw. It does not draw anything yet; it just stores the description.
3. `arcade.draw_rect_filled(paddle_rect, arcade.color.WHITE)` -- actually draws 
   the rectangle on screen, filled with white.

Every frame, `on_draw` runs and redraws the paddle at whatever `self.paddle_x` 
currently is. If `paddle_x` changed since last frame (because the player moved
it), the paddle appears in its new position.

---

## Programming Concept: Booleans

A **boolean** is a value that is either `True` or `False` -- nothing in between. 
The name comes from George Boole, a 19th-century mathematician who developed the 
logic that underpins all computing.

In Python:

```python
is_game_over = False
player_has_won = True
```

Booleans are used to answer yes/no questions in your program. They work naturally 
with `if` statements:

```python
if is_game_over:
    print("Game over!")
```

---

## Programming Concept: Boolean Flags

A **flag** is a boolean variable used to remember whether something is currently 
happening. You set the flag to `True` when the thing starts, and back to `False` 
when it stops.

Think of it like a light switch. When the switch is flipped on, something is 
active. When it is flipped off, it is not.

We use two flags to track which arrow keys are currently held down:

```python
self.left_pressed = False
self.right_pressed = False
```

Both start as `False` because no key is pressed when the game begins.

---

## Programming Concept: Event-Driven Input

You might think that to detect a key press, you would check every frame: *"Is 
the left key down right now?"* That is one approach, but Arcade uses a different 
model called **event-driven input**.

In event-driven programming, the system calls your code **only when something happens** 
-- an event. For keyboard input, Arcade provides two events:

- `on_key_press` -- called once, the moment a key is first pressed down
- `on_key_release` -- called once, the moment a key is released

This is different from checking every frame. Imagine holding the left arrow key 
for two seconds. With event-driven input:

- `on_key_press` fires **once** at the very start
- `on_key_release` fires **once** at the very end
- Nothing fires during the two seconds in between

This is why we use a flag. The flag bridges the gap: we set it `True` on press, 
and use it every frame in `on_update` to keep moving. We clear it back to `False` 
on release to stop.

---

## Handling Key Press and Release

```python
def on_key_press(self, key, modifiers):
    if key == arcade.key.LEFT:
        self.left_pressed = True
    elif key == arcade.key.RIGHT:
        self.right_pressed = True

def on_key_release(self, key, modifiers):
    if key == arcade.key.LEFT:
        self.left_pressed = False
    elif key == arcade.key.RIGHT:
        self.right_pressed = False
```

Both methods receive `key`, which tells you which key was involved. 
`arcade.key.LEFT` and `arcade.key.RIGHT` are constants provided by Arcade that 
represent the left and right arrow keys.

`modifiers` tells you whether keys like Shift or Ctrl were held at the same time. 
We do not use it here, but the method signature requires it.

Notice that `on_key_press` and `on_key_release` do **not** move the paddle 
directly. They only update the flags. The movement itself happens in `on_update`.

---

## Moving the Paddle

`on_update` runs once per frame. It reads the flags and moves the paddle 
accordingly:

```python
def on_update(self, delta_time):
    if self.left_pressed:
        self.paddle_x -= PADDLE_SPEED
    if self.right_pressed:
        self.paddle_x += PADDLE_SPEED
```

- If the left flag is `True`, subtract `PADDLE_SPEED` from `paddle_x` 
  (move left means smaller x).
- If the right flag is `True`, add `PADDLE_SPEED` to `paddle_x` (move right 
  means larger x).

Notice we use two separate `if` statements, not `if/elif`. This means if both 
flags are somehow `True` at once, the paddle would move left and right in the 
same frame -- cancelling out and staying still. That is correct behavior for a 
game that has both arrow keys pressed simultaneously.

---

## Programming Concept: Clamping

What happens if the player holds the right arrow key long enough? `paddle_x` 
keeps increasing until the paddle slides off the right edge of the screen.

To prevent this, we **clamp** the value. Clamping means forcing a number to stay 
a minimum and maximum:

```python
half = PADDLE_WIDTH / 2
if self.paddle_x < half:
    self.paddle_x = half
if self.paddle_x > SCREEN_WIDTH - half:
    self.paddle_x = SCREEN_WIDTH - half
```

Why `half`? Because `paddle_x` is the **center** of the paddle. When the center 
at `half` (50 pixels), the left edge of the paddle is at exactly 0 -- the left 
wall. Moving the center any further left would push part of the paddle off 
screen.

The same logic applies on the right: when the center is at `SCREEN_WIDTH - half` 
(590 pixels for a 640-wide screen), the right edge of the paddle is at exactly 
640 -- the right wall.

```
Left edge of paddle = paddle_x - half
Right edge of paddle = paddle_x + half

Keep left edge >= 0:   paddle_x >= half
Keep right edge <= SCREEN_WIDTH:   paddle_x <= SCREEN_WIDTH - half
```

---

## The Full Picture

Here is how all the pieces work together each frame:

```
Player holds LEFT key
        |
        v
on_key_press sets left_pressed = True
        |
        v (every frame)
on_update sees left_pressed is True
    --> paddle_x decreases by PADDLE_SPEED
    --> paddle_x is clamped to screen bounds
        |
        v (every frame)
on_draw reads paddle_x
    --> draws rectangle at new position
        |
Player releases LEFT key
        |
        v
on_key_release sets left_pressed = False
        |
        v (every frame)
on_update sees left_pressed is False
    --> paddle_x does not change
```

The game runs this cycle roughly 60 times per second, so the paddle appears to 
glide smoothly.

---

## The Complete Code

```python
import arcade

SCREEN_WIDTH = 640
SCREEN_HEIGHT = 480
SCREEN_TITLE = "Breakout"

PADDLE_WIDTH = 100
PADDLE_HEIGHT = 15
PADDLE_Y = 40
PADDLE_SPEED = 5


class BreakoutGame(arcade.Window):
    def __init__(self):
        super().__init__(SCREEN_WIDTH, SCREEN_HEIGHT, SCREEN_TITLE)
        arcade.set_background_color(arcade.color.BLACK)
        self.paddle_x = SCREEN_WIDTH / 2
        self.left_pressed = False
        self.right_pressed = False

    def on_draw(self):
        self.clear()
        paddle_rect = arcade.rect.XYWH(self.paddle_x, PADDLE_Y, PADDLE_WIDTH, PADDLE_HEIGHT)
        arcade.draw_rect_filled(paddle_rect, arcade.color.WHITE)

    def on_update(self, delta_time):
        if self.left_pressed:
            self.paddle_x -= PADDLE_SPEED
        if self.right_pressed:
            self.paddle_x += PADDLE_SPEED

        # Keep paddle within the screen
        half = PADDLE_WIDTH / 2
        if self.paddle_x < half:
            self.paddle_x = half
        if self.paddle_x > SCREEN_WIDTH - half:
            self.paddle_x = SCREEN_WIDTH - half

    def on_key_press(self, key, modifiers):
        if key == arcade.key.LEFT:
            self.left_pressed = True
        elif key == arcade.key.RIGHT:
            self.right_pressed = True

    def on_key_release(self, key, modifiers):
        if key == arcade.key.LEFT:
            self.left_pressed = False
        elif key == arcade.key.RIGHT:
            self.right_pressed = False


def main():
    BreakoutGame()
    arcade.run()


if __name__ == "__main__":
    main()
```

---

## Exercises

- Change `PADDLE_SPEED` to `10`. Does the paddle feel different? Try `2`.
- Change `PADDLE_WIDTH` to `200`. What happens at the screen edges?
- Change `PADDLE_Y` to `200`. Where does the paddle appear?
- Change the paddle color from `arcade.color.WHITE` to `arcade.color.BLUE`.

## Challenges

- Add a second set of keys to control the paddle. For example, `A` and `D` 
  instead of the arrow keys.
- Make the paddle change color while a key is held. (Hint: store the current 
  color in `self`, update it in `on_key_press` and `on_key_release`, and use 
  it in `on_draw`.)
- What happens if you remove the clamping code? Try it and observe.

---

## Reflection Questions

- Why do we use two flags (`left_pressed`, `right_pressed`) instead of one 
  variable that stores a direction?
- Why does `on_key_press` not move the paddle itself?
- What would go wrong if we moved the paddle in `on_key_press` instead of `on_update`?
- Why is `paddle_x` stored as `self.paddle_x` rather than a local variable 
  inside `on_update`?

---

# End of Chapter 5

Next: Adding a ball that moves on its own.
