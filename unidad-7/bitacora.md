# Evidencias de la unidad 7

### Actividad 01

**1. Analisis de los ejemploss de Ji Lee**


<img width="237" height="238" alt="image" src="https://github.com/user-attachments/assets/00241270-cfbf-41bc-935f-2bcb11c9fe15" />  


* **Smile**

> la L se tuerce, como un parentesis, representando la boca de la sonrisa, y ya el punto de la i sube y parece como una carita picando el ojo. 

<img width="242" height="241" alt="image" src="https://github.com/user-attachments/assets/53721f73-89f3-43da-b1b7-54fbe1a29813" />


* **Vertigo**
  
 > las palabras estan ubicadas en perspectiva para que parezca que hay profundidad, simulando que el espectador esta viendo la palabra desde una altura significativa, lo que provoca que tenga vertigo, adicionalmente, las palabras están giradas, apuntando en diferentes direcciones para enfatizar aun más la sensación de mareo cuando hay vertigo. 

<img width="240" height="242" alt="image" src="https://github.com/user-attachments/assets/1f400ac6-0233-4725-b482-5cc12d768028" />



* **The last supper**

> La palabra hace referencia a la Ultima cena de Da Vinci, donde vemos debajo de las palabras una numeración de 1 al 12, a excepción de la letra t, que no tiene nada pues representa a Jesús. Los recursos graficos son la animación de la letra T para que sea la cruz, incluso la tipografia ayuda pues usa la minuscula para verse mas como una cruz. 

**2. Ideas propias**

* **Light**
![Imagen de WhatsApp 2025-10-13 a las 18 48 16_bcb6aa66](https://github.com/user-attachments/assets/310d79a4-59a6-4987-b07d-f30ed7f0c01c)

* **Bounce y Grow**

![Imagen de WhatsApp 2025-10-13 a las 18 48 16_4290c956](https://github.com/user-attachments/assets/f529e9ef-8016-403f-97cc-7cdc671c02ca)


### Actividad 02

1. codigos ejercicios

* ejercicio 1
```js
const { Engine, World, Bodies } = Matter;

let engine, world;
let boxes = [];
let ground;

function setup() {
  createCanvas(600, 400);
  engine = Engine.create();
  world = engine.world;

  ground = Bodies.rectangle(300, height - 10, 600, 20, { isStatic: true });
  World.add(world, ground);
}

function mousePressed() {
  let box = Bodies.rectangle(mouseX, mouseY, 40, 40);
  boxes.push(box);
  World.add(world, box);
}

function draw() {
  background(220);
  Engine.update(engine);
  
  for (let b of boxes) {
    rectMode(CENTER);
    rect(b.position.x, b.position.y, 40, 40);
  }

  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, 600, 20);
}

```

* ejercicio 2
```js
const { Engine, World, Bodies, Mouse, MouseConstraint } = Matter;

let engine, world;
let ground, box, mConstraint;

function setup() {
  createCanvas(600, 400);
  engine = Engine.create();
  world = engine.world;

  ground = Bodies.rectangle(300, 380, 600, 20, { isStatic: true });
  box = Bodies.rectangle(300, 100, 50, 50);
  World.add(world, [ground, box]);

  const canvasMouse = Mouse.create(canvas.elt);
  const options = {
    mouse: canvasMouse
  };
  mConstraint = MouseConstraint.create(engine, options);
  World.add(world, mConstraint);
}

function draw() {
  background(230);
  Engine.update(engine);

  rectMode(CENTER);
  rect(ground.position.x, ground.position.y, 600, 20);
  rect(box.position.x, box.position.y, 50, 50);
}
```
* ejercicio 3
  
```js
const { Engine, World, Bodies, Body, Constraint, Mouse, MouseConstraint } = Matter;

let engine, world;
let canvas;

let ground;
let ramp, hinge;
let ball;
let cubes = [];
let links = [];
let mConstraint;

const COLORS = {
  navy:  [7, 55, 99],
  orange:[246, 167, 74],
  red:   [241, 90, 50],
  yellow:[243, 204, 80],
  line:  [200, 200, 200]
};

function setup() {
  canvas = createCanvas(720, 520);

  engine = Engine.create();
  world = engine.world;
  engine.world.gravity.y = 1;
  
  ground = Bodies.rectangle(width/2, height - 8, width, 16, {
    isStatic: true,
    friction: 0.9
  });
  World.add(world, ground);

  ramp = Bodies.rectangle(390, height - 60, 360, 20, { friction: 0.7, density: 0.002 });
  Body.rotate(ramp, -0.25);
  hinge = Constraint.create({
    pointA: { x: 330, y: height - 130 }, // punto fijo del mundo
    bodyB: ramp,
    pointB: { x: -60, y: 0 },
    length: 0,
    stiffness: 1
  });
  World.add(world, [ramp, hinge]);

  ball = Bodies.circle(560, height - 80, 48, {
    restitution: 0.1,
    friction: 0.5,
    density: 0.002
  });
  World.add(world, ball);

  const n = 6;
  const size = 28;
  const startX = 200, startY = 160;

  for (let i = 0; i < n; i++) {
    const cube = Bodies.rectangle(startX, startY + i * (size + 4), size, size, {
      friction: 0.6,
      restitution: 0.05
    });
    cubes.push(cube);
  }
  for (let i = 0; i < cubes.length - 1; i++) {
    const link = Constraint.create({
      bodyA: cubes[i],
      bodyB: cubes[i + 1],
      length: 30,
      stiffness: 0.8
    });
    links.push(link);
  }
  World.add(world, cubes);
  World.add(world, links);

  const canvasMouse = Mouse.create(canvas.elt);
  mConstraint = MouseConstraint.create(engine, { mouse: canvasMouse });
  World.add(world, mConstraint);
}

function draw() {
  background(255);
  Engine.update(engine);

  stroke(...COLORS.line);
  strokeWeight(2);
  line(0, height - 16, width, height - 16);
  noStroke();

  drawPoly(ramp, COLORS.yellow);
  push();
  noStroke();
  fill(230);
  rectMode(CENTER);
  rect(250, height - 52, 22, 44, 3);
  rect(330, height - 120, 22, 64, 3);
  pop();

 
  push();
  fill(220);
  circle(hinge.pointA.x, hinge.pointA.y, 10);
  pop();

  
  drawCircle(ball, COLORS.navy);

  for (let i = 0; i < cubes.length; i++) {
    const col = (i === 2) ? COLORS.red : (i % 2 === 0 ? COLORS.orange : COLORS.navy);
    drawPoly(cubes[i], col);
  }
}

function drawPoly(body, rgb) {
  push();
  fill(...rgb);
  noStroke();
  beginShape();
  for (let v of body.vertices) vertex(v.x, v.y);
  endShape(CLOSE);
  pop();
}

function drawCircle(body, rgb) {
  push();
  fill(...rgb);
  noStroke();
  circle(body.position.x, body.position.y, body.circleRadius * 2);
  pop();
}
```
2. enlaces

* ejercicio 1
  
  [boxes](https://editor.p5js.org/saragaravitop/sketches/IyrGw6j1O)

* ejercicio 2
  
  [box](https://editor.p5js.org/saragaravitop/sketches/KAjvtGRMS)

* ejercicio 3
  
  [example Matter.js](https://editor.p5js.org/saragaravitop/sketches/qzizAfD9c)
  
**3. explicaciones**

* **Engine:** como su nombre lo dice, es el motor que calcula las fuerzas, la gravedad y las colisiones.

* **World:** es donde se almacenan los cuerpos.

* **Bodies:** son los objetos físicos como rectángulos, círculos, polígonos, entre otros.

* **Constraint:** son como los enlaces o restricciones entre cuerpos.

* **MouseConstraint:** es la función que permite mover los cuerpos con el mouse.

* **Runner/Events:** son los que ejecutan el motor en bucle y permiten detectar acciones.
  
**4. Dificultades**

> Realmente no tuve muchas dificultades porque hice los ejercicios siguiendo el tutorial propuesto y fue muy sencillo, aunque he de decir que el primer ejercicio no me funciono tal cual y me toco hacerle unos cambios. El ejercicio 3 fue mas como probar algo mas dificil que claramente busqué en internet como hacerlo, pero fue muy divertido porque aprendí mas sobre el funcionamiento de matter.js. 
