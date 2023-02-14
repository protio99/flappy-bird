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

##### 1.Folder structure
We don't have anything too complicated here, as it's a Vanilla JavaScript project. We only have an index.html file, an app.js file, and a stylesheet for our styles. You can check the folder structure above.

##### 2. index.html
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
![basic stylesheet](https://drive.google.com/file/d/1o2rb6NmFRd0lPEwJ9dhbMMTmurwJ0xUE/view?usp=share_link)
