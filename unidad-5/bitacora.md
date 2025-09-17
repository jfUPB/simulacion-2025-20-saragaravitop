# Evidencias de la unidad 5

### Actividad 02

#### 1. ejemplo 4.2: an array of particles

¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?
> La creación de las particulas se da en draw(), se crea una nueva instancia de la clase Particle. En el origin es donde nacen las particulas (como un punto fijo). Cada particula tiene su posición, velocidad, aceleración y una vida util (para que si aparezcan y desaparezcan). Y el array es el contenedor donde se guarda toda esta información.
> Las particulas desaparecen con el lifespan, cuando ese valor llega a 0, o menos se mueren las particulas. En el arreglo, si isDead es true, la particula ya no tiene vida útil (para asi mantener el bucle sin que se rompa), y splice, la elimina de la memoria.

**Experimento**
**1. conceptos usados:**
    * unidad 1: distribución de probabilidad
    * unidad 2: motion 101
    * unidad 5: sistema de partículas básico 

**2. como gestione la creación y desaparición de particulas**
creé una nueva instancia de BeeParticle en el centro del lienzo (es el panal). Y la desaparición la hice con el lifespan y el splice, que eliminan a las particulas dentro del array bees.

**3. Explicar qué concepto aplique, cómo lo aplique y por qué.**
   * unidad 1: distribución de probabilidad. Usé random() para asignar las velocidades iniciales y los colores de cada abeja. Lo usé para poder simular trayectorias mas naturales y que se viera aleatorio. 
    * unidad 2: motion 101. Lo usé para asignarle a cada abeja una posición, velocidad y aceleración para moder modelar este movimiento físico realista. 
    * unidad 5: sistema de partículas básico. Implementé un array para la creación y desapación de las abejitas.

**4. [Enlace](https://editor.p5js.org/saragaravitop/sketches/E2QRkb0iH)**   

**5. Código**

```js
let bees = [];

function setup() {
  createCanvas(600, 400);
  angleMode(DEGREES);
}

function draw() {
  background(255, 240, 200); // Color cálido tipo panal

  // Dibujar el panal (punto de origen)
  fill(255, 204, 0);
  stroke(180, 140, 0);
  strokeWeight(2);
  hexagon(width / 2, height / 2, 20);

  // Crear una nueva abeja cada frame
  bees.push(new BeeParticle(createVector(width / 2, height / 2)));

  // Actualizar y eliminar abejas muertas
  for (let i = bees.length - 1; i >= 0; i--) {
    bees[i].run();
    if (bees[i].isDead()) {
      bees.splice(i, 1);
    }
  }
}

// Clase base con movimiento y vida útil
class ParticleBase {
  constructor(position) {
    this.position = position.copy();
    this.velocity = createVector(random(-1, 1), random(-2, -0.5));
    this.acceleration = createVector(0, 0.05);
    this.lifespan = 255;
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
    this.lifespan -= 2;
  }

  isDead() {
    return this.lifespan < 0;
  }

  run() {
    this.update();
    this.display();
  }
}

class BeeParticle extends ParticleBase {
  constructor(position) {
    super(position);
    this.phase = random(360);
    this.size = random(6, 10);
    this.bodyColor = color(random(220, 255), random(180, 200), 0); /
    this.stripeColor = color(50); // Negro
  }

  update() {
    super.update();
    let oscillation = sin(this.phase + frameCount * 4) * 1.5;
    this.position.x += oscillation;
  }

  display() {
    push();
    translate(this.position.x, this.position.y);
    rotate(sin(this.phase + frameCount * 4) * 5); // Zumbido leve
    noStroke();
    fill(this.bodyColor);
    hexagon(0, 0, this.size);
    stroke(this.stripeColor);
    strokeWeight(1);
    line(-this.size / 2, 0, this.size / 2, 0);
    line(-this.size / 3, -this.size / 3, this.size / 3, -this.size / 3);
    line(-this.size / 3, this.size / 3, this.size / 3, this.size / 3);

   pop();
  }
}

function hexagon(x, y, radius) {
  beginShape();
  for (let a = 0; a < 360; a += 60) {
    let sx = x + cos(a) * radius;
    let sy = y + sin(a) * radius;
    vertex(sx, sy);
  }
  endShape(CLOSE);
}
```
**6. Captura**

<img width="546" height="369" alt="image" src="https://github.com/user-attachments/assets/30a61e4e-9cba-464f-a1e9-9d78232f0515" />

#### 2. ejemplo 4.4 a system of systems

¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?   
> Ls creación de las particulas se da por medio de un sistema, que se llama emisor, el cual tiene su propio origen, este método crea una nueva instancia y la agrega al array interno del emisor. Como funciona cada que se hace click en la pantalla, si hay varios emisores activos, al hacer clic en diferentes partes del lienzo se generan particulas de forma independiente.
> en este caso, la desaparición de las particulas tambien se da por medio de un lifespan, el cual, cuando llega a 0, se muere la particula, y el emisor recorre el array para eliminar las que ya no estan activas.


**Experimento**
**1. conceptos usados:**
    * unidad 1: ruido perlín
    * unidad 2: aceleración aleatoria
    * unidad 5: múltiples sistemas
  
**2. como gestione la creación y desaparición de particulas****
La creación se hace con emisores que llaman a addParticles(), y lleva un conteo con un tope para dejar de emitir a tiempo. La desaparición se hace con lifespan y splice para eliminarlas del array. 

**3. Explicar qué concepto aplique, cómo lo aplique y por qué.**
   * unidad 1: ruido perlín. Usé noise() para generar un ángulo suave para mover el origen del emisor, lo usé para que de una variación coherente, y se vea fluido y natural la creación de las particulas de acuerdo al clic.
   * unidad 2: aceleración aleatoria. cada particula recibe una aceleración, esto añade una turbulencia a los trayectos de estas sin necesidad de usar una fuerza externa.
   * unidad 5: multiples sistemas. Se creo un array global con varios particleSystem, y son los que controlan la creación y desaparición de las particulas. 

**4. [Enlace](https://editor.p5js.org/saragaravitop/sketches/tASAacjfH)**
   
**5. Código**

particle.js
```
class Particle {
  constructor(x, y, randAccelMag = 0.09) {
    this.pos = createVector(x, y);
    this.vel = p5.Vector.random2D().mult(random(0.3, 1.2));
    this.acc = createVector(0, 0);
    this.lifespan = 255;
    this.size = random(6, 10);
    this.randAccelMag = randAccelMag; // Unidad 2: magnitud del "empujón" aleatorio por frame
  }

  applyForce(f) {
    // masa = 1
    this.acc.add(f);
  }

  update() {
    // Unidad 2: aceleración aleatoria suave por cuadro (random walk amortiguado)
    if (this.randAccelMag > 0) {
      const aRand = p5.Vector.random2D().mult(this.randAccelMag);
      this.applyForce(aRand);
    }
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.vel.mult(0.995);
    this.lifespan -= 2.2;
  }

  show() {
    noStroke();
    fill(200, 220, 255, this.lifespan);
    circle(this.pos.x, this.pos.y, this.size);
  }

  run() {
    this.update();
    this.show();
  }

  isDead() {
    return this.lifespan <= 0;
  }
}

class ParticleSystem {
  constructor(x, y, opts = {}) {
    // Estado
    this.origin = createVector(x, y);
    this.particles = [];
    this.perlinMove   = opts.perlinMove   ?? true;  
    this.randAccelMag = opts.randAccelMag ?? 0.09;  
    this.emissionRate = opts.emissionRate ?? 3;     
    this.maxBirths    = opts.maxBirths    ?? 300;   
    this.births = 0;
    this._t  = random(1000);
    this._dt = 0.01;   // velocidad de avance en el ruido
    this._step = 1.8;  // longitud del paso por frame
  }

  // Unidad 1: mover el origen con un campo de ángulos derivado de noise()
  moveOriginWithPerlin() {
    const ang = noise(this._t) * TWO_PI * 2.0;  // mapea [0,1] a [0, 4π)
    const step = p5.Vector.fromAngle(ang).mult(this._step);
    this.origin.add(step);
    this._t += this._dt;
    this.origin.x = constrain(this.origin.x, 0, width);
    this.origin.y = constrain(this.origin.y, 0, height);
  }

  addParticles() {
    if (this.births >= this.maxBirths) return; // deja de emitir cuando alcanza el cupo
    for (let i = 0; i < this.emissionRate; i++) {
      this.particles.push(new Particle(this.origin.x, this.origin.y, this.randAccelMag));
      this.births++;
      if (this.births >= this.maxBirths) break;
    }
  }

  run() {
    // 1) mover origen con Perlin (Unidad 1)
    if (this.perlinMove) this.moveOriginWithPerlin();
    this.addParticles();
    for (let i = this.particles.length - 1; i >= 0; i--) {
      const p = this.particles[i];
      p.run();
      if (p.isDead()) this.particles.splice(i, 1);
    }
    push();
    noFill();
    stroke(255, 120);
    circle(this.origin.x, this.origin.y, 10);
    pop();
  }

  isDead() {
    // “muere” cuando ya no emite y no quedan partículas activas
    return this.births >= this.maxBirths && this.particles.length === 0;
  }
}
```
sketch.js 

```
let systems = [];

// Ajustes por defecto para nuevos sistemas (puedes cambiarlos con teclas)
let defaults = {
  perlinMove: true,     // Unidad 1: origen con ruido Perlin
  randAccelMag: 0.09,   // Unidad 2: aceleración aleatoria por partícula
  emissionRate: 3,      // cuántas partículas por frame
  maxBirths: 300        // límite de nacimientos por sistema
};

function setup() {
  createCanvas(720, 480);
  textFont('monospace', 12);
}

function draw() {
  background(12);

  // Recorremos de atrás hacia adelante por si removemos
  for (let i = systems.length - 1; i >= 0; i--) {
    const s = systems[i];
    s.run();
    if (s.isDead()) systems.splice(i, 1); // Unidad 5: remover sistemas vacíos
  }

  // HUD simple
  noStroke();
  fill(255);
  text(`emisores: ${systems.length}`, 12, 20);
  text(`clic: nuevo emisor | [N]: Perlin on/off | [R/F]: +/− azar | [E/D]: +/− emisión | [X]: limpiar`, 12, 40);
}

function mousePressed() {
  // Crear un nuevo sistema en el mouse con los "defaults" actuales
  systems.push(new ParticleSystem(mouseX, mouseY, {
    perlinMove: defaults.perlinMove,
    randAccelMag: defaults.randAccelMag,
    emissionRate: defaults.emissionRate,
    maxBirths: defaults.maxBirths
  }));
}

// Controles para experimentar en vivo
function keyPressed() {
  const k = key.toLowerCase();

  if (k === 'n') {
    defaults.perlinMove = !defaults.perlinMove;
  } else if (k === 'r') {
    defaults.randAccelMag = min(defaults.randAccelMag + 0.02, 0.35);
  } else if (k === 'f') {
    defaults.randAccelMag = max(defaults.randAccelMag - 0.02, 0);
  } else if (k === 'e') {
    defaults.emissionRate = min(defaults.emissionRate + 1, 12);
  } else if (k === 'd') {
    defaults.emissionRate = max(defaults.emissionRate - 1, 1);
  } else if (k === 'x') {
    systems = [];
  }
}
```
**6. Captura**
<img width="611" height="436" alt="image" src="https://github.com/user-attachments/assets/4e5193ab-4637-41aa-aeba-283edeeb4a3a" />

#### 3. ejemplo 4.5: a Particle System with Inheritance and Polymorphism.

¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?      
> la creación de partículas en este ejemplo funciona en que en cada frame se añade una nueva particula al sistema, que puede ser de cualquier subclase. Cada particula tiene el metodo isDead que es el que indica cuando ya esten muertas y se usa splice para eliminarlas. Esto se logra de manera mas rapida y eficiente por el uso de herencia y polimorfismo.

#### 4. ejemplo 4.6 a particle system with forces.

¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?      
> en este ejemplo tambien usan emisores para la creación de particulas, el emisor les da una posición fija. a las particulas se les puede aplicar una fuerza antes de actualizar su movimiento. Nuevamente se usa el lifespan, pero en esta ocasión va disminuyendo con el tiempo y cuando finalmente llega a 0, se eliminan las particulas usando el splice.

#### 5. ejemplo 4.7: a particle system with a repeller.

¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?      
> para la creación de partículas tambien se usa un emisor, con posición, velocidad aceleración y con el lifespan, que se agregan al array interno del sistema. En este caso, se agregan fuerzas y un repeller, que modifican la aceleración de cada particula, lo que afecta la trayectoria.
> El lifespan va disminuyendo en cada frame, hasta que llega a 0 y la particula se considera muerta, ya el sistema recorre el array y elimina las que ya no están activas. 



