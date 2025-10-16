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


### Actividad 03

**1. Palabra elegida**
 
  > mi palabra elegida es **light**.

**2. Idea conceptual**

  > La idea de mi obra surge de representar la luz mediante los bombillos. Uitlizo la palabra light por el concepto "word as image" de Ji Lee. El propósito es que cuando el enchufe se conecta al tomacorriente la palabra resalta, se ilumina en medio de la oscuridad. 

**3. Aspectos técnicos clave**

  * **Formación de las letras:**
  > Las letras no las construí con cuerpos físicos independientes, sino que los dibuje con p5.js en un canvas auxiliar llamado textGraphics. Para hacer los bombillos internos, calculamos las coordenadas para después distribuirlos dentro y en el borde de cada letra para que se vea la silueta de la palabra. 

  * **Propiedades físicas:**
  > Motor de física Matter.Engine con gravedad en y de 1.05, para que el cable y el enchufe tengan un movimiento natural.
  > Los cuadritos que componen el cable son rectangulos con fricción y densidad baja, para que se balanceen y se comporten como una cuerda flexible.
  > El enchufe es un rectangulo más pesado que puede colisionar con el enchufe.
  > El tomacorriente es un cuadrado estático con sensor para detectar las colisiones sin impedir el movimiento.

  * **Restricciones utilizadas:**
  > Uso constraints entre los segmentos del cable y entre el último segmento y el enchufe para simular una cadena. También hay una restricción inicial que fija el cable a un punto superior para que este colgando de la pared.

4. Código

```js
const { Engine, World, Bodies, Constraint, Mouse, MouseConstraint, Collision, Composite } = Matter;
let engine, world, canvas;
const W = 900, H = 540;
const WORD_TEXT = "LIGHT";
const BULB_STEP   = 20;   // menor = más focos
const BULB_DIAM   = 12;   // diámetro idéntico
const EDGE_CLEAR  = 8;    // separación mínima del borde

let bulbsFill = [];   // focos interiores
let bulbsEdge = [];   // focos en borde 
let powered = false;
let textG;            

// Física / interacción
let plug, outlet, mConstraint;

function setup() {
  canvas = createCanvas(W, H);
  engine = Engine.create();
  world = engine.world;
  world.gravity.y = 1.05;
  World.add(world, Bodies.rectangle(W/2, H-4, W, 8, { isStatic: true }));
  outlet = Bodies.rectangle(120, H-120, 36, 62, { isStatic: true, isSensor: true });
  World.add(world, outlet);
  createCableAndPlug({
    anchor: { x: 80, y: 80 },
    segments: 10,
    segSize: { w: 22, h: 10 },
    plugSize: { w: 50, h: 28 }
  });

  const canvasMouse = Mouse.create(canvas.elt);
  mConstraint = MouseConstraint.create(engine, { mouse: canvasMouse });
  World.add(world, mConstraint);
  buildBulbs();
}

function draw() {
  background(0); 
  Engine.update(engine);
  drawWordBase();
  drawBulbs();
  if (powered) drawWordGlow();
  drawOutlet();
  drawCable();
  drawPlug();
  powered = isPlugTouchingOutlet();
}

function buildBulbs() {
  textG = createGraphics(W, H);
  textG.pixelDensity(1);
  textG.clear();
  textG.fill(255); 
  textG.noStroke();
  textG.textAlign(CENTER, CENTER);
  textG.textStyle(BOLD);
  textG.textSize(220);
  textG.text(WORD_TEXT, W/2, H/2 + 20);
  textG.loadPixels();

  const inside = (x, y) => {
    if (x < 0 || x >= W || y < 0 || y >= H) return false;
    const idx = 4 * (Math.floor(y) * W + Math.floor(x));
    return textG.pixels[idx + 3] > 10;
  };

  let minX = W, minY = H, maxX = 0, maxY = 0;
  for (let y = 0; y < H; y += 4) {
    for (let x = 0; x < W; x += 4) {
      if (inside(x,y)) {
        if (x < minX) minX = x;
        if (x > maxX) maxX = x;
        if (y < minY) minY = y;
        if (y > maxY) maxY = y;
      }
    }
  }

  const nearAir = (x, y, r=EDGE_CLEAR) =>
    !inside(x + r, y) || !inside(x - r, y) ||
    !inside(x, y + r) || !inside(x, y - r);

  // Focos interiores (letrero)
  bulbsFill = [];
  for (let y = minY + EDGE_CLEAR; y <= maxY - EDGE_CLEAR; y += BULB_STEP) {
    for (let x = minX + EDGE_CLEAR; x <= maxX - EDGE_CLEAR; x += BULB_STEP) {
      if (!inside(x, y)) continue;
    
      if (nearAir(x, y, EDGE_CLEAR-2)) continue;
      bulbsFill.push({x, y});
    }
  }

  bulbsEdge = [];
  const EDGE_STEP = BULB_STEP; // mismo paso para simplicidad
  for (let y = minY + 2; y <= maxY - 2; y += EDGE_STEP) {
    for (let x = minX + 2; x <= maxX - 2; x += EDGE_STEP) {
      if (!inside(x, y)) continue;
      if (nearAir(x, y, EDGE_CLEAR + 2)) bulbsEdge.push({x, y});
    }
  }
}

//fisica enchufe
function createCableAndPlug({ anchor, segments, segSize, plugSize }) {
  let prev = null;
  for (let i = 0; i < segments; i++) {
    const x = anchor.x + i * (segSize.w + 2);
    const y = anchor.y + i * 2 + 40;
    const seg = Bodies.rectangle(x, y, segSize.w, segSize.h, {
      friction: 0.7, density: 0.001
    });
    
    seg.render = { fillStyle: '#ffffff' };
    World.add(world, seg);

    if (i === 0) {
      World.add(world, Constraint.create({ pointA: { x: anchor.x, y: anchor.y }, bodyB: seg, length: 0, stiffness: 1 }));
    } else {
      World.add(world, Constraint.create({ bodyA: prev, bodyB: seg, length: segSize.w * 0.9, stiffness: 0.9 }));
    }
    prev = seg;
  }

  plug = Bodies.rectangle(prev.position.x + 36, prev.position.y + 2, plugSize.w, plugSize.h, {
    friction: 0.6, density: 0.002
  });
  plug.render = { fillStyle: '#ffffff' };
  World.add(world, plug);
  World.add(world, Constraint.create({ bodyA: prev, bodyB: plug, length: 28, stiffness: 0.9 }));
}

function drawWordBase() {
  
  push();
  fill(255, 255, 255, 10); // muy transparente
  noStroke();
  textAlign(CENTER, CENTER);
  textStyle(BOLD);
  textSize(220);
  text(WORD_TEXT, W/2, H/2 + 20);
  pop();
}

function drawWordGlow() {
  
  push();
  noFill();
  stroke(255, 220, 100, 40); strokeWeight(28);
  textAlign(CENTER, CENTER);
  textStyle(BOLD);
  textSize(220);
  text(WORD_TEXT, W/2, H/2 + 20);

  stroke(255, 240, 160, 24); strokeWeight(44);
  text(WORD_TEXT, W/2, H/2 + 20);
  pop();

  // refuerzo con puntos de borde
  for (const p of bulbsEdge) {
    noStroke();
    fill(255, 240, 150, 24); circle(p.x, p.y, BULB_DIAM * 2.4);
  }
}

function drawBulbs() {

  for (const p of bulbsFill) {
    if (powered) {
      noStroke();
      fill(255, 220, 90, 30); circle(p.x, p.y, BULB_DIAM * 2.8);
      fill(255, 240, 120, 60); circle(p.x, p.y, BULB_DIAM * 1.9);
      
      fill(255, 235, 140);
      stroke(220, 180, 70); strokeWeight(2);
      circle(p.x, p.y, BULB_DIAM);
    } else {
      
      noStroke();
      fill(70); circle(p.x, p.y, BULB_DIAM - 2);
      fill(30); circle(p.x, p.y, BULB_DIAM - 6);
    }
  }
}

function drawOutlet() {
  const x = outlet.position.x, y = outlet.position.y;
  push();
  rectMode(CENTER);
  
  stroke(160); fill(60);
  rect(x, y, 36, 62, 4);
  noStroke(); fill(200);
  rect(x - 7, y - 6, 4, 14, 2);
  rect(x + 7, y - 6, 4, 14, 2);
  fill(200); circle(x, y + 14, 6);
  pop();
}

function drawCable() {
  
  push(); noStroke(); fill(300);
  const bodies = Composite.allBodies(world);
  for (const b of bodies) {
    if (b === outlet || b === plug) continue;
    if (b.label !== 'Rectangle Body') continue;
    beginShape(); for (let v of b.vertices) vertex(v.x, v.y); endShape(CLOSE);
  }
  pop();
}

function drawPlug() {
  const b = plug;
  push();
  translate(b.position.x, b.position.y);
  rotate(b.angle);
  noStroke();
  fill(255); rectMode(CENTER); rect(0, 0, b.bounds.max.x - b.bounds.min.x, b.bounds.max.y - b.bounds.min.y, 5);
  // patitas (blancas)
  fill(230);
  rect(14, -6, 8, 4, 1);
  rect(14,  6, 8, 4, 1);

  fill(powered ? [255,100,100] : [120]); circle(-10, 0, 6);
  pop();
}

function isPlugTouchingOutlet() {
  if (!plug || !outlet) return false;
  const c = Collision.collides(plug, outlet);
  return c && c.collided;
}
```

**5. Captura y enlace**

<img width="815" height="483" alt="image" src="https://github.com/user-attachments/assets/25a04169-7790-4c95-b0a4-93b3b033c899" />


[Enlace sketch p5.js](https://editor.p5js.org/saragaravitop/sketches/6ip43iaiy)

[Enlace animación](https://upbeduco-my.sharepoint.com/:f:/g/personal/sara_garavito_upb_edu_co/EjH7kuhVu05EusH9GQ4vHmYBU93vsVQaS7tn16_BcRl3og?e=KPhf7y)

## Actoevaluación

Mi autoevaluación es 5 porque complete las 3 actividades e hice la autoevaluación (solo que no la monte jaja), específicamente, además de hacer las actividades completas profundice en comprender lo que estaba haciendo, además de generar un diseño que no fuera creado por la IA. jeje


