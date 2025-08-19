# Unidad 3


## 🛠 Fase: Apply

### Actividad 10

1. Diseña e implementa tu obra generativa interactiva en tiempo real.
Diseñé una obra inspirada en el problema de los n-cuerpos, representando abejas que vuelan en busca de flores. Cada abeja siente atracción hacia las flores, pero al mismo tiempo se repelen entre sí para no amontonarse. Esto genera un comportamiento colectivo parecido al de un enjambre.

* Interactividad: al hacer clic en el lienzo, aparece una nueva flor y también se agregan nuevas abejas al sistema, así, el enjambre crece dinámicamente y cambia su organización en tiempo real.

* Algoritmos de la Unidad 1 usados:
  * random
  * perlin noise 

**2. Explica cómo modelaste el problema de los n-cuerpos en tu obra.**
En la obra cada abeja se considera un "cuerpo" que experimenta fuerzas de atracción hacia las flores (simulando el polen), y de repulsión entre sí (para no chocar). Entonces, cada elemento del sistema influye en el movimiento de los demás, dando como resultado un enjambre dinámico, inspirado también en las esculturas cinéticas de Alexander Calder, ya que la obra nunca se repite igual y depende de la interacción con el espectador.

**3. Copia el enlace a tu simulación en p5.js.**               
  
[Actividad 10 unidad 03 n-cuerpos](https://editor.p5js.org/saragaravitop/sketches/qR6volO8q)

**4. código.**      

```
let bees = [];
let flowers = [];

function setup() {
  createCanvas(800, 400);
  
  for (let i = 0; i < 120; i++) {
    bees.push(new Bee(random(width), random(height)));
  }

  for (let i = 0; i < 5; i++) {
    flowers.push(createVector(random(width), random(height)));
  }
}

function draw() {
  background(240, 220, 180);
  
  noStroke();
  fill(255, 100, 150);
  for (let f of flowers) {
    ellipse(f.x, f.y, 30, 30);
  }
  
  for (let bee of bees) {

    for (let f of flowers) {
      bee.attracted(f);
    }

    for (let other of bees) {
      if (bee !== other) {
        bee.repel(other.pos);
      }
    }
    bee.update();
    bee.show();
  }
}

function mousePressed() {
  flowers.push(createVector(mouseX, mouseY));
}

class Bee {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D();
    this.acc = createVector();
    this.maxSpeed = 3;
  }

  attracted(target) {
    let force = p5.Vector.sub(target, this.pos);
    let d = force.mag();
    d = constrain(d, 10, 300); 

    let strength = 50 / (d * d); 
    force.setMag(strength); 
    this.acc.add(force);
  }
  
  repel(other) {
    let force = p5.Vector.sub(this.pos, other);
    let d = force.mag();
    if (d < 50 && d > 0) { 
      let strength = 10 / (d * d); 
      force.setMag(strength); 
      this.acc.add(force);
    }
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(this.maxSpeed);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  show() {
    fill(255, 200, 0);
    stroke(0);
    ellipse(this.pos.x, this.pos.y, 12, 12);
  }
}
```
**5. Captura una imagen representativa de tu ejemplo.**              
<img width="673" height="411" alt="image" src="https://github.com/user-attachments/assets/c18ba197-ddd3-42cd-8fe6-4b826da40acc" />

