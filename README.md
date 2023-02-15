# Flappy bird/gosh clone :hatched_chick:
--------------
*This project was proposed on the freeCodeCamp website in a video tutorial created by the YouTuber and developer Ania Kubów.*
- *Go to [freeCodeCamp](https://www.freecodecamp.org/espanol/news/40-proyectos-de-javascript-para-principiantes-ideas-faciles-para-empezar-a-codificar-en-js/#c-mo-crear-siete-juegos-cl-sicos-con-ania-kubow) article.*
- *Go to Ania Kubów's [YouTube](https://www.youtube.com/channel/UC5DNytAJ6_FISueUfzZCVsw) channel, and more specfically, the [tutorial](https://www.youtube.com/watch?v=8xPsg6yv7TU&ab_channel=freeCodeCamp.org) that I will be referring*
---------------------
Hi! Welcome to this project :hugs:. This is a clone of the famous game Flappy Bird created using Vanilla JavaScript, the DOM, and a few interesting methods that helped us create the motion effect:
* createElement()
* forEach()
* setInterval()
* clearInterval()
* addEventListener() ✨
* removeEventListener()

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
  //First check that the bird is not outside the game container
    if (birdBottom < gameDisplayHeight) {
      birdBottom += 50;
      bird.style.bottom = `${birdBottom}px`;
    }
  }
  document.addEventListener("keyup", control);
```

