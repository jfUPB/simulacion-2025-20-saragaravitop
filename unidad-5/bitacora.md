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
  
**2. como gestione la creación y desaparición de particulas**

La creación se hace con emisores que llaman a addParticles(), y lleva un conteo con un tope para dejar de emitir a tiempo. La desaparición se hace con lifespan y splice para eliminarlas del array. 

**3. Explicar qué concepto aplique, cómo lo aplique y por qué.**
   * unidad 1: ruido perlín. Usé noise() para generar un ángulo suave para mover el origen del emisor, lo usé para que de una variación coherente, y se vea fluido y natural la creación de las particulas de acuerdo al clic.
   * unidad 2: aceleración aleatoria. cada particula recibe una aceleración, esto añade una turbulencia a los trayectos de estas sin necesidad de usar una fuerza externa.
   * unidad 5: multiples sistemas. Se creo un array global con varios particleSystem, y son los que controlan la creación y desaparición de las particulas. 

**4. [Enlace](https://editor.p5js.org/saragaravitop/sketches/tASAacjfH)**
   
**5. Código**

particle.js
```js
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

```js
let systems = [];

// Ajustes por defecto para nuevos sistemas (puedes cambiarlos con teclas)
let defaults = {
  perlinMove: true,     
  randAccelMag: 0.09,   
  emissionRate: 3,     
  maxBirths: 300        
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

**Experimento**           

**1. Conceptos usados**  

   * unidad 1: distribución normal
   * unidad 2: aceleración personalizada
   * unidad 4: funciones sinusoudes, coordenadas polares
   * unidad 5: sistema extensible 

**2. como gestione la creación y desaparición de particulas**

con un sistema que crea las particulas, las cuales solo se forman si hay espacio, es decir, si no ha llegado al maximo, y si se lleno se pausa la producción. Las particulas mueren con el lifespan que va bajando hasta llegar a 0, y cuando tambien se llena, se van eliminando. 

**3. Explicar qué concepto aplique, cómo lo aplique y por qué.**
   * unidad 1: distribución normal. La use para la variabilidad en el angulo de la velocidad inicial, con randomGaussian(), y mostrar las medias. 
   * unidad 2: aceleración personalizada. lo hice para separar el comportamiento global de la particula, asi se mantiene el polimorfismo. 
   * unidad 4: funciones sinusoudes, coordenadas polares. esta la usé para aportar variedad de forma visual para no romper el sistema. 
   * unidad 5: sistema extensible. para lo mismo que explique arriba. 

**4. [Enlace](https://editor.p5js.org/saragaravitop/sketches/vlxOahLqy)**

**5. Código**
particle.js
```js
// particle.js
// Base + subclases (polimorfismo) y sistema extensible con registro
// Incluye: flores a color (solo visual), maxBirths por sistema, y gating canEmit.

class Particle {
  constructor(x, y, opts = {}) {
    this.pos = createVector(x, y);
    const meanDir = -HALF_PI;                         // apuntando hacia arriba
    const ang = randomGaussian(meanDir, PI / 10);     // desviación angular
    const mag = max(0.1, randomGaussian(2.0, 0.6));   // magnitud con gaussiana
    this.vel = p5.Vector.fromAngle(ang).mult(mag);
    this.acc = createVector(0, 0)
    this.size = constrain(randomGaussian(9, 3), 3, 22);
    this.lifespan = constrain(randomGaussian(255, 40), 120, 320);
    this.phase = random(TWO_PI);
    this.customAccel = opts.customAccel || null;
    this.hue = random([330, 300, 260, 200, 140, 40]); // tonos florales (HSB)
    this.petalCount = floor(random(6, 10));           // nº de pétalos
    this.spin = random(-0.02, 0.02);                  // giro sutil
  }

  applyForce(f) { this.acc.add(f); }

  update() {
    // aceleración personalizada por frame
    if (this.customAccel) {
      const a = this.customAccel(this);
      if (a) this.applyForce(a);
    }
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.vel.mult(0.995);
    this.lifespan -= 2.2;
  }
  display() {
    push();
    translate(this.pos.x, this.pos.y);
    const base = this.size;
    const petalLen = base * 1.6;     // largo del pétalo
    const petalWid = base * 0.8;     // ancho del pétalo
    const rot = this.phase + frameCount * this.spin;
    noStroke();
    for (let i = 0; i < this.petalCount; i++) {
      const a = rot + (TWO_PI * i) / this.petalCount;
      push();
      rotate(a);
      const h = (this.hue + i * (12 / this.petalCount)) % 360; // leve variación
      fill(h, 70, 95, this.lifespan); // HSB global (definido en setup)
      ellipse(base * 0.6, 0, petalLen, petalWid);
      pop();
    }
    fill((this.hue + 20) % 360, 40, 60, this.lifespan);
    circle(0, 0, base * 0.9);
    pop();
  }

  run() { this.update(); this.display(); }
  isDead() { return this.lifespan <= 0; }
}

class SineParticle extends Particle {
  constructor(x, y, opts = {}) {
    super(x, y, opts);
    this.sinAmp = opts.sinAmp ?? 0.06;    // amplitud de a_senoidal
    this.sinFreq = opts.sinFreq ?? 0.12;  // frecuencia (rad/frame)
  }
  update() {
    // aceleración lateral senoidal (perpendicular a la vel actual)
    const dir = this.vel.copy();
    if (dir.magSq() > 0) {
      dir.normalize();
      const perp = createVector(-dir.y, dir.x);
      const a = this.sinAmp * sin(this.phase + frameCount * this.sinFreq);
      this.applyForce(perp.mult(a));
    }
    super.update();
  }
  display() {
    // brillo pulsa con seno (ajustado a HSB global)
    const pulse = map(sin(this.phase + frameCount * 0.1), -1, 1, 0.6, 1.0);
    const b = map(pulse, 0.6, 1.0, 80, 100);
    noStroke();
    fill(220, 30, b, this.lifespan); // azul suave en HSB
    circle(this.pos.x, this.pos.y, this.size);
  }
}
class PolarParticle extends Particle {
  constructor(x, y, opts = {}) {
    super(x, y, opts);
    this.anchor = createVector(x, y);
    this.theta = random(TWO_PI);
    this.r = random(2, 8);
    // velocidades polares con distribución normal
    this.omega = randomGaussian(0.04, 0.015);  // dθ/dt
    this.rVel = randomGaussian(0.15, 0.05);    // dr/dt
  }
  update() {
    // dinámica polar con modulación senoidal en el radio
    this.theta += this.omega;
    this.r += this.rVel + 0.35 * sin(this.phase + frameCount * 0.05);
const x = this.anchor.x + this.r * cos(this.theta);
    const y = this.anchor.y + this.r * sin(this.theta);
    // aproxima vel para compatibilidad con display base
    this.vel.set(x - this.pos.x, y - this.pos.y);
    this.pos.set(x, y);
    this.lifespan -= 2.0;
  }
  display() {
    noStroke();
    fill(0, 0, 80, this.lifespan); // gris en HSB
    circle(this.pos.x, this.pos.y, this.size);
  }
}
class ParticleSystem {
  constructor(x, y, options = {}) {
    this.origin = createVector(x, y);
    this.particles = [];
    this.registry = [
      { cls: Particle,      weight: 1 },
      { cls: SineParticle,  weight: 1 },
      { cls: PolarParticle, weight: 1 }
   }
    this.emissionRate = options.emissionRate ?? 4;
    this.maxBirths = options.maxBirths ?? 350; // ← tope local por sistema
    this.births = 0;
    this.accelMode = options.accelMode ?? 'none';
    this.canEmit = true;
  }

  // API pública para extender tipos en caliente
  register(cls, weight = 1) {
    this.registry.push({ cls, weight: max(0, weight) });
  }

  // selector ponderado
  pickClass() {
    const total = this.registry.reduce((s, r) => s + r.weight, 0);
    let t = random(total);
    for (const r of this.registry) {
      if ((t -= r.weight) < 0) return r.cls;
    }
    return Particle;
  }

  // aceleración personalizada por modo (mantiene referencias léxicas)
  customAccel = (p) => {
    switch (this.accelMode) {
      case 'gravity':
        return createVector(0, 0.08);
      case 'swirl': {
        const c = createVector(width / 2, height / 2);
        const toC = p5.Vector.sub(c, p.pos);
        if (toC.magSq() === 0) return createVector(0, 0);
        const perp = createVector(-toC.y, toC.x).normalize().mult(0.03);
        return perp;
      }
      case 'mouse': {
        const m = createVector(mouseX, mouseY);
        return p5.Vector.sub(m, p.pos).setMag(0.05);
      }
      default:
        return createVector(0, 0);
    }
  }

  addParticles() {
    if (!this.canEmit || this.births >= this.maxBirths) return; // ← gating global + tope local
    for (let i = 0; i < this.emissionRate; i++) {
      const K = this.pickClass();
      this.particles.push(new K(this.origin.x, this.origin.y, {
        customAccel: this.customAccel
      }));
      this.births++;
      if (this.births >= this.maxBirths) break;
    }
  }

  run() {
    this.addParticles();
    for (let i = this.particles.length - 1; i >= 0; i--) {
      const p = this.particles[i];
      p.run();
      if (p.isDead()) this.particles.splice(i, 1);
    }
    push();
    noFill();
    stroke(0, 0, 100, 100); // blanco en HSB con alfa
    circle(this.origin.x, this.origin.y, 10);
    pop();
  }

  isDead() {
    return this.births >= this.maxBirths && this.particles.length === 0;
  }
}
```
sketch.js
```js
let systems = [];
let defaults = {
  emissionRate: 5,
  accelMode: 'none' // 'none' | 'gravity' | 'swirl' | 'mouse'
};

// ← Presupuesto global de partículas vivas
const PARTICLE_BUDGET = 3500;

function setup() {
  createCanvas(720, 480);
  textFont('monospace', 12);

  // Mover colorMode global aquí (antes estaba por partícula)
  colorMode(HSB, 360, 100, 100, 255);

  // sistema inicial
  systems.push(new ParticleSystem(width / 2, height * 0.75, {
    emissionRate: defaults.emissionRate,
    accelMode: defaults.accelMode
  }));
}

function draw() {
  background(14);

  // 1) Contar partículas vivas globalmente
  let live = 0;
  for (const s of systems) live += s.particles.length;
  const canEmit = live < PARTICLE_BUDGET;

  // 2) Ejecutar sistemas con gating de emisión
  for (const s of systems) {
    s.canEmit = canEmit; // pausa emisión si superamos el presupuesto
    s.emissionRate = defaults.emissionRate;
    s.accelMode = defaults.accelMode;
    s.run();
  }

  // HUD (dibujar encima)
  noStroke();
  fill(0, 0, 100); // blanco en HSB
  text(`sistemas: ${systems.length} | emisión: ${defaults.emissionRate}/frame | accel: ${defaults.accelMode}`, 12, 20);
  text(`vivas: ${live} / ${PARTICLE_BUDGET} | [click] nuevo sistema | [1] none [2] gravity [3] swirl [4] mouse | [E/D] +/- emisión`, 12, 40);
}

function mousePressed() {
  systems.push(new ParticleSystem(mouseX, mouseY, {
    emissionRate: defaults.emissionRate,
    accelMode: defaults.accelMode
  }));
}

function keyPressed() {
  const k = key.toLowerCase();
  if (k === 'e') defaults.emissionRate = min(defaults.emissionRate + 1, 20);
  if (k === 'd') defaults.emissionRate = max(defaults.emissionRate - 1, 1);

  if (key === '1') defaults.accelMode = 'none';
  if (key === '2') defaults.accelMode = 'gravity';
  if (key === '3') defaults.accelMode = 'swirl';
  if (key === '4') defaults.accelMode = 'mouse';
}
```

**6. Captura** 

<img width="610" height="435" alt="image" src="https://github.com/user-attachments/assets/64f3d304-4d54-4892-9025-786ac8161099" />

#### 4. ejemplo 4.6 a particle system with forces.

¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?      
> en este ejemplo tambien usan emisores para la creación de particulas, el emisor les da una posición fija. a las particulas se les puede aplicar una fuerza antes de actualizar su movimiento. Nuevamente se usa el lifespan, pero en esta ocasión va disminuyendo con el tiempo y cuando finalmente llega a 0, se eliminan las particulas usando el splice.

**Experimento **

**1. Conceptos usados**

* unidad 1: caminatas aleatorias
* unidad 2: aceleración constante
* unidad 3: modelar fuerzas/leyes de newton 
* unidad 5: sistema físico

**2. como gestione la creación y desaparición de particulas**

la generación y creación de particulas se hace tal cual como en la original, lo unico que hice fue agregar los momentos en que ciertas fuerzas afectan ciertas particulas, el efecto solo se ve en el siguiente sistema que se cree. 

**3. Explicar qué concepto aplique, cómo lo aplique y por qué.**
* unidad 1: caminatas aleatorias. Usé random(), para crear un generador e incluir la aceleración aleatoria para aplicarla a cada partícula. 
* unidad 2: aceleración constante. Se agregaban fuerzan equivalentes a la aceleración, como el viento o la gravedad, para darle mas dinamismo a la obra.
* unidad 3: modelar fuerzas/leyes de newton. Lo que hice con esto fue generar mas control en los movimientos de las particulas al generar un atractor 
* unidad 5: sistema físico. Todos los conceptos que use arriba fue para generar mas sentido y coherencia con este sistema de particulas, porque entonces podia aplicar diferentes fuerzas en distintos momentos, como si fuera un switch. 

**4. [Enlace](https://editor.p5js.org/saragaravitop/sketches/Fl5HZMwoY)

**5. Código**

particle.js
```js
class Particle {
  constructor(x, y, opts = {}) {
    this.pos = createVector(x, y);
    this.mass = constrain(randomGaussian(1.0, 0.25), 0.4, 2.0);
    const ang = random(-PI/6, PI/6) - HALF_PI; // mayormente hacia arriba
    const mag = constrain(randomGaussian(2.0, 0.6), 0.2, 4.0);
    this.vel = p5.Vector.fromAngle(ang).mult(mag);
    this.acc = createVector(0, 0);
    this.size = map(this.mass, 0.4, 2.0, 5, 12);
    this.lifespan = 255;
  }
  applyForce(F) {
    const a = p5.Vector.div(F, this.mass);
    this.acc.add(a);
  }

  update() {
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.vel.mult(0.995); // leve amortiguación numérica
    this.lifespan -= 2.2;
  }

  display() {
    push();
    translate(this.pos.x, this.pos.y);
    noStroke();
    // color por masa (solo visual)
    fill(map(this.mass, 0.4, 2.0, 220, 30), 70, 95, this.lifespan);
    circle(0, 0, this.size);
    pop();
  }

  run() { this.update(); this.display(); }
  isDead() { return this.lifespan <= 0; }
}
class ConstantAccel {
  constructor(ax, ay) {
    this.a = createVector(ax, ay);
  }
  applyTo(p) {
    // F = m·a  (para que la aceleración resultante sea igual en todas las masas)
    const F = p5.Vector.mult(this.a, p.mass);
    p.applyForce(F);
  }
}

// U3: Arrastre (aire/agua) cuadrático: Fd = -k * |v|^2 * v̂
class Drag {
  constructor(k = 0.02) { this.k = k; }
  applyTo(p) {
    const v = p.vel.copy();
    const speed = v.mag();
    if (speed === 0) return;
    const dragMag = this.k * speed * speed;
    const Fd = v.copy().mult(-1).normalize().mult(dragMag);
    p.applyForce(Fd);
  }
}

// U3: Atractor puntual con ley inversa al cuadrado (suavizada con min/max)
class Attractor {
  constructor(centerVecRef, G = 20, minR = 10, maxR = 300) {
    // centerVecRef: referencia a un p5.Vector externo que puede moverse
    this.centerRef = centerVecRef;
    this.G = G; this.minR = minR; this.maxR = maxR;
  }
  applyTo(p) {
    const dir = p5.Vector.sub(this.centerRef, p.pos);
    const d2 = constrain(dir.magSq(), this.minR * this.minR, this.maxR * this.maxR);
    const strength = (this.G * p.mass) / d2; // M=1
    dir.normalize().mult(strength);
    p.applyForce(dir);
  }
}

// U1: Caminata aleatoria como fuerza de “ruido blanco” en aceleración.
// a_rw es la magnitud de aceleración objetivo; aplicamos F = m * a_rw * û
class RandomWalkForce {
  constructor(a_rw = 0.05) { this.a = a_rw; }
  applyTo(p) {
    const aRand = p5.Vector.random2D().setMag(this.a);
    const F = aRand.mult(p.mass);
    p.applyForce(F);
  }
}
class ParticleSystem {
  constructor(x, y, options = {}) {
    this.origin = createVector(x, y);
    this.particles = [];
    this.generators = [];
    this.emissionRate = options.emissionRate ?? 5;
    this.maxBirths = options.maxBirths ?? Infinity;
    this.births = 0;
  }

  // API de sistema físico: añadir fuerzas
  addForceGenerator(gen) {
    this.generators.push(gen);
  }

  addParticles() {
    if (this.births >= this.maxBirths) return;
    for (let i = 0; i < this.emissionRate; i++) {
      this.particles.push(new Particle(this.origin.x, this.origin.y));
      this.births++;
      if (this.births >= this.maxBirths) break;
    }
  }

  run() {
    this.addParticles();
    for (const p of this.particles) {
      for (const gen of this.generators) gen.applyTo(p);
      p.run();
    }
    for (let i = this.particles.length - 1; i >= 0; i--) {
      if (this.particles[i].isDead()) this.particles.splice(i, 1);
    }
    push();
    noFill(); stroke(0, 0, 100, 100);
    circle(this.origin.x, this.origin.y, 10);
    pop();
  }

  isDead() {
    return this.births >= this.maxBirths && this.particles.length === 0;
  }
}
```

sketch.js 
```js
let systems = [];
let defaults = {
  emissionRate: 6,
  useGravity: true,
  useWind: false,
  useRandomWalk: true,
  useDrag: true,
  useAttractor: false
};

// Atractor compartido (referencia viva que leen los generadores)
let attractorPos;

function setup() {
  createCanvas(720, 480);
  textFont('monospace', 12);
  colorMode(HSB, 360, 100, 100, 255);

  attractorPos = createVector(width / 2, height / 2);

  // Crea un sistema inicial
  systems.push(makeSystem(width * 0.5, height * 0.75));
}

function draw() {
  background(14);

  // Mueve el atractor al mouse si está activo y el mouse se mantiene presionado
  if (defaults.useAttractor && mouseIsPressed) {
    attractorPos.set(mouseX, mouseY);
  }

  // Corre los sistemas
  for (let i = systems.length - 1; i >= 0; i--) {
    systems[i].run();
    if (systems[i].isDead()) systems.splice(i, 1);
  }
  noStroke(); fill(0, 0, 100);
  text(sistemas: ${systems.length} | emite: ${defaults.emissionRate}/f
[g] gravedad: ${onOff(defaults.useGravity)}  [w] viento: ${onOff(defaults.useWind)}  [r] random-walk: ${onOff(defaults.useRandomWalk)}
[d] drag: ${onOff(defaults.useDrag)}  [a] atractor: ${onOff(defaults.useAttractor)}  [E/D] +/- emisión  |  click: nuevo sistema`,
    12, 20
  );

  // Visual del atractor (si está activo)
  if (defaults.useAttractor) {
    push();
    noFill(); stroke(200, 80, 100, 160);
    circle(attractorPos.x, attractorPos.y, 16);
    pop();
  }
}

// Crear un sistema y conectar fuerzas según flags (U5: sistema físico)
function makeSystem(x, y) {
  const ps = new ParticleSystem(x, y, { emissionRate: defaults.emissionRate });

  // U2: aceleraciones constantes
  if (defaults.useGravity)  ps.addForceGenerator(new ConstantAccel(0, 0.12));
  if (defaults.useWind)     ps.addForceGenerator(new ConstantAccel(0.02, 0));

  // U1: caminata aleatoria
  if (defaults.useRandomWalk) ps.addForceGenerator(new RandomWalkForce(0.05));

  // U3: fuerzas físicas
  if (defaults.useDrag)       ps.addForceGenerator(new Drag(0.02));
  if (defaults.useAttractor)  ps.addForceGenerator(new Attractor(attractorPos, 40, 12, 500));

  return ps;
}

function mousePressed() {
  systems.push(makeSystem(mouseX, mouseY));
}

function keyPressed() {
  const k = key.toLowerCase();
  if (k === 'g') defaults.useGravity = !defaults.useGravity;
  if (k === 'w') defaults.useWind = !defaults.useWind;
  if (k === 'r') defaults.useRandomWalk = !defaults.useRandomWalk;
  if (k === 'd') defaults.useDrag = !defaults.useDrag;
  if (k === 'a') defaults.useAttractor = !defaults.useAttractor;

  if (k === 'e') defaults.emissionRate = min(defaults.emissionRate + 1, 20);
  if (k === 'd') defaults.emissionRate = max(defaults.emissionRate - 1, 1);

  // Actualiza la emisión de los sistemas existentes (fuerzas nuevas se aplican a los nuevos sistemas)
  for (const s of systems) s.emissionRate = defaults.emissionRate;
}

function onOff(b) { return b ? 'ON' : 'off'; }
```

**6. Captura**

<img width="645" height="432" alt="image" src="https://github.com/user-attachments/assets/c566c695-8df4-44f6-b9b9-a19e0cb7cc7b" />


#### 5. ejemplo 4.7: a particle system with a repeller.

¿Cómo se está gestionando la creación y la desaparción de las partículas y cómo se gestiona la memoria en cada una de las simulaciones?      
> para la creación de partículas tambien se usa un emisor, con posición, velocidad aceleración y con el lifespan, que se agregan al array interno del sistema. En este caso, se agregan fuerzas y un repeller, que modifican la aceleración de cada particula, lo que afecta la trayectoria.
> El lifespan va disminuyendo en cada frame, hasta que llega a 0 y la particula se considera muerta, ya el sistema recorre el array y elimina las que ya no están activas. 

Experimento 

**1. Conceptos usados**

**2. como gestione la creación y desaparición de particulas**

**3. Explicar qué concepto aplique, cómo lo aplique y por qué.**

**4. [Enlace]()

**5. Código**

**6. Captura**



