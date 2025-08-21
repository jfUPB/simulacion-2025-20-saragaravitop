# Unidad 3


## 🛠 Fase: Apply

### Actividad 10

1. Diseña e implementa tu obra generativa interactiva en tiempo real.
Mi obra consiste en un móvil generativo inspirado en Alexander Calder, donde múltiples piezas geométricas se balancean, interactúan entre sí y responden de acuerdo a la interacción del usuario.

* Interactividad: la obra es interactiva en tiempo real, al hacer clic con el mouse en la pantalla, se agregan nuevos pivotes en el lienzo. Estos pivotes funcionan como “ejes” a los que algunas piezas se reasignan con cierta probabilidad, modificando la estructura del móvil y generando nuevas configuraciones visuales.

* Algoritmos de la Unidad 1 usados:
Además del uso de random(), incorporé estos algoritmos:

  * Perlin Noise: utilizado para simular el viento suave que balancea continuamente las piezas. A diferencia de la aleatoriedad pura, el Perlin noise produce oscilaciones suaves y continuas que hacen que el movimiento sea más natural y orgánico.

  * Distribuciones de probabilidad ponderadas: para determinar el tipo de figura (círculo, triángulo, rectángulo), su tamaño y su color, empleé una selección basada en pesos. Por ejemplo, los colores primarios tienen mayor probabilidad de aparecer, mientras que los tamaños grandes aparecen con menor frecuencia. Esto me permite controlar la estética general de la obra y acercarme al lenguaje visual de Calder.

**2. Explica cómo modelaste el problema de los n-cuerpos en tu obra.**
El sistema fue modelado como un conjunto de piezas (cuerpos) y pivotes (centros de atracción), porque quería representar el movimiento natural de las abejas que se acercan a las flores de manera instintiva, por así decirlo, de esta forma. 

 * Cada pieza está conectada a un pivote mediante una fuerza tipo resorte que busca mantenerla a cierta distancia (longitud de barra). esto para simular el equilibrio físico de un móvil colgado.

 * Implemente una fuerza inversamente proporcional al cuadrado de la distancia entre piezas, gracias a esto, las piezas no se amontonan y el móvil conserva una distribución espacial armónica.

 * Viento (ruido Perlin): Se añade como una fuerza externa común a todos los cuerpos, logrando un balanceo continuo y dinámico.

De esta manera, cada pieza está influenciada por su pivote y por las demás piezas, lo que constituye un sistema de n-cuerpos interactuando en tiempo real.

**3. Copia el enlace a tu simulación en p5.js.**               
  
[Actividad 10 unidad 03 n-cuerpos](https://editor.p5js.org/saragaravitop/sketches/qR6volO8q)

**4. código.**      

```
let pieces = [];
let pivots = [];

function setup() {
  createCanvas(900, 520);
  angleMode(RADIANS);

  // Pivotes iniciales (como “brazos” superiores del móvil)
  for (let i = 0; i < 4; i++) {
    pivots.push(new Pivot(random(width * 0.2, width * 0.8), random(height * 0.15, height * 0.35)));
  }

  // Piezas iniciales colgadas en diferentes pivotes
  for (let i = 0; i < 80; i++) {
    const pvt = random(pivots);
    pieces.push(new Piece(random(width), random(height * 0.55, height * 0.9), pvt));
  }

  // Paleta “Calder”
  noStroke();
}

function draw() {
  background(238, 228, 206); // beige neutro de galería
  // Dibujar pivotes (negros discretos)
  for (const p of pivots) p.show();

  // Fuerzas n-cuerpos + viento
  for (let i = 0; i < pieces.length; i++) {
    const a = pieces[i];

    // Atracción tipo resorte hacia su pivote (como barra que busca equilibrio)
    a.attractedToPivot();

    // Repulsión entre piezas (evita amontonamiento)
    for (let j = i + 1; j < pieces.length; j++) {
      const b = pieces[j];
      repelPair(a, b, 120, 30); // radio/fortaleza
    }

    // “Viento” con Perlin noise (oscilación suave)
    a.wind();

    // Suavizado, límites y avance
    a.edges();
    a.update();
  }

  // Barras (líneas) + piezas
  for (const a of pieces) {
    a.showLink();
  }
  for (const a of pieces) {
    a.show();
  }
}

// Interacción: agregar pivote nuevo (no agregamos piezas)
function mousePressed() {
  pivots.push(new Pivot(mouseX, mouseY));
  // Opcional: reasignar algunas piezas al nuevo pivote por probabilidad
  for (const piece of pieces) {
    if (random() < 0.08) piece.pivot = random(pivots);
  }
}

class Pivot {
  constructor(x, y) {
    this.pos = createVector(x, y);
  }
  show() {
    push();
    fill(20);
    noStroke();
    circle(this.pos.x, this.pos.y, 6);
    pop();
  }
}

class Piece {
  constructor(x, y, pivot) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(0.5);
    this.acc = createVector(0, 0);
    this.pivot = pivot;

    // Distribuciones (pesos) para tipo, color y tamaño
    this.type = weightedChoice([
      ["circle", 0.55],
      ["triangle", 0.18],
      ["rect", 0.27],
    ]);

    this.size = weightedChoice([
      [random(10, 18), 0.45],
      [random(18, 28), 0.35],
      [random(30, 46), 0.20], // pocos grandes
    ]);

    this.col = weightedChoice([
      [color(244, 79, 57), 0.28],  // rojo
      [color(254, 203, 0), 0.28],  // amarillo
      [color(26, 89, 191), 0.22],  // azul
      [color(0), 0.12],            // negro
      [color(255), 0.10],          // blanco
    ]);

    this.maxSpeed = 3.0;
    this.noiseSeed = random(1000);
    this.rodLength = random(this.size * 2.2, this.size * 4.2); // “longitud de barra”
    this.rodFlex = random(0.035, 0.06); // cuánta “elasticidad” tiene el brazo
  }

  // Fuerza de resorte hacia una circunferencia de radio rodLength alrededor del pivote
  // (como si la pieza colgara de una barra que busca ese largo)
  attractedToPivot() {
    const dir = p5.Vector.sub(this.pos, this.pivot.pos);
    let d = dir.mag();
    if (d === 0) {
      dir.set(1, 0);
      d = 0.0001;
    }
    // error de longitud (preferimos d == rodLength)
    const stretch = d - this.rodLength;
    // fuerza proporcional (Hooke) hacia/desde el pivote
    dir.normalize();
    // pequeño torque lateral para sensación de balanceo
    const tangent = createVector(-dir.y, dir.x).mult(0.2 * sin(frameCount * 0.01 + this.noiseSeed));
    const f = p5.Vector.mult(dir, -this.rodFlex * stretch).add(tangent);
    this.acc.add(f);
  }

  // Perlin-wind: fuerza suave que cambia en el tiempo
  wind() {
    const nx = noise(this.noiseSeed + frameCount * 0.005);
    const ny = noise(this.noiseSeed + 1000 + frameCount * 0.005);
    // Mapear a [-1, 1] y escalar poco para no romper el equilibrio
    const wx = (nx * 2 - 1) * 0.25;
    const wy = (ny * 2 - 1) * 0.18;
    this.acc.x += wx;
    this.acc.y += wy * 0.6; // un poco más de componente vertical
  }

  edges() {
    // Fuerza suave hacia el centro si se acerca al borde (evita “pegarse”)
    const m = 40;
    if (this.pos.x < m) this.acc.x += 0.6;
    if (this.pos.x > width - m) this.acc.x -= 0.6;
    if (this.pos.y < m) this.acc.y += 0.6;
    if (this.pos.y > height - m) this.acc.y -= 0.6;
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed);
    this.pos.add(this.vel);
    // amortiguación suave (como rozamiento del aire)
    this.vel.mult(0.985);
    this.acc.mult(0);
  }

  showLink() {
    // Dibujar la “barra” entre pivot y pieza
    stroke(30);
    strokeWeight(2);
    line(this.pivot.pos.x, this.pivot.pos.y, this.pos.x, this.pos.y);
    noStroke();
  }

  show() {
    push();
    translate(this.pos.x, this.pos.y);
    // orientación: que apunte levemente según su velocidad
    rotate(this.vel.heading() * 0.25);

    fill(this.col);
    stroke(0);
    strokeWeight(1);

    const s = this.size;
    if (this.type === "circle") {
      circle(0, 0, s);
    } else if (this.type === "rect") {
      // rectángulo con esquinas suaves
      rectMode(CENTER);
      rect(0, 0, s * 1.2, s * 0.7, 3);
    } else if (this.type === "triangle") {
      triangle(-s * 0.6, s * 0.5, s * 0.7, 0, -s * 0.6, -s * 0.5);
    }

    pop();
  }
}

// Repulsión entre dos piezas (inversa al cuadrado)
function repelPair(a, b, radius = 120, k = 30) {
  const dir = p5.Vector.sub(a.pos, b.pos);
  const d = dir.mag();
  if (d > 0 && d < radius) {
    const strength = k / (d * d);
    dir.setMag(strength);
    a.acc.add(dir);
    b.acc.sub(dir);
  }
}

// Elección ponderada por pesos (distribuciones)
function weightedChoice(options) {
  // options: [[valor, peso], ...]
  let total = 0;
  for (const [, w] of options) total += w;
  let r = random(total);
  for (const [val, w] of options) {
    if (r < w) return val;
    r -= w;
  }
  return options[options.length - 1][0];
}
```
**5. Captura una imagen representativa de tu ejemplo.**              
<img width="652" height="473" alt="image" src="https://github.com/user-attachments/assets/adc31bcc-c630-490b-8ddf-9cc15a0b8af0" />



