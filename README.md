# Game On

## Project Setup and Canvas

- Create HTML5 Canvas
- Give it an id `gameScreen`
- With `width=800` and `height=600` attribute in it
- In head tag add a style tag
- Inside the style tag add `border` for the `gameScreen` Canvas

## Drawing the Canvas

- Grab the canvas inside the `index.js` file with `getElementById`
- Create context(`ctx`) for the current canvas element, `canvas.getContext('2d')`
- Draw a red rectangle inside the canvas using the `ctx.fillStyle and ctx.createRect(x,y,w,h)`

## Clearing the Canvas

Whenever you try to develop game you need to clear the canvas area becuase every time you redraw onto the canvas what was previously on the canvas is still there so when you're working with a game where objects are moving around on the screen you want to make make sure you clear what was previously there before.

- So What you can do is you can call `clearRect` method from the `ctx`(context) and add four parameters(x,y,canvas.width,canvas.height) inside that method.

## Creating the Player's Paddle

Now we need to create a paddle first. The paddle should be at the bottom of the canvas and player can move it to left and right in order to catch the ball and bounce it back up towards the bricks.

- Delete previous rectangle you created for the test.
- Now inside the current `index.js` file create two constant variables `GAME_WIDTH` and `GAME_HEIGHT` and give them the value of canvas width and height
- Create a class to handle all the aspects of a paddle.
- Inside the `src` folder create a new file `paddle.js`
- Now create a class inside that file
- add a `constructor` method as well as another method called `draw`
- pass `ctx` argument inside the `draw` method
- Now pass two parameters inside the constructor method `gameWidth` and `gameHeight`
- Inside the `constructor` add some of the required properties
- like `this.width`, `this.height` with some random values e.g. 150 and 20 respectively
- add another object property inside the constructor `this.position` and inside that object add x and y keys
- for `y` key we can use `gameHeight - this.height - 10`
- for `x` key we can use `gameWidth/2 - this.width/2`
- Now inside the `draw` method use `ctx.fillRect(this.position.x, this.position.y, this.width, this.height)`
- Now export this class by adding `export default` keyword before the class keyword
- Now inside the `index.js` file import the `Paddle` class and the create a copy of it to `paddle` variable. Don't forget to pass two required parameters `GAME_WIDTH` and `GAME_HEIGHT`. Then call the draw method by passing `ctx` object inside it.

## The Game Loop

Now this time we are going to move our paddle on the screen. So, in order to do that we need to establish a game loop. A game loop is something that runs every frames, update all the objects, redraws them in the new position and then moves on to the next frame.

- Create a new function inside the `index js` file called `gameLoop`
- Now move the `ctx.clearRect...` line we already created earlier inside the `gameLoop` function
- Then call the `update` method of the `paddle` object which we didn't created yet. So, switch to `paddle js` file and add update method inside the class.
- Now let's just for now increment 5 for the `position.x`. So it will add five pixels per second. So, the update method needs to bring in a delta time or you might see it as `dt` and a lot of other games what is called delta time that means the change in time how much time has changed since the last time this has been updated. So, we're going to update this, like five divided by the delta time. It means it is going to go 5 pixels divided by how much time is in past.
- So, go back to `index js` file. Inside the `gameLoop` function we need to know what was the last time. So, above the `gameLoop` function create a new variable `lastTime` and initialize it with `zero`.
- Now this loop needs to bring in a `timestamp` and we need to do `paddle.draw` and pass in the `ctx`. We need to calculate how much time has passed. So we will do here, `let detlaTime = timestamp - lastTime` then after that `lastTime = timestamp`. So where does the timestamp come from? We are going to request an animation frame from the browser and give it the game loop. So, every time this runs it will say "hey when the next frame is ready call this game loop again" and pass the `timestamp` into it and we will calculate the detlta time, so pass it into our `paddle.update` method and continue.
- Now call the `gameLoop` function under the `gameLoop` declaration.
- Now the reason for that is not working because on our very first frame we're not passing a timestamp at all which is fine because we're starting at zero but if you switch back to `paddle js` file you can see the paddle is expecting to divide by the delta time here which is going to be zero. So, we can put in here if not delta time then return. Now you can see our paddle is moving across the screen.
- So, let's just remove this line `this.position.x += 5 / detlaTime`. And we will handle actually moving the paddle with inputs later.

## Handling Keyboard Input

Let's create a simple input handler to listen to a couple key events for moving the arrows left and right. These will be used to move the paddle left and right on the screen.

- Now crate a new file and give the file name `input.js`. Then inside that file we are going to create a new class called `InputHandler` and export this by adding `export default` keyword before `class` keyword.
- Give it a `constructor`
- Now inside the `index js` file we can simple import that class
- So, what should this thing do? Let's just add an event listener inside the `constructor` method for listening to a `keydown` event on the arrow keys. And inside the anonymous function we can simply show event keyCode using `alert`. Or you can use `console.log` to know what is the event key code.
- Now inside the `index js` file we need to instantiate it before the game loop function. So if we push the left arrow key we get 37 and if we press right arrow key we get 39.
- So, now we just need to make a little switch statement here. So `switch(event.keyCode)` and first case would be 37 then we are showing `move left` in alert function. And `move right` if the case is 39. So, we can get rid of the previous `alert` function. Now let's check, perss left arrow key `move left` and then right arrow key `move right`.
- Next we need to tell the paddle how to handle these movements.

## Moving the paddle

Next Let's actually move the paddle with a key down here.

- So remove all `alert` functions. Then inside the 37 case we can write `paddle.moveLeft()`. Now we don't have access to the `paddle` object, so we can use `paddle` argument inside our `constructor` method. And inside our `index js` file let's pass our `paddle` object inside the inputHandler.
- Now we need to create our `moveLeft` method inside the `paddle js` file, so go back to it, and create `moveLeft` inside the class. Now we need to think what is going to happen when this method is going to be called. So, let's create `maxSpeed` property inside the `paddle` constructor and initialize the value with 10. And add another `speed` property into it and give the value of zero. So, inside the `moveLeft` method we can assign the speed value by negative maxSpeed.
- Now inside the update method we can increment the x key value of position by the `speed` property of paddle. Now when we tell it to move left the speed becomes negative ten and then when we update the x position, it's value gets moved a minus ten amount. Now let's try it out. You can see the speed is fast, so, we need to lower the value of maxSpeed from ten to 7. So, let's change it. Now we have another issue whenever we press the left button, once the paddle gets to the edge of the screen it keeps going. We need to make sure that it cannot leave the bounds of our game. So the solutions would be if the position of x is less then zero we need to assign it's value to zero inside the update method. Now let's check it out again. Press the left arrow key, there we go. Now it stops when it hit the edge of the screen.
- Now all we have to do is do our move right and we will be done. So, go back to the `input js` file, inside the swtich statement, where we have case 39 we can write: `paddle.moveRight();`. Now go back to `paddle js` file and make a move right method under the `moveLeft` method. And inside the method we can assign the value of `speed` property to `maxSpeed` value. So, this sets its to the positive seven. But again just like with the left bounds we need to make sure that it can't go off the right side of the screen. So inside the update method we can use another conditional statement. If x position plus it's width of the paddle is greater than the game width then assign the value of x position to game width minus paddle width. At the moment we don't have access to game width. So inside the constrcutor we need to add a new line before the initialization of paddle width. So, let's press the right arrow key, now the paddle cannot go off the right edge of the screen.

## Stopping the paddle

Alright now let's work on stopping the paddle when the player releases a key.

- So, copy whole event handler code we created earlier and paste them right under it. But instead of using `keydown` we are going to use `keyup` event. And change those `moveLeft` and `moveRight` methods to `stop`. So, if we release one of these keys we are going to stop the paddle. So, let's just create a quick function inside our `paddle js` file under the `moveRight` method.

- Then add one line of code which sets the value of paddle speed to zero. Now let's try it out in the browser. It works great but here's the problem if we are pushing left and then push right and if we release the the left key it kind of stops for a second so what we need to do is we need to make sure that it's moving in the same direction that we're releasing. So, inside the `keyup` event handler, for the case of 37 we need to check if paddle speed is less than zero then we are going to stop the paddle. And for the case of 39 we need to check if paddle speed is greater than zero then stop the paddle. Now let's try it out.

## Drawing the ball image

Next we need to learn how we can draw an image onto our canvas.

- First we need an image, so, create a directory called `assets` and inside it create another folder called `images`. Copy `ball.png` file there.
- Go to `index html` file and after the `body` tag add an `img` tag and give the src as `assets/images/ball.png`, give it an id attribute with the value of `img_ball`. Now Inside the style tag or if you have an external stylsheet use `#img_ball{display:none}` style declaration.
- So, let's go into our `index js` file and declare `imgBall` variable with `document.getElementById('img_ball')`. So we have the image, now inside the `gameLoop` function whenever we draw we can call `ctx.drawImage(imgBall, 16, 16, 16, 16);`. Now Let's just simply refactor this into our own class. So, create a new file inside the `src` folder called `ball.js`. Now write `export default class Ball`. We need a constructor and we are also going to need a draw method and later we will need a update method. So, inside the constructor we will need that image, so we should assign an image property by using `this.image = document.getElementById('img_ball')`. Now go back to `index js` and remove previous line where we assigned an `imgBall` variable. Now grab the line where we wrote `ctx.drawImage`, and move it into draw method of ball class. We need to pass `ctx` object inside the draw method. And change the `imgBall` to `this.image`. So, we create the ball class it will grab the image from the page and every time we want to draw it will draw it here. Inside the `index js` file we import the ball class. And after the `paddle` variable declration we can simply instantiate the ball class by writing `let ball = new Ball()`. Then in our game loop function we need to draw the ball `ball.draw(ctx)`. Now we have a new clean class that we use to change the position of the ball as it moves around in our game world before we start moving the ball around on the screen.

## Moving the ball

Before we start moving the ball around the screen let's just clean up a couple of issues real quick in our `index js` file. Instead of just calling the game loop, we can use the `window.requestAnimationFrame` and that will give us a valid timestamp which means we can go to our `paddle js` file and in our update method we can get rid of the if condition.

```js
// Remove this line
if (!deltaTime) return;
```

- Let's go to our ball class, our update method is going to have delta time, so that we know how far to move. Now inside the constructor we can have a `speed` object property and inside the speed object we can have x and y property. And we can hard code the value of x to 4 and y to 2. And another position property with x of 10 and y of 10. Now inside the update method we can say, `this.position.x += this.speed.x` as well as `this.position.y += this.speed.y`. Now inside the draw method we can change the value of hard coded 10 values to `this.position.x` and `this.position.y`.
- Now go back to `index js` file and inside the `gameLoop` function above the `ball.draw` line we can write `ball.update(deltaTime)`. Now see on the browser we have our ball moving.
- Now back to `ball js` file add another property called size into our constructor method and give it's value to 16. Then inside the draw method change those hard coded values 16 to `this.size`.

Now we need to worry about bouncing off the balls. Just like the paddle we need to pass `gameWidth` and `gameHeight` arguments inside our ball constructor. So, back in our `index js` file When we instantiate our ball, we need to give it `GAME_WIDTH` as well as `GAME_HEIGHT`. Now go back to `ball js` file and inside the constructor we can simply write `this.gameWidth = gameWidth` and `this.gameHeight = gameHeight`. So, now the ball knows how big the game world is.

- Let's talk about the collisions here. Inside our update method we need to say, if x position is grater than the game width or x position is less than the zero we need to reverse the speed on the x axis. Again we need another check if y position is greater than the game height or y position is less than the zero we need to reverse the speed on the y axis.

```js
if (this.position.x > this.gameWidth || this.position.x < 0) {
  this.speed.x = -this.speed.x;
}

if (this.position.y > this.gameHeight || this.position.y < 0) {
  this.speed.y = -this.speed.y;
}
```

Now let's see if the ball bounces off the bottom wall. This is working nice, but in y position we have an issue. Let's fix it. When we are checking `this.position.y > this.gameHeight` add the ball size after the `this.position.y`. Now same thing on the right edge. Let's fix it also, add the ball size after the `this.position.x`

```js
if (this.position.x + this.size > this.gameWidth || this.position.x < 0) {
  this.speed.x = -this.speed.x;
}

if (this.position.y + this.size > this.gameHeight || this.position.y < 0) {
  this.speed.y = -this.speed.y;
}
```

## Refactoring to the game class

Let's do a little bit of refactoring, right now we are passing two parameters to both the ball and the paddle. We are passing game width and game height, if we're going to pass more parameters like the ball might might need to know where the paddle is.

Now let's create a new file called `game.js` and as usual create a game class with export default keyword. So, this class is going to be in charge of managing all this for us. So, let's go inside the `index js`, now grab three lines of code (ball, paddle and inputHandler) and move them into our new `game` class.

```js
// Move this lines
let paddle = new Paddle(GAME_WIDTH, GAME_HEIGHT);
let ball = new Ball(GAME_WIDTH, GAME_HEIGHT);

new InputHanlder(paddle);
```

So, create a constructor method, pass two arguments `gameWidth` and `gameHeight`. And Inside the method we can simply paste our code. So, right above the `let paddle...` create a new line and add `this.gameWidth = gameWidth` as well as `this.gameHeight = gameHeight`. So, inside the `new Paddle` and `new Ball` we can remove those parameters we wrote earlier and write `this` parameter inside the parenthesis. Let's move this lines of code inside our new method called `start`.

```js
start() {
    let paddle = new Paddle(this);
    let ball = new Ball(this);

    new InputHanlder(paddle);
}
```

Now let's go ahead and grab those imported classes and move them inside our game js file above the class.

```js
// move these codes to `game js` file above the class
import Paddle from './paddle';
import Ball from './ball';
import InputHanlder from './input';
```

Now go back to our paddle and ball class and do some necessary changes. So, open the `ball js` file frist, then instead of taking gameWidth and gameHeight we're just going to take the game object. Then change `gameWidth` to `game.gameWidth` as well as `gameHeight` to `game.gameHeight`.
