# Computational Practices 1, Spring 2019

This course repository contains homework assignments, useful guides, and code for **Computational Practices 1** at CCA, Spring 2019.

### Week 1: Monday, January 28, 2019

Inspiration:
- [Interviews with Practitioners](https://www.youtube.com/watch?v=eBV14-3LT-g)
- [Casey Reas](https://www.youtube.com/watch?v=_8DMEHxOLQE)

Lecture ([slides here](intro.pdf)):
- Introductions
- Computer Systems
- Reading Code

Hands-on activities:
- Representing things that aren't numbers...with numbers!

[Homework for Week 1](hw/week1.md)


### Week 2: Monday, February 4, 2018

Homework Review
- What did you learn from Shiffman's videos?

Intro to p5.js
- [Here's the p5.js reference](http://p5js.org/reference).
- `function setup` and `function draw`
- (`x`,`y`) coordinates and the Cartesian plane

#### Workshop: Building Blocks of Code -- functions

You give the computer commands by first writing the name of the command, like `background`, followed by a comma-separated list, in parentheses, containing the parameters for the function: that is, how the function should be run.

Think of the command **"Do some pushups!"** You might ask: **"how many pushups?"** -- that number is a parameter to the command. It tells you something about how to perform the command. Same is true in JavaScript / p5.

Here's the code we wrote together in class to replicated Piet Mondrian's classic *Composition II*:

```javascript
function setup() {
    createCanvas(400, 400);
}

function draw() {
    background(240);
    noStroke();
    strokeCap(SQUARE);

    // blue square
    fill(0, 0, 255);
    rect(0, 320, 80, 80);

    // red square
    fill(255, 0, 0);
    rect(80, 0, 320, 320);

    // yellow square
    fill(255, 255, 0);
    rect(370, 360, 30, 40);

    stroke(0);
    strokeWeight(10);
    line(80, 0, 80, 400);     // left line
    line(0, 320, 400, 320);   // middle line
    line(370, 320, 370, 400); // right

    strokeWeight(15);
    line(370, 360, 400, 360); // bottom

    strokeWeight(20);
    line(0, 160, 80, 160);
}
```

Run it in your editor, or run it on the [p5.js web editor](http://editor.p5js.org). (Either way, make sure to save frequently!)

#### Building Blocks of Code: variables

Here's the modified program we wrote in class together, this time using variables.

(Note for the afternoon class:  In the evening class we ended up using some slightly different names for the variables, but the effect is the same.)

```javascript
function setup() {
  createCanvas(400, 400);
}

function draw() {
  var leftLineX = 80;
  var rightLineX = 370;
  var topLineY = 160;
  var middleLineY = 320;
  var bottomLineY = 360;

  background(240);
  noStroke();
  strokeCap(SQUARE);

  fill(220, 0, 0); // Red
  rect(leftLineX, 0, width-leftLineX, middleLineY); // Large red rectangle

  fill(0, 120, 255); // Blue
  rect(0, middleLineY, leftLineX, height-middleLineY); // Smaller blue rectangle

  fill(255, 255, 0);  // Yellow
  rect(rightLineX, bottomLineY, width-rightLineX, height-bottomLineY); // Yellow rectangle

  stroke(0); // Black
  strokeWeight(10);
  line(0, middleLineY, width, middleLineY); // middle horizontal
  line(leftLineX, 0, leftLineX, height); // left vertical
  line(rightLineX, middleLineY, rightLineX, height); // right vertical

  strokeWeight(15);
  line(rightLineX, bottomLineY, width, bottomLineY); // bottom horizontal

  strokeWeight(20);
  line(0, topLineY, leftLineX, topLineY); // top horizontal
}
```

What's different about it? **Names!**

Our previous code was a sea of numbers. If you wanted to move a point or line intersection around on that drawing, you had to find all the places in the code that referred to that point -- not an easy task! -- and change them in concert.

This **variable** approach here **names** some of the key locations on the canvas and lets us change them everywhere they're used in a drawing command by changing the number itself in **only one place**. p5.js already gives you some variables to work with, including `width` and `height`, used above to refer to the width and height of the canvas.

Naming is a very useful tool in our **abstraction** toolkit.

1. Try changing the canvas size in the code above and observe what happens. Why does this happen? (What happened when you changed the canvas size in the original, non-variable code?

2. Explore some of the other variables the p5.js provides. `mouseX` and `mouseY` are a fun interactive pairing!

#### Animations

One use of variables, explored above, is to name information and use that name to link other components together.

Another use of variables is to track information about an animation as it progresses.

For example, to move a circle across the screen, we might consider code like this:

```javascript
function setup() {
  createCanvas(400, 400);
}

var x = 10;

function draw() {
  background(220);

  ellipse(x, 200, 40);

  x = x + 3;
}
```

In an animation, your computer draws frames at a fixed rate (called, appropriately, the *frame rate*). In p5.js, you are responsible for telling p5 what to draw each frame: you put that drawing code inside the `draw` function. Each frame, p5.js runs your `draw` function and copies whatever it draws to the screen.

To make a smooth animation, we want each frame to look *mostly* like the previous frame, but with a small difference. The code above does that: each frame, the circle is draw 3 pixels further to the right. The code tracks the position of the circle with the `x` variable, whose value persists across frames -- so each frame, `x` is increased by 3.

The code above does what we want, but it's very one-shot: after the circle reaches the right edge of the canvas, it's over.

We can use an `if` statement -- a condition-checking piece of code -- to help us reset the circle's position if it reaches the right edge of the screen, like in the following code:

```javascript
function setup() {
  createCanvas(400, 400);
}

var x = 10;

function draw() {
  background(220);

  ellipse(x, 200, 40);

  x = x + 3;

  if (x > width) {
    x = 10;
  }
}
```

The `if` condition is checked every frame, and when (if?) `x` ever reaches the value of `width` -- 400 in this case, because that's the canvas width -- then `x` is reset to 10.

But that's just a reset. What if we want the circle to move backwards across the screen?

That's a second thing we need to track: the motion of the circle itself. We can make a new variable that tracks how many pixels per frame the circle moves, and then change that variable whenever the circle reaches a canvas boundary.

```javascript
function setup() {
  createCanvas(400, 400);
}

var x = 10;
var changeInX = 3;

function draw() {
  background(220);

  ellipse(x, 200, 40);

  x = x + changeInX;

  if (x > width) {
    changeInX = -3;
  }

  if (x < 0) {
    changeInX = 3;
  }
}
```

Finally, here's the code we wrote in class together that two balls which move independently of one another:

```javascript
function setup() {
  createCanvas(600, 200);
}

var ball = 0;
var ballSpeed = 10;
var ballChangeX = ballSpeed;

var ball2 = 100;
var ballSpeed2 = 5;
var ballChangeX2 = ballSpeed2;

function draw() {
  background(220);
  fill(200, 200, 0);
  ellipse(ball, height/2, 100);
  ellipse(ball2, height/4, 75);
  ball = ball + ballChangeX;
  ball2 = ball2 + ballChangeX2;

  // Check if ball is off right hand side of screen
  if (ball >= width) {
    ballChangeX = -ballSpeed;
  }
  if (ball2 >= width) {
    ballChangeX2 = -ballSpeed2;
  }

  // Check if ball is off left hand side of screen
  if (ball <= 0) {
    ballChangeX = ballSpeed;
  }
  if (ball2 <= 0) {
    ballChangeX2 = ballSpeed2;
  }
}

```


[Homework for Week 2](hw/week2.md)
