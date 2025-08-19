# Unidad 3

## 🔎 Fase: Set + Seek

### Actividad 09

#### Fuerza: gravitacional 

**1. Explica cómo modelaste la fuerza.**         
especificamente esta, la modele como un torque proporcional a `-sin(angle) * gravedad`. Esto simula cómo la gravedad intenta devolver a la bailarina a la posición vertical cuando está inclinada, similar a un péndulo, cuanto mayor es el ángulo de inclinación, mayor es la fuerza que actúa para regresarla.
   
**2. Conceptualmente cómo se relaciona la fuerza con la obra generativa.**          
la fuerza de gravitación conceptualmente se trata de un objeto que es atraido por otro, en este caso, la bailarina al estar sobre un solo pie y con el otro elevado hacia arriba (pasé abierto), pierde el equilibrio natural que tenemos al estar parados en los dos pies, por ende, comienza a ser atraida hacia el piso, entonces por eso en la obra, cada que clickeo la pantalla, la bailarina cae, no delo todo porque naturalmente si pasara en la vida real, pongo el otro pie y vuelvo a la posición, entonces por eso vuelve al punto de equilibrio como un péndulo.
   
**3. Copia el enlace a tu ejemplo en p5.js.**      
 [actividad 09 unidad 03 gravitacional](https://editor.p5js.org/saragaravitop/sketches/Ds-HFZNLK)
   
**4. Copia el código.**       
```
let bailarina;

function setup() {
  createCanvas(400, 400);
  bailarina = new Ballerina(width / 2, height / 2 + 100);
}

function draw() {
  background(255);

  let gravedad = 0.002; 
  let torque = -sin(bailarina.angle) * gravedad;
  bailarina.applyTorque(torque);

  bailarina.update();
  bailarina.display();
}

function mousePressed() {
  bailarina.applyTorque(random(-0.02, 0.02));
}

class Ballerina {
  constructor(x, y) {
    this.origin = createVector(x, y); 
    this.angle = 0;
    this.angVel = 0;
    this.angAcc = 0;
    this.damping = 0.99; 
    this.maxAngle = radians(40); 
  }

  applyTorque(torque) {
    this.angAcc += torque;
  }

  update() {
    this.angVel += this.angAcc;
    this.angVel *= this.damping;
    this.angle += this.angVel;

    if (this.angle > this.maxAngle) {
      this.angle = this.maxAngle;
      this.angVel *= -0.3;
    } else if (this.angle < -this.maxAngle) {
      this.angle = -this.maxAngle;
      this.angVel *= -0.3;
    }

    this.angAcc = 0;
  }

  display() {
    push();
    translate(this.origin.x, this.origin.y);
    rotate(this.angle);

    stroke(0);
    strokeWeight(10);
    line(0, 0, 0, -80);
    
    fill(255, 0, 0);
    stroke(0);
    strokeWeight(6);
    line(0, -80, 0, -140);

    stroke(0);
    strokeWeight(8);
    line(0, -80, 30, -100);
    line(30, -100, 0, -120); 

    fill(200, 0, 100);
    noStroke();
    ellipse(0, -120, 50, 20);

    fill(255, 220, 200);
    noStroke();
    ellipse(0, -160, 30, 30);

    stroke(0);
    strokeWeight(6);
    line(0, -130, -30, -160);
    line(0, -130, 30, -160);

    pop();
  }
}
 ```
  
**5. Captura una imagen representativa de tu ejemplo.**
   
<img width="555" height="326" alt="image" src="https://github.com/user-attachments/assets/6d3b3479-4787-4f37-9f13-79a84f765c24" />

#### Fuerza: fricción 

**1. Explica cómo modelaste la fuerza.**      
la modele de tal forma que cuando la pelota está en contacto con el piso, se aplica un vector horizontal opuesto a la dirección del movimiento, donde μ es distinto para cada piso. En el código, esto está en `applyGroundFriction()`, y es lo que hace que en algunos pisos se frene mucho y en otros casi no se frene.

**2. Conceptualmente cómo se relaciona la fuerza con la obra generativa.**     
conceptualmente, la fricción, es una fuerza que se opone al movimiento, y para un objeto que esta en movimiento, dependiendo del medio y del material de este, puede ir más lento o más rápido. en la obra es una pelota (que se mueve con las flechas del teclado), que dependiendo del piso (pasto, arena, hielo, mágico (uno liso)), la pelota va más rápido o más despacio. Por ejemplo, en el mágico va mucho mas rápido que en el hielo, en la arena se frena casi complemente y en el pasto va mas lento pero no tanto como en la arena. 

**3. Copia el enlace a tu ejemplo en p5.js.**       
[actividad 09 unidad 03 fricción](https://editor.p5js.org/saragaravitop/sketches/yysFgIR-1)

**4. Copia el código.**      

```
let mover;
let pisos = [];
const R = 20; 

function setup() {
  createCanvas(700, 450);
  mover = new Mover(width / 2, 80, 1);

  pisos = [
    { y: 380, h: 30, mu: 0.25, c: color(205, 170, 125), name: "Arena (μ=0.25)" },
    { y: 300, h: 30, mu: 0.05, c: color(180, 220, 255), name: "Hielo (μ=0.05)" },
    { y: 220, h: 30, mu: 0.15, c: color(120, 200, 120), name: "Pasto (μ=0.15)" },
    { y: 140, h: 30, mu: 0.00, c: color(230),           name: "Piso mágico (μ=0)" }
  ];
}

function draw() {
  background(255);

  noStroke();
  textAlign(LEFT, BOTTOM);
  textSize(12);
  for (let p of pisos) {
    fill(p.c);
    rect(0, p.y, width, p.h);
    fill(40);
    text(p.name, 10, p.y - 4);
  }

  // Controles (empuje horizontal)
  if (keyIsDown(LEFT_ARROW))  mover.applyForce(createVector(-mover.thrust, 0));
  if (keyIsDown(RIGHT_ARROW)) mover.applyForce(createVector( mover.thrust, 0));
  mover.applyGravity();

  mover.update();
  mover.resolvePlatforms(pisos);
  mover.applyGroundFriction();
  mover.show();
}

function keyPressed() {
  if (keyCode === UP_ARROW) {
    mover.changeLevel(-1, pisos); // subir
  } else if (keyCode === DOWN_ARROW) {
    mover.changeLevel(1, pisos);  // bajar
  }
}

function mousePressed() {
  mover.jumpToClosestPlatform(mouseY, pisos);
}

class Mover {
  constructor(x, y, mass = 1) {
    this.position = createVector(x, y);
    this.prevPos  = this.position.copy();
    this.velocity = createVector(0, 0);
    this.accel    = createVector(0, 0);
    this.mass     = mass;

    this.g      = 0.6;
    this.thrust = 0.4;  // menor empuje → fricción se nota más

    this.onGround = false;
    this.groundY  = null;
    this.groundMu = 0;
  }

  applyForce(f) {
    this.accel.add(p5.Vector.div(f, this.mass));
  }

  applyGravity() {
    this.applyForce(createVector(0, this.mass * this.g));
  }

  update() {
    this.prevPos.set(this.position);
    this.velocity.add(this.accel);
    this.position.add(this.velocity);
    this.accel.mult(0);

    if (this.position.x < R) {
      this.position.x = R; this.velocity.x = 0;
    } else if (this.position.x > width - R) {
      this.position.x = width - R; this.velocity.x = 0;
    }
  }

  resolvePlatforms(pisos) {
    this.onGround = false;
    this.groundMu = 0;
    this.groundY  = null;

    for (let p of pisos) {
      const top = p.y;
      const wasAbove = this.prevPos.y + R <= top;
      const isBelowOrTouching = this.position.y + R >= top;
      const falling = this.velocity.y >= 0;

      if (wasAbove && isBelowOrTouching && falling) {
        this.position.y = top - R;
        this.velocity.y = 0;
        this.onGround = true;
        this.groundMu = p.mu;
        this.groundY  = top;
      }
    }

    const floorTop = height;
    if (!this.onGround && this.position.y + R > floorTop) {
      this.position.y = floorTop - R;
      this.velocity.y = 0;
      this.onGround = true;
      this.groundMu = 0.25;
      this.groundY  = floorTop;
    }

    if (this.onGround && this.groundY !== null) {
      this.position.y = this.groundY - R;
    }
  }

  applyGroundFriction() {
    if (!this.onGround) return;
    if (abs(this.velocity.x) < 1e-3) return;

    let friction = createVector(-this.velocity.x, 0);
    friction.normalize();
    const frictionMag = this.groundMu * this.g * 2.5; // multiplicador → efecto visible
    friction.setMag(frictionMag);
    this.applyForce(friction);
  }
  changeLevel(dir, pisos) {
    // dir = -1 (subir), 1 (bajar)
    let currentIndex = pisos.findIndex(p => p.y === this.groundY);
    if (currentIndex === -1) return;

    let targetIndex = currentIndex + dir;
    if (targetIndex >= 0 && targetIndex < pisos.length) {
      this.position.y = pisos[targetIndex].y - R;
      this.velocity.y = 0;
      this.onGround = true;
      this.groundMu = pisos[targetIndex].mu;
      this.groundY  = pisos[targetIndex].y;
    }
  }
  jumpToClosestPlatform(mouseY, pisos) {
    let closest = null;
    let minDist = Infinity;
    for (let p of pisos) {
      let d = abs(mouseY - p.y);
      if (d < minDist) {
        minDist = d;
        closest = p;
      }
    }
    if (closest) {
      this.position.y = closest.y - R;
      this.velocity.y = 0;
      this.onGround = true;
      this.groundMu = closest.mu;
      this.groundY  = closest.y;
    }
  }

  show() {
    noStroke();
    fill(0, 0, 0, 25);
    const shadowY = (this.onGround && this.groundY) ? this.groundY - 6 : this.position.y + R;
    ellipse(this.position.x, shadowY, 36, 10);

    fill(120, 60, 200);
    stroke(30);
    strokeWeight(1);
    ellipse(this.position.x, this.position.y, R * 2, R * 2);
  }
}
```

**5. Captura una imagen representativa de tu ejemplo.**         
<img width="669" height="428" alt="image" src="https://github.com/user-attachments/assets/b2043f50-7a99-4bad-812a-682149596f28" />










