# Flappy bird/gosh clone :hatched_chick:

<img width="253" alt="animation" src="https://user-images.githubusercontent.com/76016357/219280710-0c134cd6-2607-4cfd-a187-62f10e71c9f8.gif">

--------------
*This project was proposed on the freeCodeCamp website in a video tutorial created by the YouTuber and developer Ania Kubów.*
- *Go to [freeCodeCamp](https://www.freecodecamp.org/espanol/news/40-proyectos-de-javascript-para-principiantes-ideas-faciles-para-empezar-a-codificar-en-js/#c-mo-crear-siete-juegos-cl-sicos-con-ania-kubow) article.*
- *Go to Ania Kubów's [YouTube](https://www.youtube.com/channel/UC5DNytAJ6_FISueUfzZCVsw) channel, and more specfically, the [tutorial](https://www.youtube.com/watch?v=8xPsg6yv7TU&ab_channel=freeCodeCamp.org) that I will be referring*
---------------------
Hi! Welcome to this project :hugs:. This is a clone of the famous game Flappy Bird created using Vanilla JavaScript, the DOM, and a few interesting methods that helped us create the motion effect:
* createElement()
* setInterval()
* clearInterval()
* addEventListener() ✨
* removeEventListener()

I highly recommend this project to those who are learning more about DOM manipulation. It's a very fun and educational project.
In this article, I will briefly explain the logic behind this mini-project  

#### 1. Folder structure
We don't have anything too complicated here, as it's a Vanilla JavaScript project. We only have an index.html file, an app.js file, and a stylesheet for our styles. You can check the folder structure above.

#### 2. index.html
The index.html file is even more simple than the structure.
```html
    <div class="game-container">
      <div class="sky">
        <div class="bird"></div>
      </div>
      <div class="ground"></div>
    </div>
```
As you can see, we have a structure based on div tags. The 'game-container' tag is our main container, inside of which we have the 'sky' container where we'll put the bird and the top obstacles, and the 'ground' container where part of the bottom obstacles will be contained.

Using basic styles, we are going to achieve the following result.

<img src="https://user-images.githubusercontent.com/76016357/218874647-580e0322-fa9e-4896-aeab-6bccdd19a2ec.png"  width="400"> --> <img width="249" alt="Screenshot 2023-02-14 215805" src="https://user-images.githubusercontent.com/76016357/218915867-c93e1618-e9b5-448d-aa8e-2b8f7625e205.png">

If you are wondering where the top obstacle come from, don't worry, I'll explain that forward, for now just ignored it

#### 3. Javascript code
Moving into JavaScript code, I'll divide the code into little pieces to explain the behavior of each one. Let's keep in mind the following variable declarations.
```js
//First, we use the querySelector method from the document object to select both divs 
//(gameDisplay and ground) and the bird div using their class names
  const bird = document.querySelector(".bird");
  const gameDisplay = document.querySelector(".game-container");
  const ground = document.querySelector(".ground");
  //Keep in mind this variables
  let birdLeft = 220;
  let birdRight = 280;
  let obstacleWidth = 60;
  let groundHeight = 150;
  let birdBottom = 100;
  let gravity = 2;
  let isGameOver = false;
  let gap = 430;
  let birdHeight = 45;
  let gameDisplayHeight = 500
```
Learn more about [*querySelector*](https://developer.mozilla.org/en-US/docs/Web/API/Document/querySelector)

#### 3.1. start() function
The **'start'** function sets up the initial position of the bird. We define both the x and y positions using the HTML DOM style object's left and bottom properties and the variables **'birdBottom'** (set to 100) and **'birdLeft'** (set to 220) that were defined earlier
```js
 function starGame() {
    birdBottom -= gravity;
    bird.style.bottom = `${birdBottom}px`;
    bird.style.left = `${birdLeft}px`;
  }
  let gameTimerId = setInterval(starGame, 20);
```
Learn more about [*HTML DOM style object*](https://www.w3schools.com/jsref/dom_obj_style.asp)

Learn more about [*setInterval()*](https://developer.mozilla.org/en-US/docs/Web/API/setInterval) method

#### 3.2. control() and jump() function
The **'control'** function is very simple. We are going to capture the event produced by the space key. You can find information about each keycode [here](https://www.toptal.com/developers/keycode). The keycode 32 corresponds to the space key, so every time we press the space key, the jump function will be called.
```js
  function control(e) {
    if (e.keyCode === 32) {
      jump();
    }
  }
```

```js
  function jump() {
    if (birdBottom < 500) {
      birdBottom += 50;
      bird.style.bottom = `${birdBottom}px`;
    }
  }
  document.addEventListener("keyup", control);
```
The `jump()` function first checks whether the bird is outside of the game container (which is set at 500 pixels). If not, the distance between the **'bird'** and the **'bottom'** of the container will increase by **50 pixels** every time the space key is pressed. This behavior is controlled using the method `addEventListener()`, which constantly listens for the keyup event. Every time the keyup is pressed, the method calls the function `control()`, and control, in turn, calls `jump()` .

Learn more about [addEventListener](https://developer.mozilla.org/es/docs/Web/API/EventTarget/addEventListener) method
Learn more about [keyup](https://www.w3schools.com/jsref/event_onkeyup.asp) event
```
#### 3.3. gameOver() function
Before pass trought the largest function in the program, let's talk about the `gameOver()` function. 
```js
  function gameOver() {
    clearInterval(gameTimerId);
    isGameOver = true;
    document.removeEventListener("keyup", control);
  }
```
Do you remember when we talked about the `setInterval()` function? Well, if you recall, I mentioned that the function returns an ID, and that's where we'll use it. When the `gameOver()` function is called, the game should stop. We'll use the `clearInterval()` function, which is the opposite of the `setInterval()` function, to stop the motion of the bird.
The `removeEventListener()` method is the opposite of the `addEventListener()` method, so you can imagine what it does.

Regarding the `isGameOver` variable, we'll discuss it in a moment.

Learn more about [clearInterval()](https://developer.mozilla.org/en-US/docs/Web/API/clearInterval) method
Learn more about [removeEventListener()](https://developer.mozilla.org/es/docs/Web/API/EventTarget/removeEventListener) method

#### 3.4. generateObstacle() function
This is the largest part of the code, and we'll analyze every part.
```js
  function generateObstacle() {
    let obstacleLeft = gameDisplayWidth;
    let randomHeight = Math.random() * 60;
    let obstacleBottom = randomHeight;
    const obstacle = document.createElement("div");
    const topObstacle = document.createElement("div");
    if (!isGameOver) {
      obstacle.classList.add("obstacle");
      topObstacle.classList.add("topObstacle");
    }
    gameDisplay.appendChild(obstacle);
    gameDisplay.appendChild(topObstacle);
    obstacle.style.left = `${obstacleLeft}px`;![Animation](https://user-images.githubusercontent.com/76016357/219280329-ae84d5b5-d5c4-4775-8e26-8fe3cbe4025c.gif)

    topObstacle.style.left = `${obstacleLeft}px`;
    obstacle.style.bottom = `${obstacleBottom}px`;
    topObstacle.style.bottom = `${obstacleBottom + gap}px`;

    function moveObstacle() {
      obstacleLeft -= 2;
      obstacle.style.left = `${obstacleLeft}px`;
      topObstacle.style.left = `${obstacleLeft}px`;

      if (obstacleLeft === -60) {
        clearInterval(timerId);
        gameDisplay.removeChild(obstacle);
        gameDisplay.removeChild(topObstacle);
      }
      if (
        (obstacleLeft > birdLeft - obstacleWidth &&
          obstacleLeft < birdRight &&
          birdLeft === 220 &&
          (birdBottom < obstacleBottom + groundHeight ||
            birdBottom > obstacleBottom + gap - (groundHeight + birdHeight))) ||
        birdBottom === 0
      ) {
        gameOver();
        clearInterval(timerId);
      }
    }

    let timerId = setInterval(moveObstacle, 20);
    if (!isGameOver) setTimeout(generateObstacle, 3300);
  }
  generateObstacle();
```

Let's analyze the first part, just before the first conditional.
```js
    let obstacleLeft = gameDisplayWidth;
    let randomHeight = Math.random() * 60;
    let obstacleBottom = randomHeight;
    const obstacle = document.createElement("div");
    const topObstacle = document.createElement("div");
```
As you can see, we have a few variables and constants. The `obstacleLeft` variable is set to 500, which is the width of the container. This means that the obstacles are being generated outside of the container.

The variable `randomHeight` refers to the height of the obstacle bottom that will be generated. We'll use the `Math` object and its `random` method, which returns a number between zero and one. If we multiply this by 60, we will obtain a number between zero and sixty.

Next, we will use the `createElement()` method to create two new `<div>` tags. One will be for the obstacle itself, and the other for the top obstacle. The top obstacle is essentially the same as the bottom obstacle but with a 180-degree rotation.
Learn more about [createElement()](https://developer.mozilla.org/en-US/docs/Web/API/Document/createElement) method.

Learn more about [Math](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Matht) object.

Learn more about [random()](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Math/random) method.

The second part of the code verify the variable isGameOver, which is initially set as false.
```js
if (!isGameOver) {
      obstacle.classList.add("obstacle");
      topObstacle.classList.add("topObstacle");
    }
```
If it's not game over, which means we are still playing, we'll add two new styles using the `classList` property and the `add()` method

<img src="https://user-images.githubusercontent.com/76016357/219120015-20253785-a4dd-4418-8e97-3dffeed5846b.png"  width="400">

It's important to note that all elements have an absolute position. This position will help us understand the rules of the game over condition

Learn more about [classList](https://developer.mozilla.org/en-US/docs/Web/API/Element/classList) property.

Next, there's this portion of code
```js
    gameDisplay.appendChild(obstacle);
    gameDisplay.appendChild(topObstacle);
    obstacle.style.left = `${obstacleLeft}px`;
    topObstacle.style.left = `${obstacleLeft}px`;
    obstacle.style.bottom = `${obstacleBottom}px`;
    topObstacle.style.bottom = `${obstacleBottom + gap}px`;
```
Using the method `appendChild()`, we will append the obstacle to the main container. Then, using the property `style`, we will add the left and bottom styles. Remember that we already discussed the meaning of each variable. The top obstacle is a reflection of the bottom obstacle, which means that it has the same values, except for the `style.bottom` property. A gap (set to 430) is added to create the space between both obstacles.

#### 3.6 moveObstacle() function
Let's break down the function for an easier explanation.

```js
function moveObstacle() {
      obstacleLeft -= 2;
      obstacle.style.left = `${obstacleLeft}px`;
      topObstacle.style.left = `${obstacleLeft}px`;

      if (obstacleLeft === -60) {
        clearInterval(timerId);
        gameDisplay.removeChild(obstacle);
        gameDisplay.removeChild(topObstacle);
      }
      if (
        (obstacleLeft > birdLeft - obstacleWidth &&
          obstacleLeft < birdRight &&
          birdLeft === 220 &&
          (birdBottom < obstacleBottom + groundHeight ||
            birdBottom > obstacleBottom + gap - (groundHeight + birdHeight))) ||
        birdBottom === 0
      ) {
        gameOver();
        clearInterval(timerId);
      }
    }

    let timerId = setInterval(moveObstacle, 20);
```
As you may recall, the value of `obstacleLeft` was initially set equal to the width of the container, which is 500 pixels. In the `moveObstacle()` function, we will subtract 2 pixels from both the bottom and top obstacles in each call, and use the 'style.left' property to update the value.
Note: Outside the `moveObstacle()` function, there is a `setInterval()` method that calls the `moveObstacle()` function every twenty milliseconds

```js
      obstacleLeft -= 2;
      obstacle.style.left = `${obstacleLeft}px`;
      topObstacle.style.left = `${obstacleLeft}px`;
```
In the first conditional, we will check the left position of the obstacle. If the left position is equal to minus sixty pixels, it means the obstacle has just left the game container (the width of the obstacle). In that case, we will remove the obstacle using the `removeChild()` method
```js
      if (obstacleLeft === -60) {
        clearInterval(timerId);
        gameDisplay.removeChild(obstacle);
        gameDisplay.removeChild(topObstacle);
      }
```
Let's analyze this part deeply. Here we will put the conditions to execute the `gameOver()` function.
There are three ways to lose in Flappy Bird: First, if the bird touches the ground. Second, if the bird crashes into the obstacles. And finally, if the bird touches the obstacles while passing through the gap.
```js
       if (
        (obstacleLeft > birdLeft - obstacleWidth &&
          obstacleLeft < birdRight &&
          birdLeft === 220 &&
          (birdBottom < obstacleBottom + groundHeight ||
            birdBottom > obstacleBottom + gap - (groundHeight + birdHeight))) ||
        birdBottom === 0
      ) {
        gameOver();
        clearInterval(timerId);
      }
```
We will check the first condition to lose, if the bird chrash into the obstacles
```js
       
        (obstacleLeft > birdLeft - obstacleWidth &&
          obstacleLeft < birdRight &&
          birdLeft === 220)
          
```
If the obstacle's left distance is greater than the bird's left distance (set at 220) minus the obstacle's width (set at 60), and also if the obstacle's left distance is lower than the bird's right distance (set at 280), then if either of those conditions is true, it means the bird is already in the position where the obstacle is.

In both the first and second images, I'm trying to show both the upper and lower limits. Please don't get confused in the second one, as we aren't analyzing the second condition for losing, which is in the y-axis. I only put the bird in that position in order to analyze the upper limit in x-axis.

<img width="253" alt="Screenshot 2023-02-14 105741" src="https://user-images.githubusercontent.com/76016357/219260800-d8227efa-c39f-4ade-a61f-8c974e670936.png">--<img width="252" alt="Screenshot 2023-02-14 115802" src="https://user-images.githubusercontent.com/76016357/219261995-c8f87601-dfa9-4f47-b606-40d56462c86c.png">

Then, to check the y-axis conditions, we should keep in mind the absolute position of the obstacles. First, let's remember the height of the obstacle, which is 300 pixels, and the height of the ground, which is 150 pixels.

```js
        (birdBottom < obstacleBottom + groundHeight ||
            birdBottom > obstacleBottom + gap - (groundHeight + birdHeight))) || birdBottom === 0
      )
```
The ground height is the middle of the obstacle's height, therefore the value of y (in the image) is just the value of the ground height plus the random height bottom.

<img width="320" alt="Screenshot 2023-02-14 125453" src="https://user-images.githubusercontent.com/76016357/219268647-752cca29-d54c-41da-b48e-da748db82de3.png">

In order to check the top obstacle condition, we should keep an eye on the gap. Let's analyze this case where the height of the obstacle is 486 pixels.
Remember that we generate the top obtacle bottom taking the random bottom plus the gap, which is 430 pixels.

<img width="348" alt="Screenshot 2023-02-14 144100" src="https://user-images.githubusercontent.com/76016357/219269295-af993dd9-edce-47cc-99a4-f66b9aaebcae.png">

In this case, the random bottom was 56 pixels. We may be tempted to say that the condition goes only as far as that. However, we need to remember the positions of the obstacles: their position is relative to the main container, but the position of the bird is relative to the sky container. Because of this, we should subtract, as we did before, the height of the ground and also the height of the bird.

<img width="399" alt="Screenshot 2023-02-14 145654" src="https://user-images.githubusercontent.com/76016357/219271808-15c72805-63eb-41cc-8eea-f4f1343af260.png">


Finally, if the bird touch the bottom, the gameOver() function is also executed.

---------------------
You can add styles to the project and customize it as you want. I hope you have found this article helpful. Keep coding!


