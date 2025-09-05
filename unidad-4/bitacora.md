# Evidencias de la unidad 4

## Explicación conceptual de la obra

* ¿Qué concepto de la unidad 4 y cómo lo aplicaste en la obra?
> Usé el concepto de ondas, especificamente las que tienen la función sinusoide. Este movimiento es aplicado a las estrellas que se mueven en el eje Y, y de su brillo intermitente, además, usé esta misma función para crear un degradado de color en las estrellas mientras se mueven a lo largo de su trayectoria de onda, mezclando dos colores de manera suave.

* ¿Qué concepto de la unidad 3 y cómo lo aplicaste en la obra?
> Usé el concepto de la fuerza gravitacional junto con los n-bodies aplicado en las estrellas fugaces, entonces cada que aparece x cantidad de estrellas se simula la fuerza de atracción, conforme las estrellas estan mas cerca se convierte en aceleración para cambiar la trayectoria de las estrellas, lo que hace que se curven y se acerquen entre sí en lugar de moverse en línea recta. 

* ¿Qué concepto de la unidad 2 y cómo lo aplicaste en la obra?
> Usé el Motion 101, para que los objetos se muevan porque su velocidad afecta su posición, y una aceleración (que representa una fuerza) cambia su velocidad. Agregué un bucle continuo que actualiza estos valores, creando el movimiento. 

* ¿Qué concepto de la unidad 1 y cómo lo aplicaste en la obra?
> Usé la aleatoriedad para que el movimiento de los objetos fuera diferente, marqué una posición y velocidad inicial de los objetos completamente al azar. Gracias al desplazamiento aleatorio, las estrellas parpadean y se mueven de forma ondulada (unidad 4). 

> Use la aleatoriedad con probabilidades para el movimiento de las ondas. 

## ¿Cómo resolviste la interacción?
> La interacción es dada por el mouse. Cuando se acerca el mouse a la pantalla, las estrellas siguen el movimiento de este solo cuando esta en el lienzo, y adicionalmente, cada que se hace click se agrega una estrella fugaz. 

## Enlace a la obra en el editor de p5.js

[Aquí está mi obra](https://editor.p5js.org/saragaravitop/sketches/xt1lvJ0ia)

## Código de la obra 

``` js
// --- Algoritmic Universe v4 

let objetos = [];

function setup() {
  createCanvas(800, 400);

  // Crear estrellas
  for (let i = 0; i < 50; i++) {
    objetos.push(new Espacial("estrella"));
  }
  // Crear planetas
  for (let i = 0; i < 10; i++) {
    objetos.push(new Espacial("planeta"));
  }
}

function draw() {
  background(10); // Fondo negro fijo

  // Aplica la fuerza gravitacional solo entre las estrellas fugaces
  let fugaces = objetos.filter(o => o.tipo === "fugaz");
  for (let i = 0; i < fugaces.length; i++) {
    for (let j = 0; j < fugaces.length; j++) {
      if (i !== j) {
        let force = fugaces[j].attract(fugaces[i]);
        fugaces[i].applyForce(force);
      }
    }
  }

  for (let o of objetos) {
    if (o.tipo === "estrella") {
      if (mouseX > 0 && mouseX < width && mouseY > 0 && mouseY < height) {
        let mouse = createVector(mouseX, mouseY);
        let dir = p5.Vector.sub(mouse, o.pos);
        dir.normalize();
        dir.mult(0.1 * sin(frameCount * 0.05)); // influencia ondulante
        o.acc = dir;
      } else {
        o.acc = createVector(0, 0);
      }
    }
    o.update();
    o.display();
  }

  objetos = objetos.filter(o => !o.isDead());
}

function mousePressed() {
  objetos.push(new Espacial("fugaz"));
}

class Espacial {
  constructor(tipo) {
    this.tipo = tipo;
    this.pos = createVector(random(width), random(height));
    this.vel = p5.Vector.random2D().mult(random(0.5, 2));
    this.acc = createVector(0, 0);
    this.size = (this.tipo === "planeta") ? random(20, 50) : 8;
    this.lifetime = (this.tipo === "fugaz") ? 400 : Infinity;
    this.topspeed = 6;
    
    // Masa proporcional al tamaño. Las fugaces tienen una masa pequeña para no afectar tanto.
    this.mass = (this.tipo === "fugaz") ? this.size * 0.8 : this.size; 



    // offsets para personalizar cada estrella
    this.phaseOffset = random(TWO_PI);
    this.blinkSpeed = random(0.05, 0.15);

    // Nuevas propiedades de color
    if (this.tipo === "planeta") {
      let r = random(150, 220);
      let g = random(150, 220);
      let b = random(150, 220);
      this.color = color( (r+g)/2, (g+b)/2, (b+r)/2, 200 );
    } else if (this.tipo === "estrella") {
      this.baseColor = color(255, 230, 180);
    } else if (this.tipo === "fugaz") {
      this.trail = [];
    }
  }
  
  // Método para aplicar una fuerza
  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acc.add(f);
  }

  // Método para calcular la fuerza de atracción
  attract(other) {
    let G = 0.8; // Constante gravitacional (ajustada para un efecto más visible en las fugaces)
    let force = p5.Vector.sub(this.pos, other.pos);
    let distance = force.mag();
    distance = constrain(distance, 5, 25);
    let strength = (G * this.mass * other.mass) / (distance * distance);
    force.setMag(strength);
    return force;
  }


  update() {
    
    this.vel.add(this.acc);
    this.vel.limit(this.topspeed);
    if (this.tipo === "estrella") {
      this.vel.add(this.acc);
      this.vel.limit(this.topspeed);

      let wave = sin(frameCount * 0.05 + this.phaseOffset) * 10;
      this.pos.add(this.vel.x, this.vel.y + wave);
    } else {
      this.vel.add(this.acc);
      this.pos.add(this.vel);
    }
    this.acc.mult(0);
    
    if (this.tipo === "fugaz") {
      this.lifetime--;
      this.trail.push(createVector(this.pos.x, this.pos.y));
      if (this.trail.length > 30) {
        this.trail.shift();
      }
    }
    this.checkEdges();
  }

  display() {
    noStroke();
    if (this.tipo === "estrella") {
      let flicker = map(sin(frameCount * this.blinkSpeed + this.phaseOffset), -1, 1, 180, 255);
      let colorProgress = map(sin(frameCount * 0.05 + this.phaseOffset), -1, 1, 0, 1);
      let currentColor = lerpColor(this.baseColor, color(180, 220, 255), colorProgress);
      
      fill(red(currentColor), green(currentColor), blue(currentColor), flicker);
      push();
      translate(this.pos.x, this.pos.y);
      star(0, 0, 5, this.size, 5);
      pop();
    } else if (this.tipo === "planeta") {
      fill(this.color);
      circle(this.pos.x, this.pos.y, this.size);
    } else if (this.tipo === "fugaz") {
      for (let i = 0; i < this.trail.length; i++) {
        let p = this.trail[i];
        let alpha = map(i, 0, this.trail.length, 0, 180);
        let trailSize = map(i, 0, this.trail.length, 3, 10);
        fill(255, 200, 150, alpha);
        circle(p.x, p.y, trailSize);
      }

      fill(255, 240, 220, 255);
      push();
      translate(this.pos.x, this.pos.y);
      rotate(this.vel.heading() + PI/2);
      star(0, 0, 5, 12, 5);
      pop();
    }
  }

  checkEdges() {
    if (this.pos.x > width) this.pos.x = 0;
    if (this.pos.x < 0) this.pos.x = width;
    if (this.pos.y > height) this.pos.y = 0;
    if (this.pos.y < 0) this.pos.y = height;
  }

  isDead() {
    return (this.tipo === "fugaz" && this.lifetime <= 0);
  }
}

function star(x, y, radius1, radius2, npoints) {
  let angle = TWO_PI / npoints;
  let halfAngle = angle / 2.0;
  beginShape();
  for (let a = 0; a < TWO_PI; a += angle) {
    let sx = x + cos(a) * radius2;
    let sy = y + sin(a) * radius2;
    vertex(sx, sy);
    sx = x + cos(a + halfAngle) * radius1;
    sy = y + sin(a + halfAngle) * radius1;
    vertex(sx, sy);
  }
  endShape(CLOSE);
}
```

## Captura de pantalla representativa

<img width="728" height="366" alt="image" src="https://github.com/user-attachments/assets/e938a2e4-4fb3-4132-8018-d1707d5fc34f" />







