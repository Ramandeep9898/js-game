# JS-GAME

The PLAYER (in centre) Shot Balls (Projectiles) to kill Enemies and if any Enemy touch the PLAYER "GAME OVER".
`P.s.~ use Chorme FOR BEST VEIW` <br>
[click here](https://js-gamee.netlify.app/)

```
git clone https://github.com/Ramandeep9898/js-game.git
```

# ⚙️BASIC SETUP

- ## HTML

```html
<canvas> </canvas>
```

- ## CSS

```css
* {
  margin: 0;
  padding: 0;
  font-family: "Courier New", Courier, monospace;
}
```

- ## JS

```javascript
const canvas = document.querySelector("canvas");

canvas.width = innerWidth;
canvas.height = innerHeight;
```

> _this will remove default browser pading nd will give it same height nd width as window_

- ## CDN

```javascript
<script
  src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.5.1/gsap.min.js"
  integrity="sha512-IQLehpLoVS4fNzl7IfH8Iowfm5+RiMGtHykgZJl9AWMgqx0AmJ6cRWcB+GaGVtIsnC4voMfm8f2vwtY+6oPjpQ=="
  crossorigin="anonymous"
></script>
```

# 🚀TO-DO LIST :)

1. Create a Player.
1. Shoot Projectiles.
1. Create Enimes.
1. Detect Collisions on Enemy/Projectile Hit.
1. Detect Collisions on Enemy/Player Hit.
1. Remove off screen Projectiles.
1. Colorize Game.
1. Shrink Enemies on Hit.
1. Create Particles Explosion on Hit.
1. Add Score.
1. Add Start Game Btn.
1. Add Game over UI.
1. Add Restart Button.

# 1. CREATE A PLAYER🔨

![image info](img/img1.png)

To Draw on Canvas we need to specify context of Canvas first.

```javascript
const c = canvas.getContext("2d");
```

What properties Do a Player have ??

- Always in Centre (i.e. it's x,y positions).
- It's size (i.e. it's radius).
- Color.

```javascript
class Player {
  constructor(x, y, radius, color) {
    this.x = x;
    this.y = y;
    this.radius = radius;
    this.color = color;
  }
}
let player = new Player(x, y, 20, "white");
```

Now to Draw the Player on Screen

```javascript
class Particle {
  constructor(x, y, radius, color, velocity) {
    this.x = x;
    this.y = y;
    this.radius = radius;
    this.color = color;
    this.velocity = velocity;
    this.alpha = 1;
  }

  draw() {

    c.beginPath(); //tell canvas to begin the path
    c.arc(this.x, this.y, this.radius, Math.PI * 2, false); // make circle (arc)
    c.fillStyle = this.color;
    c.fill(); //fill's the palyer with white color

  }
  const x = canvas.width / 2;
  const y = canvas.height / 2;

  let player = new Player(x, y, 20, "white");
  player.draw()
```

# 2. Shoot Projectiles.🔨

![image info](img/img2.jpeg)

What properties a projectile have ??

- x,y positions co-ordinates.
- it's size (i.e. it's radius).
- Color.
- where ever we click it make's new projectile.
- projectile's Velocity.(most imp.)

```javascript
class Projectile {
  constructor(x, y, radius, color, velocity) {
    this.x = x;
    this.y = y;
    this.radius = radius;
    this.color = color;
    this.velocity = velocity;
  }

  draw() {
    c.beginPath();
    c.arc(this.x, this.y, this.radius, Math.PI * 2, false);
    c.fillStyle = this.color;
    c.fill();
  }
}
```

To make new projectile "on click" we will add a event listener.

```javascript
addEventListener("click", (event) => {
  const pojectile = new Projectile(
    event.clientX,
    event.clientY,
    5,
    "red",
    null
  );

  projectile.draw();
});
```

![image info](img/img3.jpg)

Now, where ever we click we get a projectile on the sepecific location. But nthg is moving. To make the balls move use little tirgno.

> We don't want particals to spawning where ever we click. we want them to spawn from center.

> we don't have any potential to move it. so, we will make a animation loop.

> we want to draw multiple particle on the screen within an animate loop nd to get these rendered on screen and moving into different directions at the same exact time we need to create an Array.
> ~A group of Projectiles and then draw them all out at the same time

```javaScript

class Projectile {
  constructor(x, y, radius, color, velocity) {
    this.x = x;
    this.y = y;
    this.radius = radius;
    this.color = color;
    this.velocity = velocity;
  }

  draw() {
    c.beginPath();
    c.arc(this.x, this.y, this.radius, Math.PI * 2, false);
    c.fillStyle = this.color;
    c.fill();
  }

  update() {
    this.draw();
    this.x = this.x + this.velocity.x;
    this.y = this.y + this.velocity.y;
  }
}
const projectiles = []
function animate() {
  requestAnimationFrame(animate);
  c.fillRect(0, 0, canvas.width, canvas.height);
  player.draw();
  particles.forEach((projectile) => {
      particle.update();
  });
}

addEventListener("click", (event) => {

  const angle = Math.atan2(
    event.clientY - canvas.height / 2,
    event.clientX - canvas.width / 2
  );
  const velocity = {
    x: Math.cos(angle) * 5,
    y: Math.sin(angle) * 5,
  };

  projectiles.push(
    new Projectile(canvas.width / 2, canvas.height / 2, 5, "white", velocity)
  );
});

```

# 3.Create Enimes.🔨

> **PROPERTY OF ENEMY:**

```java script
class Enemy {
  constructor(x, y, radius, color, velocity) {
    this.x = x;
    this.y = y;
    this.radius = radius;
    this.color = color;
    this.velocity = velocity;
  }

  draw() {
    c.beginPath();
    c.arc(this.x, this.y, this.radius, Math.PI * 2, false);
    c.fillStyle = this.color;
    c.fill();
  }

  update() {
    this.draw();
    this.x = this.x + this.velocity.x;
    this.y = this.y + this.velocity.y;
  }
}
```

> We Need to create Enemies that come from outside of screen towards centre

```javascript
let enemies = [];

function spawnEnemies() {
  setInterval(() => {
    const radius = Math.random() * (30 - 5) + 5; //i.e any value 30-5
    let x;
    let y;
    if (Math.random() < 0.5) {
      x = Math.random() < 0.5 ? 0 - radius : canvas.width + radius;
      //------>when math.radom value is smaller than 0.5 than we will asign(?) it 0-radius.... if greater than .5 it will be(:) canvas.width+radius
      y = Math.random() * canvas.height;
    } else {
      x = Math.random() * canvas.width;
      y = Math.random() < 0.5 ? 0 - radius : canvas.height + radius;
    }

    const color = `hsl(${Math.random() * 360}, 50%, 50%)`;
    const angle = Math.atan2(canvas.height / 2 - y, canvas.width / 2 - x);
    // if u do y - canvas height nd  x - canvaswidth enemy will go in opp direction
    const velocity = {
      x: Math.cos(angle),
      y: Math.sin(angle),
    };

    enemies.push(new Enemy(x, y, radius, color, velocity));
  }, 1000);
}
```

# 4. Detect Collisions on Enemy/Projectile Hit.🔨

> when Collisions happens we will remove enemy from screen.

> To do soo, We need to Go inside the animate loop because for each frame we need to check whether or not our projectile touching the enemy

> So, we r looping throuh all of our enemies already but for each enemy on screen we'll want to loop through each projectile on the screen this way for enemy we can test the distance between every single projectile that we have so we're going to have a loop within a loop

> To remove flash effect add it in set tiomeout loop.

```javascript
function animate() {
  animationId = requestAnimationFrame(animate);

  c.fillRect(0, 0, canvas.width, canvas.height);
  player.draw();

  projectiles.forEach((projectile) => {
    projectile.update();
  });

  enemies.forEach((enemy, index) => {
    enemy.update();

    projectiles.forEach((projectile, projectileIndex) => {
      const dist = Math.hypot(projectile.x - enemy.x, projectile.y - enemy.y);

      //when projectiles touch enemy
      if (dist - enemy.radius - projectile.radius < 1) {
        setTimeout(() => {
          enemies.splice(index, 1);
          projectiles.splice(projectileIndex, 1);
        }, 0);
      }
    });
  });
}
```

# 5.Detect Collisions on Enemy/Player Hit.🔨

> When an enemy hits the our player we are just going to end the the Game altogether pause the whole game.
> Eventually we'll add an end game screen with score.
> We have to do is check the distance betwwen each enemy and our actucal player.
> and to end the game we'll cancel the request animation frame.

```javascript
let animationId;
function animate() {
  animationId = requestAnimationFrame(animate);
  c.fillRect(0, 0, canvas.width, canvas.height);
  player.draw();


  projectiles.forEach((projectile) => {
    projectile.update();



  enemies.forEach((enemy, index) => {
    enemy.update();
    const dist = Math.hypot(player.x - enemy.x, player.y - enemy.y);

    //end game
    if (dist - enemy.radius - player.radius < 1) {
      cancelAnimationFrame(animationId);
    }

    projectiles.forEach((projectile, projectileIndex) => {
      const dist = Math.hypot(projectile.x - enemy.x, projectile.y - enemy.y);

      //when projectiles touch object
      if (dist - enemy.radius - projectile.radius < 1) {
          setTimeout(() => {
            enemies.splice(index, 1);
            projectiles.splice(projectileIndex, 1);
          }, 0);
        }
      }
    );
  });
}
```

# 6.Remove off screen Projectiles.🔨

When we shoot projectles, they're going to start going off the screen and even they go off the screen we're stll going to animate them.

> we need to make sure that once they off the screen they no longer in the game. they are removed from our array and we don;t actucally perform any compution on them at all.
> So, we are performing collision detection but more so when our projectile colids with the very edges of our screen.

```javascript
let animationId;
let score = 0;
function animate() {
  animationId = requestAnimationFrame(animate);
  c.fillRect(0, 0, canvas.width, canvas.height);
  player.draw();
  particles.forEach((particle, index) => {
    if (particle.alpha <= 0) {
      particles.splice(index, 1);
    } else {
      particle.update();
    }
  });

  projectiles.forEach((projectile, index) => {
    projectile.update();

    //remove from edges of screen
    if (
      projectile.x + projectile.radius < 0 ||
      projectile.x - projectile.radius > canvas.width ||
      projectile.y + projectile.radius < 0 ||
      projectile.y - projectile.radius > canvas.height
    ) {
      setTimeout(() => {
        projectiles.splice(index, 1);
      }, 0);
    }
  });

  enemies.forEach((enemy, index) => {
    enemy.update();
    const dist = Math.hypot(player.x - enemy.x, player.y - enemy.y);

    //end game
    if (dist - enemy.radius - player.radius < 1) {
      cancelAnimationFrame(animationId);
      score_board.style.display = "grid";
      bigScore.innerHTML = score;
    }

    projectiles.forEach((projectile, projectileIndex) => {
      const dist = Math.hypot(projectile.x - enemy.x, projectile.y - enemy.y);

      if (dist - enemy.radius - projectile.radius < 1) {
        setTimeout(() => {
          enemies.splice(index, 1);
          projectiles.splice(projectileIndex, 1);
        }, 0);
      }
    });
  });
}
```

# 7.Colorize Game.🔨

To make enemies of diffrent color.

```javascript
function spawnEnemies() {
  setInterval(() => {
    const radius = Math.random() * (30 - 5) + 5;
    let x;
    let y;
    if (Math.random() < 0.5) {
      x = Math.random() < 0.5 ? 0 - radius : canvas.width + radius;

      y = Math.random() * canvas.height;
    } else {
      x = Math.random() * canvas.width;
      y = Math.random() < 0.5 ? 0 - radius : canvas.height + radius;
    }
    //to randomize the color to enemy
    //hsl( hue, saturation, lightness)
    const color = `hsl(${Math.random() * 360}, 50%, 50%)`;
    const angle = Math.atan2(canvas.height / 2 - y, canvas.width / 2 - x);

    const velocity = {
      x: Math.cos(angle),
      y: Math.sin(angle),
    };
    99;

    enemies.push(new Enemy(x, y, radius, color, velocity));
  }, 1000);
}
```

# 8.Shrink Enemies on Hit.🔨

> If enemy's radis is greater than certain size, then we want subtratct a value from that radius instead of removing it all together.

> But when it's beneath certain size that is when we're going to remove it from our screen and take it out of the game.

```javascript
let animationId;
function animate() {
  animationId = requestAnimationFrame(animate);
  c.fillStyle = "rgba(0, 0, 0, 0.1)";
  c.fillRect(0, 0, canvas.width, canvas.height);
  player.draw();
  particles.forEach((particle, index) => {
    if (particle.alpha <= 0) {
      particles.splice(index, 1);
    } else {
      particle.update();
    }
  });

  projectiles.forEach((projectile, index) => {
    projectile.update();

    //remove from edges of screen
    if (
      projectile.x + projectile.radius < 0 ||
      projectile.x - projectile.radius > canvas.width ||
      projectile.y + projectile.radius < 0 ||
      projectile.y - projectile.radius > canvas.height
    ) {
      setTimeout(() => {
        projectiles.splice(index, 1);
      }, 0);
    }
  });

  enemies.forEach((enemy, index) => {
    enemy.update();
    const dist = Math.hypot(player.x - enemy.x, player.y - enemy.y);

    //end game
    if (dist - enemy.radius - player.radius < 1) {
      cancelAnimationFrame(animationId);
    }

    projectiles.forEach((projectile, projectileIndex) => {
      const dist = Math.hypot(projectile.x - enemy.x, projectile.y - enemy.y);

      //when projectiles touch touch enemy
      if (dist - enemy.radius - projectile.radius < 1) {
        if (enemy.radius - 10 > 10) {
          enemy.radius -= 10;
          setTimeout(() => {
            projectiles.splice(projectileIndex, 1);
          }, 0);
        } else {
          setTimeout(() => {
            enemies.splice(index, 1);
            projectiles.splice(projectileIndex, 1);
          }, 0);
        }
      }
    });
  });
}
```

it's not shrinking progressively and also it don't have the smooth animation. for that we'll use Gsap

> So, instead of immediately subtracting 10 from enemy's radius Actually going to tween between this radius value. <br> i.e It is going to add smootha animation effect.

```javascript
let animationId;
function animate() {
  animationId = requestAnimationFrame(animate);
  c.fillStyle = "rgba(0, 0, 0, 0.1)";
  c.fillRect(0, 0, canvas.width, canvas.height);
  player.draw();
  particles.forEach((particle, index) => {
    if (particle.alpha <= 0) {
      particles.splice(index, 1);
    } else {
      particle.update();
    }
  });

  projectiles.forEach((projectile, index) => {
    projectile.update();

    //remove from edges of screen
    if (
      projectile.x + projectile.radius < 0 ||
      projectile.x - projectile.radius > canvas.width ||
      projectile.y + projectile.radius < 0 ||
      projectile.y - projectile.radius > canvas.height
    ) {
      setTimeout(() => {
        projectiles.splice(index, 1);
      }, 0);
    }
  });

  enemies.forEach((enemy, index) => {
    enemy.update();
    const dist = Math.hypot(player.x - enemy.x, player.y - enemy.y);

    //end game
    if (dist - enemy.radius - player.radius < 1) {
      cancelAnimationFrame(animationId);
      score_board.style.display = "grid";
      bigScore.innerHTML = score;
    }

    projectiles.forEach((projectile, projectileIndex) => {
      const dist = Math.hypot(projectile.x - enemy.x, projectile.y - enemy.y);
      if (dist - enemy.radius - projectile.radius < 1) {
        if (enemy.radius - 10 > 10) {
          gsap.to(enemy, {
            radius: enemy.radius - 10,
          });
          setTimeout(() => {
            projectiles.splice(projectileIndex, 1);
          }, 0);
        } else {
          score += 250;
          scoreEl.innerHTML = score;

          setTimeout(() => {
            enemies.splice(index, 1);
            projectiles.splice(projectileIndex, 1);
          }, 0);
        }
      }
    });
  });
}
```

# 9.Create Particles Explosion on Hit.🔨

> It add nice burst effect enemies exploading into million pieces.<br>

> To add this effect we basically need to create all these particles independent from one another. when ever our projectile hits an enemy.
> <br> Particles that have those indiviual properties by which they all go in diffrent direction.

```javascript
class Particle {
  constructor(x, y, radius, color, velocity) {
    this.x = x;
    this.y = y;
    this.radius = radius;
    this.color = color;
    this.velocity = velocity;
  }

  draw() {
    c.beginPath();
    c.arc(this.x, this.y, this.radius, Math.PI * 2, false);
    c.fillStyle = this.color;
    c.fill();
  }

  update() {
    this.draw();
    this.x = this.x + this.velocity.x;
    this.y = this.y + this.velocity.y;
  }
}
```

> When even projectile touches an enemy we want to create an enemy we want to create a number of particles and Render them on the screen.

```javascript
let player = new Player(x, y, 20, "white");
let projectiles = [];
let enemies = [];
let particles = [];

let animationId;
function animate() {
  animationId = requestAnimationFrame(animate);
  c.fillStyle = "rgba(0, 0, 0, 0.1)";
  c.fillRect(0, 0, canvas.width, canvas.height);
  player.draw();
  particles.forEach((particle, index) => {
    particle.update();
  });

  projectiles.forEach((projectile, index) => {
    projectile.update();

    //remove from edges of screen
    if (
      projectile.x + projectile.radius < 0 ||
      projectile.x - projectile.radius > canvas.width ||
      projectile.y + projectile.radius < 0 ||
      projectile.y - projectile.radius > canvas.height
    ) {
      setTimeout(() => {
        projectiles.splice(index, 1);
      }, 0);
    }
  });

  enemies.forEach((enemy, index) => {
    enemy.update();
    const dist = Math.hypot(player.x - enemy.x, player.y - enemy.y);

    projectiles.forEach((projectile, projectileIndex) => {
      const dist = Math.hypot(projectile.x - enemy.x, projectile.y - enemy.y);

      //when projectiles touch enemy
      if (dist - enemy.radius - projectile.radius < 1) {
        //create explosions------->
        for (let i = 0; i < enemy.radius * 2; i++) {
          particles.push(
            new Particle(
              projectile.x,
              projectile.y,
              Math.random() * 2,
              enemy.color,
              {
                x: (Math.random() - 0.5) * 5,
                //math. random is always gonna be +ve(0-1)
                // to make it completely random value that can also be -ve
                y: (Math.random() - 0.5) * 5,
              }
            )
          );
        }
      }
    });
  });
}
```

> Now, We have particles keep on Going they don't actually fade out or Disappear over time.<br>
> Todo so,add aditional property. <br>
> ~Alpha value.<br>
> Starting alpha value will always going to 1 bcoz it's always going to opaque first but, as time goes on with this particles we're going to subtract an alpha value from it( which is going to fade it out over time).
> add frition so that thing don't slow down quickly.

```javascript
const friction = 0.98;
class Particle {
  constructor(x, y, radius, color, velocity) {
    this.x = x;
    this.y = y;
    this.radius = radius;
    this.color = color;
    this.velocity = velocity;
    this.alpha = 1;
  }

  draw() {
    c.save();
    c.globalAlpha = this.alpha;
    c.beginPath(); //begin the path
    c.arc(this.x, this.y, this.radius, Math.PI * 2, false); // make circle
    c.fillStyle = this.color;
    c.fill();
    c.restore();
  }

  update() {
    this.draw();
    this.velocity.x *= friction;
    this.velocity.y *= friction;
    this.x = this.x + this.velocity.x;
    this.y = this.y + this.velocity.y;
    this.alpha -= 0.01;
  }
}

let animationId;
function animate() {
  animationId = requestAnimationFrame(animate);
  c.fillStyle = "rgba(0, 0, 0, 0.1)";
  c.fillRect(0, 0, canvas.width, canvas.height);
  player.draw();
  particles.forEach((particle, index) => {
    if (particle.alpha <= 0) {
      particles.splice(index, 1);
    } else {
      particle.update();
    }
  });

  projectiles.forEach((projectile, index) => {
    projectile.update();

    //remove from edges of screen
    if (
      projectile.x + projectile.radius < 0 ||
      projectile.x - projectile.radius > canvas.width ||
      projectile.y + projectile.radius < 0 ||
      projectile.y - projectile.radius > canvas.height
    ) {
      setTimeout(() => {
        projectiles.splice(index, 1);
      }, 0);
    }
  });

  enemies.forEach((enemy, index) => {
    enemy.update();
    const dist = Math.hypot(player.x - enemy.x, player.y - enemy.y);

    projectiles.forEach((projectile, projectileIndex) => {
      const dist = Math.hypot(projectile.x - enemy.x, projectile.y - enemy.y);

      //when projectiles touch touch enemy
      if (dist - enemy.radius - projectile.radius < 1) {
        //create explosions------->

        for (let i = 0; i < enemy.radius * 2; i++) {
          particles.push(
            new Particle(
              projectile.x,
              projectile.y,
              Math.random() * 2,
              enemy.color,
              {
                x: (Math.random() - 0.5) * 5,
                y: (Math.random() - 0.5) * 5,
              }
            )
          );
        }
      }
    });
  });
}
```

# 10. Add Score.

## HTML

```HTML
<div class="Score_div">
      <span> Score : </span>
      <span id="Score"> 0 </span>
    </div>
```

## CSS

U can style however u want.

## JS

```javascript
const scoreEl = document.querySelector("#Score");

let animationId;
let score = 0;
function animate() {
  animationId = requestAnimationFrame(animate);
  c.fillStyle = "rgba(0, 0, 0, 0.1)";
  c.fillRect(0, 0, canvas.width, canvas.height);
  player.draw();
  particles.forEach((particle, index) => {
    if (particle.alpha <= 0) {
      particles.splice(index, 1);
    } else {
      particle.update();
    }
  });

  projectiles.forEach((projectile, index) => {
    projectile.update();

    //remove from edges of screen
    if (
      projectile.x + projectile.radius < 0 ||
      projectile.x - projectile.radius > canvas.width ||
      projectile.y + projectile.radius < 0 ||
      projectile.y - projectile.radius > canvas.height
    ) {
      setTimeout(() => {
        projectiles.splice(index, 1);
      }, 0);
    }
  });

  enemies.forEach((enemy, index) => {
    enemy.update();
    const dist = Math.hypot(player.x - enemy.x, player.y - enemy.y);

    projectiles.forEach((projectile, projectileIndex) => {
      const dist = Math.hypot(projectile.x - enemy.x, projectile.y - enemy.y);

      //when projectiles touch touch enemy
      if (dist - enemy.radius - projectile.radius < 1) {
        //create explosions------->

        for (let i = 0; i < enemy.radius * 2; i++) {
          particles.push(
            new Particle(
              projectile.x,
              projectile.y,
              Math.random() * 2,
              enemy.color,
              {
                x: (Math.random() - 0.5) * 5,
                y: (Math.random() - 0.5) * 5,
              }
            )
          );
        }
        if (enemy.radius - 10 > 10) {
          //increase our score
          score += 100;
          scoreEl.innerHTML = score;

          gsap.to(enemy, {
            radius: enemy.radius - 10,
          });
          setTimeout(() => {
            projectiles.splice(projectileIndex, 1);
          }, 0);
        } else {
          //REMOVE FROM SCREEN ALTOGETHER
          score += 250;
          scoreEl.innerHTML = score;

          setTimeout(() => {
            enemies.splice(index, 1);
            projectiles.splice(projectileIndex, 1);
          }, 0);
        }
      }
    });
  });
}
```

# 11. Add Start Game Btn.🔨

## HTML

```html
<div id="score_board" class="Score_board">
  <h1 id="bigScore">0</h1>
  <h3>Points</h3>
  <div>
    <button id="StartGameBtn" class="Score_btn">Start Game</button>
  </div>
</div>
```

## CSS

### **Style however u want.**

## JS

```JAVASCRIPT
function init() {
  player = new Player(x, y, 20, "white");
  projectiles = [];
  enemies = [];
  particles = [];
  score = 0;
  scoreEl.innerHTML = score;
  bigScore.innerHTML = score;
}


const startGameBtn = document.querySelector("#StartGameBtn");
const score_board = document.querySelector("#score_board");
const bigScore = document.querySelector("#bigScore");
startGameBtn.addEventListener("click", () => {
  init();
  animate();
  spawnEnemies();
  score_board.style.display = "none";
});


let animationId;
let score = 0;
function animate() {
  animationId = requestAnimationFrame(animate);
  c.fillStyle = "rgba(0, 0, 0, 0.1)";
  c.fillRect(0, 0, canvas.width, canvas.height);
  player.draw();
  particles.forEach((particle, index) => {
    if (particle.alpha <= 0) {
      particles.splice(index, 1);
    } else {
      particle.update();
    }
  });

  projectiles.forEach((projectile, index) => {
    projectile.update();

    //remove from edges of screen
    if (
      projectile.x + projectile.radius < 0 ||
      projectile.x - projectile.radius > canvas.width ||
      projectile.y + projectile.radius < 0 ||
      projectile.y - projectile.radius > canvas.height
    ) {
      setTimeout(() => {
        projectiles.splice(index, 1);
      }, 0);
    }
  });

  enemies.forEach((enemy, index) => {
    enemy.update();
    const dist = Math.hypot(player.x - enemy.x, player.y - enemy.y);

    //end game
    if (dist - enemy.radius - player.radius < 1) {
      cancelAnimationFrame(animationId);
      score_board.style.display = "grid";
      bigScore.innerHTML = score;
    }

    projectiles.forEach((projectile, projectileIndex) => {
      const dist = Math.hypot(projectile.x - enemy.x, projectile.y - enemy.y);

      //when projectiles touch touch enemy
      if (dist - enemy.radius - projectile.radius < 1) {
        //create explosions------->

        for (let i = 0; i < enemy.radius * 2; i++) {
          particles.push(
            new Particle(
              projectile.x,
              projectile.y,
              Math.random() * 2,
              enemy.color,
              {
                x: (Math.random() - 0.5) * 5,
                y: (Math.random() - 0.5) * 5,
              }
            )
          );
        }
        if (enemy.radius - 10 > 10) {
          //increase our score
          score += 100;
          scoreEl.innerHTML = score;

          gsap.to(enemy, {
            radius: enemy.radius - 10,
          });
          setTimeout(() => {
            projectiles.splice(projectileIndex, 1);
          }, 0);
        } else {
          //REMOVE FROM SCREEN ALTOGETHER
          score += 250;
          scoreEl.innerHTML = score;

          setTimeout(() => {
            enemies.splice(index, 1);
            projectiles.splice(projectileIndex, 1);
          }, 0);
        }
      }
    });
  });
}

```
