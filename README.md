# CSC-1002-snake
CSC 1002 winter/spring 2021 Homework 2, a snake game
# Design Document

## Overview

This program is a snake game. The user uses four arrow keys to maneuver the snake around the game bar. User can also toggle space bar to pause/resume the snake's movement. There are numbers 1-9 randomly scattered on the game bar. The snake's tail will extend length equivalent to the number it ate. There is also a 'monster' in the game trying to reach to the snake's head. The user wins if the snake ate all the numbers before the monster catches the snake's head. Otherwise he/she loses.

## Data Model:

- The snake:

  - The snake's head: A *Turtle* object
    - shape: **square**
    - fill color: **orange**
    - pen color(border color): **black**
    - size: original size, **20px\* 20px **
  - The snake's tail: A series of *stamps* made by the snake's head
    - shape: **square**
    - fill color: **sky blue**
    - pen color(border color): **black**
    - size: original size, **20px\* 20px **
- The monster: A *Turtle* object
  - shape: **square**
  - fill color: **violet**
  - pen color(border color): **black**
  - size: original size, **20px\* 20px **
- The screen: A *Screen* object
  - shape: **square**
  - fill color: **white**
  - size: original size, **660px\* 740px **
- The game bar:  A *Turtle* object
  - shape: **square**
  - fill color: **None ('')**
  - pen color(border color): **black**
  - size: original size, **500px\* 80px **
- The status bar: A *Turtle* object
  - shape: **square**
  - fill color: **None ('')**
  - pen color(border color): **black**
  - size: original size, **500px\* 500px **
- The numbers: each created by a *Turtle* object, using `Turtle.write()` method
  - The Turtle objects are hided
  - The numbers: **center** aligned, **Arial **font with size **10**

## Program Structure

1. Create the screen, status bar, game bar, snake, monster instance
2. Setup the canvas using `set_canvas()`
3. Listen for left mouse button click event
   1. Trigger `initialize()` upon event, start the game
   2. For the snake's loop:
      1. Judge if the game is won or lost. If the game is neither won nor lost:
         1. Detect collision
         2. Create tail
         3. Move forward
         4. Update status bar
         5. Check if the snake eats a number by calling `check_eat()`
         6. Update screen
   3. For the monster's loop:
      1. If the game is neither won nor lost:
         1. Select a direction to chase the snake
         2. Move forward
         3. Detect overlapping by calling `check_contact()`
4. Listen for left/right/up/down arrow key event.  Call `left()`, `right()`,`up()`,`down()` respectively.
5. Listen for space bar key event. Call `toggle_pause()` upon event

## Processing Logic

#### Movement of snake and monster

##### Movement of snake

The snake is  initially placed at the center (0,0).

It remains still until the left mouse button is clicked (game started) and one of left/right/up/down key  is pressed.

Every time a left/right/up/down key is pressed , the direction of snake is modified by `seth()`method and remains unchanged until the next key is pressed. The global variable `pause` will set to be **False**. The global variable `motion`, which has value of either 'up', 'down', 'left' or 'right' is also modified and remains before pressing the next key.

When the snake is at the border and has the heading that causes a collision(e. g., at x=-240 and moving left, or y =240 and moving upwards), `pause` will set to be **True**

When space is pressed, the value of `pause` will be reversed.

The snake only advance if `pause` is **False**. It advances  by 20px in the direction assigned.

The refresh time, when the snake is not extending its tail, is fixed at 300ms

While extending tail, the refresh time is 600ms

##### Movement of monster

The monster is positioned arbitrarily, but far enough from the snake's initial position.

It's initial vertical/ horizontal distance from(0,0) is randomly picked in range $[-200,-80]\cap[80,200]$ 

Each time the monster is attempting to move, it calculates $\Delta x=|x_{monster}-x_{snake\ head}|$ and $\Delta y=|y_{monster}-y_{snake\ head}|$ and pick the larger of the two direction(e. g, $\Delta x = 50 \ and \ \Delta y =20$  then the monster moves horizontally) and advance towards the snake's head in that direction.

The monster forwards in the assigned direction 20pixels at a time.

The refresh time of the motion of monster is arbitrarily chosen from 300~600ms each time.

#### Expanding of the snake's tail

The snake's tail is composed of stamps made by the snake's head (instance name: `snake`) as it advances.

The length of tail is stored by the global variable `tail_length` and initially set to be **5**. This variable is updated every time the snake eats a number.

The tail is constructed by the following procedure:

1. Change the fill color of `snake` to **skyblue**
2. Make a stamp in the current location
3. Record the position of the stamp and store it in global variable `tails_pos`
4. Change back the fill color of `snake` to **orange**

The tail is updated upon each loop of snake. If the snake's current tail length (achieved by `len(tails_pos)`equals to `tail_length`, then the end of tail must be deleted. This process happens by:

1. Use `clearstamps(1)` to delete the first stamp (dequeue) among all current stamps
2. pop the first stamp's location from `tails_pos`

#### Detecting the contact between monster and snake:

The total contact number is stored in global variable `contact`.

During each loop of monster, the `check_contact()` function will be called.

The function traverses the `tails_pos` and use `distance()` function to calculate the distance between monster and every piece of tail. If the distance is less than 20, it means there is an overlap.  `check_contact()` function will then add `contact` by 1.

## Function Specifications

- `set_canvas()`
  - Description: Arrange elements displayed on the canvas. Including:
    - Resizing the screen
    - Display the game bar, status bar and the status
    - Position the snake at the origin
    - Position the monster randomly but far enough from the snake
  - Parameters:
    - None
  - Returns:
    - None
- `initialize()`
  - Description: Place the numbers on the screen and start the game.
    - The numbers are created by 9 turtles. Each turtle is hidden.  By calling `Turtle.write()`, the numbers are created. The position of turtle is offset in y direction downwards by 8 units to make sure snake can have head-on collision with the numbers.
  - Parameters:
    - None
  - Returns:
    - None
- `left()`
  - Description: Make the snake move left.
  - Parameters:
    - None
  - Returns:
    - None
- `right()`
  - Description: Make the snake move right.
  - Parameters:
    - None
  - Returns:
    - None
- `up()`
  - Description: Make the snake move upwards.
  - Parameters:
    - None
  - Returns:
    - None
- `down()`
  - Description: Make the snake move downwards.
  - Parameters:
    - None
  - Returns:
    - None
- `toggle_pause()`
  - Description: Pause/ resume the snake's motion
  - Parameters:
    - None
  - Returns:
    - None
- `move()`
  - Description: Main loop for the movement of snake. Used for:
    - Move the snake to the assigned direction
    - Pause the snake when it hits the boundary
    - Extend the snake's tail when the snake consumed numbers
  - Parameters:
    - None
  - Returns:
    - None
- `chase()`
  - Description: Main loop for the movement of monster. Used for:
    - Move the monster towards the snake's head
    - Check if there are any overlap between the monster and the snake's tail by calling `check_contact()` function
  - Parameters:
    - None
  - Returns:
    - None
- `judge_win()`
  - Description: Judge if the game should be terminated
    - If the monster overlaps the snake's head, the game is lost
    - If the snake eats all the numbers, the game is won
  - Parameters:
    - None
  - Returns:
    - None
- `check_eat()`
  - Description: Check if the monster eats a number.
  - Parameters:
    - None
  - Returns:
    - None
- `check_contact()`
  - Description: Check if there are any overlap between the snake and monster
  - Parameters:
    - None
  - Returns:
    - None

## Output: Skipped



