# Unidad 3

## 🔎 Fase: Set + Seek

### Actividad 09

#### Fuerza: gravitacional 

**1. Explica cómo modelaste la fuerza.**         
especificamente esta, la modele como un torque proporcional a `-sin(angle) * gravedad`. Esto simula cómo la gravedad intenta devolver a la bailarina a la posición vertical cuando está inclinada, similar a un péndulo, cuanto mayor es el ángulo de inclinación, mayor es la fuerza que actúa para regresarla.
   
**2. Conceptualmente cómo se relaciona la fuerza con la obra generativa.**          
la fuerza de gravitación conceptualmente se trata de un objeto que es atraido por otro, en este caso, la bailarina al estar sobre un solo pie y con el otro elevado hacia arriba (pasé abierto), pierde el equilibrio natural que tenemos al estar parados en los dos pies, por ende, comienza a ser atraida hacia el piso, entonces por eso en la obra, cada que clickeo la pantalla, la bailarina cae, no delo todo porque naturalmente si pasara en la vida real, pongo el otro pie y vuelvo a la posición, entonces por eso vuelve al punto de equilibrio como un péndulo.
   
**3. Copia el enlace a tu ejemplo en p5.js.**      
 [actividad 09](https://editor.p5js.org/saragaravitop/sketches/Ds-HFZNLK)
   
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
